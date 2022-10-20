# rfc-net-20221020-1
https://github.com/o3de/sig-network/issues/77

> This proposes an updated version of an accepted RFC - [rfc-net-20220118-1-clientserver](./rfc-net-20220118-1-clientserver.md)

# What is the delta produced by this Amendment?
### Feature Design Description
The amendment proposes moving from a Client and Server target with a CMake target to allow Client to include Server to a Client, Server and Unified target. This is largely what was described as the alternative solution in the original RFC. The prior solution is now listed as the alternative.

### Technical Design Description
Example CMake was updated for accuracy and updated Feature Design Description.

### Macros
Added further details on macro usage and expected user interactions.

### Open Questions
Added a question around AutoGen usage in CMake for modules using Client/Server split

# RFC Feature: Client and server build target separation
### Summary:
For some O3DE applications, like multiplayer and live service games, server code cannot be shipped to the clients. Doing so creates incremental security and cheating risks. Servers that include client code and resources can also bring unnecessary dependencies which causes long build and relocation times. That being said, customers may also require a unified build containing client and server logic. This RFC proposes a hard client and server split by separating the client and server into different build targets when required while maintaining the option of a unified build for customers that require it.

### What is the relevance of this feature?
With the solution proposed by this RFC, O3DE will support the following two hosting modes for multiplayer applications:
1. (Headless Dedicated Server mode) One is for live service games, where headless dedicated servers can be packaged and distributed to on-prem/remote servers to provide reliable and stable hosting solutions. The dedicated server build omits the packages and modules that a headless server does not require and has a smaller compiled size. 
2. (Client-As-Server mode) Supporting Single Purchase products which cannot rely on subscription revenue to cover the costs of dedicated server hosting. In this mode, one of the clients acts as a server to which other players connect.

### Feature design description:
This solution proposes adding two compiler definitions to allow the separation of client and server behaviors:
- `AZ_TRAIT_SERVER_ENABLED`: Compile the server code for the current target.
- `AZ_TRAIT_CLIENT_ENABLED`: Compile the client code for the current target.

O3DE developers can use these compiler definitions to separate the client and server targets, so that they can create a dedicated server for live service games, a client with no server logic or a client hosted server for console titles. These will be referred to as a Server build, Client build and Unified build going forward.

### Technical design description:
#### CMake definitions
For any multiplayer O3DE project, as well as any dependent gems that contain multiplayer components, define separate static libraries for the Client, Server and Unified targets like below:

```
create target Multiplayer.Client.Static
    COMPILE_DEFINITIONS
        PUBLIC
            AZ_TRAIT_CLIENT_ENABLED=1
            AZ_TRAIT_SERVER_ENABLED=0

create target MultiplayerSample.Client.Static
    BUILD_DEPENDENCIES
        PRIVATE
            Gem::Multiplayer.Client.Static
            
create target Multiplayer.Server.Static
    COMPILE_DEFINITIONS
        PUBLIC
            AZ_TRAIT_CLIENT_ENABLED=0
            AZ_TRAIT_SERVER_ENABLED=1

create target MultiplayerSample.Servers.Static
    BUILD_DEPENDENCIES
       PRIVATE
           Gem::Multiplayer.Server.Static

create target Multiplayer.Unified.Static
    COMPILE_DEFINITIONS
        PUBLIC
            AZ_TRAIT_CLIENT_ENABLED=1
            AZ_TRAIT_SERVER_ENABLED=1

create target MultiplayerSample.Unified.Static
    BUILD_DEPENDENCIES
       PRIVATE
           Gem::Multiplayer.Unified.Static

if (PAL_TRAIT_BUILD_SERVER_SUPPORTED)
    # If developers need to build a server launcher, then add the project name to the list of server launcher projects
    set_property(GLOBAL APPEND PROPERTY LY_LAUNCHER_SERVER_PROJECTS MultiplayerSample)
endif()
```

All the downstream targets should always refer to one of the Client, Server or Unified static libraries.

Multiplayer gameplay components will live in both of the client and server targets to keep a simple cook and replication pipeline. Their behavior and accessors will be different because of the compile definitions and macros.

If O3DE developers only need the client hosted solution and don’t want to generate a server launcher, they can just remove the following lines in the CMake configuration:

```
if (PAL_TRAIT_BUILD_SERVER_SUPPORTED)
    # If developers need to build a server launcher, then add the project name to the list of server launcher projects
    set_property(GLOBAL APPEND PROPERTY LY_LAUNCHER_SERVER_PROJECTS MultiplayerSample)
endif()
```

#### Macros
Macros would be enforced at the [auto-generated component](https://o3de.org/docs/user-guide/networking/autopackets/) level. We should be adding additional `#if AZ_TRAIT_SERVER_ENABLED` and `#if AZ_TRAIT_CLIENT_ENABLED` markup around authority, server, autonomous, client properties and remote procedure calls (RPC), which will force derived components to respect those same defines.

Auto-generation provides a variety of logic overall. Some of this logic is declared and defined purely in AutoGen. Some however, is declared in AutoGen and defined by the user. RPC Handlers are an excellent example of this. In these cases, the declaration itself will be macro wrapped which will require users to macro wrap their definitions.

How to handle logic the user declares and defines that can be separated using Client/Server macros is left to the user's discretion.

### What are the advantages of the feature?
- Hard client and server separation can make O3DE multiplayer application more secure and compact.
- The hard split mode can be helpful to catch programmer errors. For example, when compiling the client only target, the compiler can catch errors where O3DE developers use server only network state on a client code path.

### What are the disadvantages of the feature?
- Testing/validating all permutations would multiply the number of builds on Jenkins. AR will have to pick the primary mode as default and validate the non-default permutation (client hosted use case) periodically during Jenkins nightly builds. Some breaking changes might pass AR and won’t be detected by Jenkins jobs immediately. It is suggested to validate the non-default permutation on one platform and configuration instead of doing this on 3 platforms (mac/linux/windows) with 3 configurations (debug/profile/release) and 2*2 permutations (monolithic/unity/non-monolithic-unity/non-unity).

### How will this be implemented or integrated into the O3DE environment?
* Identify the projects/gems that contain multiplayer components and split their client and server targets. Initial list this RFC targets on includes:
    * MultiplayerSample project
    * Multiplayer gem
    * AWS GameLift gem
* Add project and gem templates to O3DE. Any new multiplayer-dependent projects or gems will follow the same configurations proposed by this RFC.

### Are there any alternatives to this feature?
Using an extra CMake flag, remove the Unified solution and instead allow the Client configuration to function as the Client launcher or the Unified launcher based on said flag.

Project/Gem CMake configuration will look like below:

```
create target Multiplayer.Client.Static
    COMPILE_DEFINITIONS
        PUBLIC
            AZ_TRAIT_CLIENT_ENABLED=1
            AZ_TRAIT_SERVER_ENABLED=${LY_CLIENT_BEHAVES_AS_SERVER}

create target MultiplayerSample.Client
    BUILD_DEPENDENCIES
        PRIVATE
            Gem::Multiplayer.Client.Static
            
create target Multiplayer.Server.Static
    COMPILE_DEFINITIONS
        PUBLIC
            AZ_TRAIT_CLIENT_ENABLED=0
            AZ_TRAIT_SERVER_ENABLED=1

create target MultiplayerSample.Servers
    BUILD_DEPENDENCIES
       PRIVATE
           Gem::Multiplayer.Server.Static

if (PAL_TRAIT_BUILD_SERVER_SUPPORTED)
    # If developers need to build a server launcher, then add the project name to the list of server launcher projects
    set_property(GLOBAL APPEND PROPERTY LY_LAUNCHER_SERVER_PROJECTS MultiplayerSample)
endif()
```

This solution has the same advantages as the proposed solution. Disadvantages include:
- Toggling the CMake flag requires rebuilding the affected launcher
- Building all three types of launchers requires toggling the CMake flag at least once to update all builds
- Same compile definitions and testing/validating problems as the proposed solution.

### How will users learn this feature?
- Existing O3DE project (MultiplayerSample) and any gems that contain multiplayer components can be referenced as examples for the hard client and server separation.
- Specific project/gem template can be provided with the client and server target predefined as references.
- Document usage of the new compile definitions.
- Certain Multiplayer AutoGen based features will require using the new compile definitions.

### Are there any open questions?
- Lumberyard 1.x was WAF based and its [GridMate networking system](https://docs.aws.amazon.com/lumberyard/latest/userguide/network-intro.html) provided a macro solution for separating the client and server code. How could O3DE avoid messing up macros in most of the multiplayer-dependent classes as GridMate did?
    - Enforce macros at the auto-generated component level which results in quick compile errors when macros are misused in derived classes.
- For sig-build and sig-testing, what optimizations can be made to reduce the build time and simplify testing/validating on Jenkins when switch between these two hosting modes?
    - Create a common static library between server and client static targets that has the code shared between the two (that is not affected by the GENERATE_ defines) to reduce the amount of code that is compiled multiple times.
- How will AutoGen be affected by this feature?
    - A module generally allows one target to define AutoGen rules. This has historically been the Static target. Splitting into multiple static targets removes that option. A solution would be to provide an AutoGen target with Client and Server compile definitions not defined that all other Static targets inherit from.
