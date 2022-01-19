# rfc-net-20220118-1
* https://github.com/o3de/sig-network/issues/32

# RFC Feature: Client and server build target separation
### Summary:
For some O3DE applications, like multiplayer and live service games, server code cannot be shipped to the clients. Doing so creates incremental security and cheating risks. Servers that include client code and resources can also bring unnecessary dependencies which causes long build and relocation times. This RFC proposes a hard client and server split by separating the client and server into different build targets when required.

### What is the relevance of this feature?
With the solution proposed by this RFC, O3DE will support the following two hosting modes for multiplayer applications:
1. (Primary mode) One is for live service games, where headless dedicated servers can be packaged and distributed to on-prem/remote servers to provide reliable and stable hosting solutions. The dedicated server build omits the packages and modules that a headless server does not require and has a smaller compiled size. 
2. (Listen-server mode) Supporting Single Purchase products which cannot rely on subscription revenue to cover the costs of dedicated server hosting. In this mode, one of the clients acts as a server to which other players connect.

### Feature design description:
This solution proposes to add a new Boolean CMake flag `LY_CLIENT_BEHAVES_AS_SERVER` which indicates whether the client target can behave as a client server. If this flag is set to true, the generated binaries will satisfy the requirements of use case 2, and if false the binaries will satisfy the requirements of use case 1.

Two compiler definitions are also introduced to allow the separation of client and server behaviors:
- `AZ_TRAIT_SERVER_ENABLED`: Compile the server code for the current target.
- `AZ_TRAIT_CLIENT_ENABLED`: Compile the client code for the current target.

O3DE developers can use combination of these CMake flags and compiler definitions to separate the client and server targets, so that they can create either a dedicated server for live service games or client hosted server for console titles.

### Technical design description:
#### CMake definitions
For any multiplayer O3DE project, as well as any dependent gems that contain multiplayer components, define separate static libraries for the client and server targets like below:

```
create target MultiplayerSample.Client.static
    COMPILE_DEFINITIONS
        AZ_TRAIT_CLIENT_ENABLED=1
        AZ_TRAIT_SERVER_ENABLED=${LY_CLIENT_BEHAVES_AS_SERVER}
    BUILD_DEPENDENCIES
        PRIVATE
            Gem::Multiplayer.Client.Static

create target MultiplayerSample.Clients
    BUILD_DEPENDENCIES
        PRIVATE
            Gem::MultiplayerSample.Client.Static
            
if (LY_CLIENT_BEHAVES_AS_SERVER)
    MultiplayerSample.Server.static alias to MultiplayerSample.Client.static
else()
    create target MultiplayerSample.Server.static
        COMPILE_DEFINITIONS
            AZ_TRAIT_SERVER_ENABLED=1
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::Multiplayer.Server.Static
endif()

create target MultiplayerSample.Servers
    BUILD_DEPENDENCIES
       PRIVATE
           Gem::MultiplayerSample.Server.Static

if (PAL_TRAIT_BUILD_SERVER_SUPPORTED)
    # If developers need to build a server launcher, then add the project name to the list of server launcher projects
    set_property(GLOBAL APPEND PROPERTY LY_LAUNCHER_SERVER_PROJECTS MultiplayerSample)
endif()
```

All the downstream targets should always refer to the client or server static library.

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

### What are the advantages of the feature?
- Hard client and server separation can make O3DE multiplayer application more secure and compact.
- The hard split mode can be helpful to catch programmer errors. For example, when compiling the client only target, the compiler can catch errors where O3DE developers use server only network state on a client code path.

### What are the disadvantages of the feature?
- Changing `LY_CLIENT_BEHAVES_AS_SERVER` will require a rebuild of all the targets that are using it, which may result in long recompilation and compilation times.
- Testing/validating both permutations would multiply the number of builds on Jenkins. AR will have to pick the primary mode as default and validate the non-default permutation (client hosted use case) periodically during Jenkins nightly builds. Some breaking changes might pass AR and won’t be detected by Jenkins jobs immediately. It is suggested to validate the non-default permutation on one platform and configuration instead of doing this on 3 platforms (mac/linux/windows) with 3 configurations (debug/profile/release) and 2*2 permutations (monolithic/unity/non-monolithic-unity/non-unity).

### How will this be implemented or integrated into the O3DE environment?
* Identify the projects/gems that contain multiplayer components and split their client and server targets. Initial list this RFC targets on includes:
    * MultiplayerSample project
    * Multiplayer gem
    * AWS GameLift gem
* Add project and gem templates to O3DE. Any new multiplayer-dependent projects or gems will follow the same configurations proposed by this RFC.

### Are there any alternatives to this feature?
Add the same CMake flag/compile definitions as the proposed solution and also create the following three static libraries for the client hosted and dedicated server mode:

* MultiplayerSample.Unified.static: Unified static library for the client target that can also behave as a server.
* MultiplayerSample.Server.static: Static library for the dedicated server target which doesn’t include any client code.
* MultiplayerSample.Client.static: Static library for the client target which doesn’t include any server code.

Project/Gem CMake configuration will look like below:

```
if (LY_CLIENT_BEHAVES_AS_SERVER)
    create target MultiplayerSample.Unified.static
        COMPILE_DEFINITIONS
            GENERATE_SERVER=1
            GENERATE_CLIENT=1
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::Multiplayer.Unified.Static
    
    create target MultiplayerSample.Unified
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::MultiplayerSample.Unified.Static
else()
    create target MultiplayerSample.Server.static
        COMPILE_DEFINITIONS
            GENERATE_SERVER=1
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::Multiplayer.Server.Static
                
    create target MultiplayerSample.Server
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::MultiplayerSample.Server.Static

    create target MultiplayerSample.Client.static
        COMPILE_DEFINITIONS
            GENERATE_CLIENT=1
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::Multiplayer.Client.Static
                
    create target MultiplayerSample.Client
        BUILD_DEPENDENCIES
            PRIVATE
                Gem::MultiplayerSample.Client.Static
endif()

if (CMAKE_ALLOW_CLIENT_HOSTED)
    ly_create_alias(NAME Multiplayer.Clients NAMESPACE Gem TARGETS Gem::Multiplayer.Unified)
    ly_create_alias(NAME Multiplayer.Servers NAMESPACE Gem TARGETS Gem::Multiplayer.Unified)
else()
    ly_create_alias(NAME Multiplayer.Clients NAMESPACE Gem TARGETS Gem::Multiplayer.Client)
    ly_create_alias(NAME Multiplayer.Servers NAMESPACE Gem TARGETS Gem::Multiplayer.Server)
endif()

if (PAL_TRAIT_BUILD_SERVER_SUPPORTED)
    # If developers need to build a server launcher, then add the project name to the list of server launcher projects
    set_property(GLOBAL APPEND PROPERTY LY_LAUNCHER_SERVER_PROJECTS MultiplayerSample)
endif()
```

This solution has the same advantages as the proposed solution. Disadvantages include:
- Create a redundant unified target for the client hosted use case and complicate the CMake configuration.
- Same compile definitions and testing/validating problems as the proposed solution.

### How will users learn this feature?
- Existing O3DE project (MultiplayerSample) and any gems that contain multiplayer components can be referenced as examples for the hard client and server separation.
- Specific project/gem template can be provided with the client and server target predefined as references.
- Document usage of the new `LY_CLIENT_BEHAVES_AS_SERVER` flag and compile definitions.

### Are there any open questions?
- Lumberyard 1.x was WAF based and its [GridMate networking system](https://docs.aws.amazon.com/lumberyard/latest/userguide/network-intro.html) provided a macro solution for separating the client and server code. How could O3DE avoid messing up macros in most of the multiplayer-dependent classes as GridMate did?
    - Enforce macros at the auto-generated component level which results in quick compile errors when macros are misused in derived classes.
- For sig-build and sig-testing, what optimizations can be made to reduce the build time and simplify testing/validating on Jenkins when switch between these two hosting modes?
    - Create a common static library between server and client static targets that has the code shared between the two (that is not affected by the GENERATE_ defines) to reduce the amount of code that is compiled multiple times.
