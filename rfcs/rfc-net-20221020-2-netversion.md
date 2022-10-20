# rfc-net-20221020-2
https://github.com/o3de/sig-network/issues/81

# Summary
This feature will provide a way to detect when a multiplayer server and client are using different versions of multiplayer components. 

This RFC was created as a proposed fix for [github.com/o3de/o3de/issues/9669](http://github.com/o3de/o3de/issues/9669)
 
## What is the relevance of this feature?
As a developer I want to know if my server and client have a version mismatch.

It's important for servers and clients to be running the same version of all the multiplayer components (components which communicate to each other over the network). Any mismatch will lead to unexpected behavior. This feature will warn users immediately upon connecting to the server if there is a mismatch.

Example:
A developer wants to push a game update to players in the wild. One of the changes was made to a multiplayer auto-component, but the change was minimal; a developer reordered the auto-components RPCs inside the XML, so they were in alphabetical order (just because felt easier to read that way). Before pushing the update live, the developer tries testing the new build and starts a match by connecting to one of their production branch servers on the cloud. The game appears to be going smoothly, except the RPCs associated with this auto-component they reordered has seemed to have stopped working. Unbeknownst to the developer, this is because the server’s RPC index is different; although it felt minor at the time, reordering the RPCs inside the XML has broken the logic. The developer now needs to go and debug why the RPCs aren’t working as expected, which can take a long time if they don't realize the server and client are actually running different multiplayer auto-component versions.
After this feature is implemented, the developer would know right away that their client and server builds are different. Now upon connecting to the server, the client and server would share a hash-value representing the current snapshot of all their multiplayer components. If the hashes don’t match, an error message will be logged, and an error popup will be prominently displayed in the game's viewport. The developer will see the message, and know that he forgot to update the server.
Note: developers may want to add their own extensions to client/server "versioning" (ie region checks), this would instead be a feature for a custom game-specific handshake which is already supported.

## Feature design description:
The multiplayer system will be updated so that it can detect when a server and client are using different multiplayer auto-components. The server and client machines will then be warned of a mismatch, at which point the user can go and update their server or client builds to the latest build version. The hash generation, error detection, and reporting will all happen automatically without adding any extra steps to the developer's normal build process.


## Common example workflows:

1. *Local development*: There currently isn’t any client/server code split so accidentally mismatching game and server in this a local development environment is not really an issue. Whether building Editor, or GameLaunch, or ServerLauncher, the multiplayer auto-components are built as part of the game gem module and shared between all three apps. However, if client and server logic is split it may become more likely that a dev could forget to build the server after updating multiplayer components. Should this occur when developing locally, the dev will see a popup on their client app immediately upon trying to connect explaining that there’s a mismatch. “Warning: Server/Client Mismatch Detected!” The logs will include which exact components are mismatched: “Multiplayer Component mismatch includes NetPlayerController, NetCameraController, etc.”. At which point the dev will likely just rebuild both server and client to be sure. One question to ask is if there’s an opportunity to narrow down knowing whether it's the client or the server that needs rebuilding (ie which one is newer and which is older)? If Jinja also generated a timestamp during compilation, perhaps this build timestamp could be displayed along with the mismatch warning logs to provide better clues as to which build is older. 
2. *Developer testing with remote server*: A developer might want to try testing their game using a remote server, or a server on other developer’s machines to play together. After receiving the mismatch warnings, each developer would need to check their code source control to double-check who has latest. The dev with the older code will need to sync latest and rebuild. There may be an opportunity to improve this workflow using generic engine versioning; if each pull request increments a minor version number this extra information could be shared between machines and give clues as to which machine is older. However, is unlikely to help in a dev-to-dev play session since developers don’t modify their engine versions between local builds.
3. *Play testing with real players*: If players in the wild have a client version mismatch with a live server, the game logic should connect to the mismatch notification bus in order to receive a callback on the client when a mismatch occurs. Game specific logic can then be deployed to assist the client upgrade to the latest client build. Although this is unlikely to be necessary as the game logic should already be using custom handshake logic to ensure the client is using the proper game version.


## Technical design description:
In order for this feature not to impact normal developer workflows, we'll aim to have the normal build process automatically generate all of the code required for comparing multiplayer components. For this, we'll rely on AzCodeGen.

High-level overview:

1. AzAutoGen creates a unique 64-bit hash value for each auto-component xml it digests.
    1. The 64-bit hash will be stored in the ComponentData class that’s passed to the global MultiplayerComponentRegistry
2. Connection packets will be updated so that the server and the player can exchange their current holistic version hash (see Hash Generation (Per-Application) below).
3. During application start up, as all the gems are registering their components with the MultiplayerComponentRegistry, the MultiplayerComponentRegistry will combine each component’s hash to create its own 64-bit version hash.
4. The MultiplayerComponentRegistry’s version hash is sent from the client to the server as part of the client’s connection packet.
5. Server will compare the client’s version-hash with its own to make sure it matches
    1. If the server finds a mismatch:
        1. Error logs will be reported
        2. An EBus notification is broadcast
            1. Test automation can receive the event
            2. MultiplayerViewportMessageSystemComponent can receive the mismatch event and display a warning on-screen
        3. The version hash of each individual auto-component will be exchanged (using a new packet type) in order that the server and client know exactly which components are out-of-date.


Auto-component XML To Multiplayer Component Registry
[Image: image.png]*## Hash Method Available to AzCodeGen/Jinja:*
Ultimately we want to compare the server and client auto-component XMLs. Instead of having to send and receive 100's of kilobytes of XML files over the network, we'll instead parse the XML and create a unique hash. Parsing the XML and creating a hash can be achieved by leveraging the same AzCodeGen that's used to parse and convert XML into game components and packets. For converting XML into a 64-bit hash, a new method will be added to \cmake\AzAutoGen.py  called CreateHashValue64(string). It will return a AZ::HashValue64. Then in Jinja one can call the code as follows:
 {{ SomeStringRepresentingTheAutoComponent | createHashValue64string}}


## Hash Generation (Per-Auto-Component):
Auto-components contain a name, namespace, RPCs, RPC parameters, network properties, network inputs, and archetypes. Each of these properties would need to be considered when generating a hash. The auto-component classes would have a new member variable "AZ::HashValue64 m_versionHash" and a public accessor method "GetVersionHash()".

Creating the auto-component hash value will be done during AzCodeGen. The multiplayer auto-component Jinja will need to be updated to parse all the auto-component attributes (name, namespace, RPCs, RPC parameters, network properties, archetypes, and network inputs), store it in a string, pass it to the createHashValue64string method, and store it in m_versionHash.

Relevant Files: 
\Gems\Multiplayer\Code\Include\Multiplayer\AutoGen\AutoComponent_Header.jinja 
\Gems\Multiplayer\Code\Include\Multiplayer\AutoGen\AutoComponent_Source.jinja

## Hash Generation (Per-Application):
Instead of checking every auto-component on the server against every auto-component on the client, we will first compare a single holistic hash that’s generated per-application.
This hash, unlike the individual auto-component hash, will not be generated during AzCodeGen and instead will be produced at runtime.
Currently, at the start of the app, all of the auto-components register themselves with the global MultiplayerComponentRegistry. MultiplayerComponentRegistry will be updated so that as each component registers itself, the MultiplayerComponentRegistry will update its own AZ::HashValue64, by combining the component hashes together. (Note: A lazy init may be more appropriate here whereby the hash is generated once it’s asked for and then stored for future use)

\Gems\Multiplayer\Code\Include\Multiplayer\Components\MultiplayerComponentRegistry.h
class MultiplayerComponentRegistry
{
public:

 void RegisterMultiplayerComponent( ComponentData componentData );
...
private:
  AZ::HashValue64 m_versionHash;
}

\Gems\Multiplayer\Code\Include\Multiplayer\Components\MultiplayerComponentRegistry.cpp
void MultiplayerComponentRegistry::RegisterMultiplayerComponent( ComponentData componentData )
{
  ...
  // update m_versionHash by adding in componentData.m_versionHash;
}

## Sending and Receiving Hashes Between Client and Server

1. Update MultiplayerPackets::Connect packet to contain a new 64-bit hash value called “MultiplayerComponentVersion”. This will contain the holistic hash generated per-application.
2. Upon receiving the MultiplayerPackets::Connect packet, the server will check the MultiplayerComponentVersion against his own version, and send back an updated MultiplayerPackets::Accept packet which contains a boolean specifying if the hashes match.
    1. If the hash doesn't match:
        1. The server and client will exchange a new type of packet that contains the per-component hashes. This information which allows each to map auto-components and their version hash; this will allow us to quickly identify which auto-components were added, removed, or modified. Trigger EBus notifications, log warnings, and display viewport messages.
        2. The client/server should disconnect from one another; no player should be spawned.


Relevant Files:
\Gems\Multiplayer\Code\Source\MultiplayerSystemComponent.cpp\h
\Gems\Multiplayer\Code\Source\AutoGen\Multiplayer.AutoPackets.xml

## What are the advantages of the feature?
This feature will alert developers and players when their game doesn't match the server version. It's an important safeguard. By knowing which components don't match, developers can then check the code on either machine to see which is out-of-date.

## What are the disadvantages of the feature?
This feature will add to build times, although probably not much considering the new Jinja logic will only be a fraction of what’s being generated per auto-component. To be safe, the GitHub pull-request should include a comment about before and after build times using Multiplayer Sample. If parsing each auto-component by hand is too slow, maybe it's faster to turn the entire XML into a string in order to hash; but be careful not to consider non-essential characters (such as newlines) which can be different depending on the operating system.


## How will this be implemented or integrated into the O3DE environment?
This will be added as default behavior in the MultiplayerGem connect logic. Users of the gem will get the behavior for free, when they update the gem. Users who require more control may utilize the IsHandshakeComplete method which already exists in the IMultiplayer interface; users can specify what constitutes a complete handshake specific to their game’s requirements.


## Are there any alternatives to this feature?
1. Alternative Hash Generation (Per-Auto-Component and Per-Auto-Packet):
There's a possibility to hash the entire auto-component XML file instead of having to parse its individual auto-component properties to create a hash. We'd want to be careful that the internal XML data-structure passed into Jinja ignores newlines as these could produce different results based on a machine's operating system. For instance, if a Windows machine using CR/LF line endings connects to a Linux server using CR line endings, the hash function could incorrectly produce a different hash. The upside is that using an XML "ToString()" method would be computationally faster, and also quicker to implement (fewer lines of code and easier to read).

2. Leverage Existing Engine-Version API:
https://github.com/o3de/sig-core/issues/44
Instead of automatically generating versions for multiplayer components specifically, we could simply leverage the upcoming O3DE engine versioning API to send the current engine/project/gem versions. The problem with this is it likely won't catch any multiplayer component mismatches unless developers specifically remember to update versions after every change.

3. Instead of automatically generating versions, we could add a "version" field to the auto-component XML. The problem with this is it won't catch any multiplayer component mismatches unless developers specifically remembers to update versions after every change.

## How will users learn this feature?
Users will not need to learn of this feature in order to enable it as it will be enabled automatically.
Should a version mismatch occur, debug logs, and an on-screen warning message will appear on the client machine tipping off users as to the version mismatch, however, a detailed networking troubleshooting doc should cover what to do in order to fix a version mismatch.

## Out of Scope

1. Auto-packet Versioning: Versioning based on auto-packets out of scope. it would be highly unusual for a customer to alter the actual packets since that would involve editing core engine code and gems. An overall engine version # check would probably be more than sufficient here. The auto-component network properties, types, and RPCs and param types are what customers will be implementing and editing in their game code, and are most important to capture in this version check.


## Are there any open questions?
- Can this be turned off with a cvar switch? For debugging purposes, etc.?
- Are there solutions to knowing which application is out-of-date? Right now mismatch is known, but not which is right or wrong. 