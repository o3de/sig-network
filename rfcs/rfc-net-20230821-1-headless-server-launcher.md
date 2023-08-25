### Summary:
This RFC is to propose support for O3DE headless server launchers. Currently the server launcher is launched 'headlessly' by specifying the Atom null-renderer as either part of the command line or specified in a custom `.setreg` or `.cfg` file for the game project. When headless server mode is activated this way, the server launcher will simply suppress the creation of the client window and proceed with the server launcher process. However, to be a truly headless server, the determination of a graphical client vs a console application needs to be determined at compile time when the server launcher is built. While the behavior varies by platform, CMake requires that an executable be identified by the target type of either `APPLICATION` or `EXECUTABLE`. To make the server launcher truly headless, updates will be needed to refactor how the server launcher and related libraries are defined.

### What is the relevance of this feature?
The general use case for dedicated game servers do not need any graphical client since there is no user interaction necessary. Server applications normally just need to launch and shutdown. Launching is done through executing the application, and shutdown can be done through a terminate signal (CTRL-C) or a kill request. Being able to build a true headless server launcher will remove the requirement of having a GUI manager installed on the machine that will host the server.

### Feature design description:
To maintain backwards compatibility, an additional `HeadlessServerLauncher` target will be built in addition to the existing `ServerLauncher` target. The `HeadlessServerLauncher`  will behave in the follow ways:

1.  On Windows, the server launcher will not spawn any native window and return immediately. Instead, it will behave like a true console application (It will be linked with `/SUBSYSTEM:CONSOLE` instead of `/SUBSYSTEM:WINDOWS`). It will direct all of its output to the current terminal / command window from where it is executed. If double-clicked from a file explorer, it will behave as a console application; a default Windows terminal will launch displaying the console stdout/stderr of the server process.
2.  On Linux, the server launcher will not have any dependencies on any X11/XCB system libraries. It will not spawn any native window and will output any messages from stdout/stderr to the console.  
3.  There is no server launcher support for any other platform at this time.

### Technical design description:
#### AzFramework Refactor

The majority of the lower-level code that interfaces directly with the host system's devices such as the display window and input keyboard, mouse, controllers, etc are defined in the `AzFramework` library. These interfaces are defined in their respective platform-independent classes:

*   [Application](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Application/Application.h#L44)
*   [InputDeviceGamepad](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Input/Devices/Gamepad/InputDeviceGamepad.h#L28)
*   [InputDeviceKeyboard](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Input/Devices/Keyboard/InputDeviceKeyboard.h#L30)
*   [InputDeviceMotion](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Input/Devices/Motion/InputDeviceMotion.h#L25)
*   [InputDeviceMouse](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Input/Devices/Mouse/InputDeviceMouse.h#L27)
*   [InputDeviceTouch](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Input/Devices/Touch/InputDeviceTouch.h#L23)
*   [InputDeviceVirtualKeyboard](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Input/Devices/VirtualKeyboard/InputDeviceVirtualKeyboard.h#L22)
*   [NativeWindow](https://github.com/o3de/o3de/blob/2e3f76beb03a976d4e4fcd819349506bbefc5746/Code/Framework/AzFramework/AzFramework/Windowing/NativeWindow.h#L95)

These classes were designed to provide platform-specific services to the host machine through the use of the PIMPL idiom and Platform Abstraction Layer (PAL) pattern in O3DE. However, since `AzFramework` is a base library for O3DE and has a build dependency to almost all targets in O3DE, it will pull in dependencies to any platform-specific native libraries that handle windowing and input as well. This forces all projects to pull in these dependencies. On Linux platforms, that means it will pull in dependencies to XCB and X11 libraries, even if it does not use them.

What is needed is to split off the platform-specific services and their dependent library dependencies from `AzFramework` into its own library,  `AzFramework.NativeUI`.  The abstraction of the windowing and input devices makes this possible. The PIMPL idiom allows us to move the platform-specific implementations of the native window and input device interactions with the platform into the new library, and the inclusion of that library can be specified in targets that intend to use them. In the case of the headless server launcher, it will be intentionally omitted. 

When omitted, the opaque pointers to the underlying PIMPL implementations will be `null` , and the abstraction layers that hosts these PIMPLs will be protected with a `null` check and perform a no-op in those cases. This will in effect disable any attempt to read or write to the native window or devices in headless mode (if any).

#### AzGameFramework, AzToolsFramework, and the UnifiedLauncher

In order to remove the native ui and input dependencies for server launchers, we will need to be explicit in which target will include them. For O3DE, an application/executable can either be a game/server launcher or a tool. Game/server launchers have a dependency on `AzGameFramework`, while all tool applications have a dependency on `AzToolsFramework`. The new `AzFramework.NativeUI` library will be added as a dependency to `AzToolsFramework` since this RFC is to only make server launchers headless. For `AzGameFramework` , we will also add `AzFramework.NativeUI` as a dependency since game launchers (and unified launchers) will not be headless. 

In order to make server launchers headless, we will make a new `AzGameFramework.Headless` library, which will be the same as `AzGameFramework`, except for the following:

*   The dependency to `AzFramework.NativeUI`  will be omitted.
*   A new public `LY_HEADLESS=1`  compilation will be defined to signal downstream dependencies that headless mode is enabled.

The [Unified Launcher](https://github.com/o3de/o3de/tree/development/Code/LauncherUnified) target defines declares three different possible launchers that projects can generate:

*   [Game Launchers](https://github.com/o3de/o3de/blob/26e68948b32e38f97231274acf90e461b4236f13/Code/LauncherUnified/CMakeLists.txt#L33)
*   [Server Launchers](https://github.com/o3de/o3de/blob/26e68948b32e38f97231274acf90e461b4236f13/Code/LauncherUnified/CMakeLists.txt#L51)
*   [Unified Launchers](https://github.com/o3de/o3de/blob/26e68948b32e38f97231274acf90e461b4236f13/Code/LauncherUnified/CMakeLists.txt#L70)

Currently, all three launcher types have build dependencies on:

*   `AZ::AzCore` 
*   `AZ::AzGameFramework` 
*   `Legacy::CryCommon` 

As noted above, we will be adding a dependency of `AzFramework.NativeUI`  to `AzGameFramework` . 

In order to support a headless version of [Server Launchers](https://github.com/o3de/o3de/blob/26e68948b32e38f97231274acf90e461b4236f13/Code/LauncherUnified/CMakeLists.txt#L51), we will need a separate CMAKE target omit the dependency of `AzFramework.NativeUI` from it. This means that for headless server launchers, it can no longer depend on `AzGameFramework` . We will need a headless version of `AzGameFramework` called `AzGameFramework.Headless` which will be the same as `AzGameFramework` but without the dependency on `AzFramework.NativeUI`.

#### Atom Render

Currently the server launcher can be launched without any specific Atom `RHI`  renderer by specifying the Atom Null Renderer in the startup argument `-NullRenderer`. This will prevent any graphics rendering and thus prevent the creation of any native client window. Even though we can do a dynamic switch to headless mode, the executable will still pull in dependencies to X11/XCB on Linux as a link dependency. For non-monolithic builds, the Vulkan RHI gem will attempt to load and bring in this dependency at runtime. For monolithic builds, this dependency will be pulled in at build time since the Vulkan RHI gem will be included in the executable (even if its not activated).

In order to solve this dependency issue, the `HeadlessServerLauncher` will rely on a combination of O3DE's build `variants` and creating headless versions of some targets that omit the dependency on any RHI implementations ([Atom\_Feature\_Common](https://github.com/o3de/o3de/blob/1ed05c8f21ebe203712128f1e473dc9bc7d7a4ab/Gems/Atom/Feature/Common/Code/CMakeLists.txt#L71C3-L92) and [Atom\_AtomBridge](https://github.com/o3de/o3de/blob/1ed05c8f21ebe203712128f1e473dc9bc7d7a4ab/Gems/AtomLyIntegration/AtomBridge/Code/CMakeLists.txt#L33-L64)). These two targets are pulling in the runtime dependencies for the non-null RHI implementations using the PAL pattern of including additional cmake files:

```cmake

PLATFORM_INCLUDE_FILES
    ${pal_dir}/additional_${PAL_PLATFORM_NAME_LOWERCASE}_runtime_deps.cmake

```

To create headless versions of [Atom\_Feature\_Common](https://github.com/o3de/o3de/blob/1ed05c8f21ebe203712128f1e473dc9bc7d7a4ab/Gems/Atom/Feature/Common/Code/CMakeLists.txt#L71C3-L92) and [Atom\_AtomBridge](https://github.com/o3de/o3de/blob/1ed05c8f21ebe203712128f1e473dc9bc7d7a4ab/Gems/AtomLyIntegration/AtomBridge/Code/CMakeLists.txt#L33-L64), the CMAKE target definition for them will be duplicated, with the exception of having a `.Headless` suffix in the target name, the omission of the `additional_${PAL_PLATFORM_NAME_LOWERCASE}_runtime_deps.cmake`, and an additional compile definition `LY_HEADLESS=1`, which will be needed in the gem's module code to distinguish between the normal and headless versions when declaring their module class (based on the gem name).

The headless versions will be specified through the use of a new `HeadlessServers` variant and will be aliased to target these headless versions instead of the normal ones. For example, the normal alias for the [Atom\_AtomBridge](https://github.com/o3de/o3de/blob/1ed05c8f21ebe203712128f1e473dc9bc7d7a4ab/Gems/AtomLyIntegration/AtomBridge/Code/CMakeLists.txt#L33-L64) target looks like:


```cmake

ly_create_alias(NAME Atom_AtomBridge.Servers NAMESPACE Gem TARGETS Gem::Atom_AtomBridge)

```
  

A new `HeadlessServers` variant alias will also be added to direct to the headless version of [Atom\_AtomBridge](https://github.com/o3de/o3de/blob/1ed05c8f21ebe203712128f1e473dc9bc7d7a4ab/Gems/AtomLyIntegration/AtomBridge/Code/CMakeLists.txt#L33-L64) :

```cmake

ly_create_alias(NAME Atom_AtomBridge.HeadlessServers NAMESPACE Gem TARGETS Gem::Atom_AtomBridge.Headless)

```

In order to ensure that the `NullRenderer` is always enabled when launching the headless version of the server launcher, it will query the application type to see if it is headless (This will be provided through updates to AzFramework)

### What are the advantages of the feature?
* On Linux, this option will enable O3DE server launchers to be deployed on Linux hosts that do not have or support a GUI     
* On Windows, this option will block the return from the server launcher until the server is shuts down or terminated. This can enable external tools to be built to launch and monitor the server process if needed.


### What are the disadvantages of the feature?
* This involves a mild refactor of a low-level library `AzFramework` .  
* This will increase build time (minimally) due to the additional libraries that need to be built. (For headless versions of the library)  
* The headless server will not have the ability to run any interactive console. That means that you will not have the ability to enter in any console commands (CVARS) interactively with the headless server. To retain this ability, the normal server launcher would be used instead.

### How will this be implemented or integrated into the O3DE environment?
The new feature will be added as an additional target for the launchers.

### Are there any alternatives to this feature?
The current Server Launcher supports launching without creating a native UI window through command-line arguments, however on Linux it will still pull in dependencies to X11 and XCB.

### How will users learn this feature?
This feature will be added to the user guides for 

* [CMake Settings Reference](https://www.docs.o3de.org/docs/user-guide/build/reference/)
* [Running Multiplayer Projects](https://www.docs.o3de.org/docs/user-guide/networking/multiplayer/running/)

### Are there any open questions?
* Why not provide a flag to build the server as normal or headless?  
  * This is also an option. However you can still control which specific target you want to build through cmake, and having the different name for the launcher helps distinguish the type of launcher it is even further.  
          
* Why not just make a headless version of \`AzFramework\` in addition to the original and make only the server launcher depend on that?  
  * The problem with that approach was that almost every target has some dependency on `AzFramework`. Even if we make the server launcher depend on an `AzFramework.Headless` by itself, other libraries (such as CrySystem) will have a dependency on that, not to mention all the gems. We had pull out the native ui dependency from AzFramework, which means if we opted to do `AzFramework.Headless` instead, then that change would need to propogate to every target in the build system.  
          
* Are there any security considerations or risks that this feature introduces?  
  * Even though this affects the server launcher, this feature only affects UI presentation (or suppression of) for the server launcher. Nothing related networking is involved other than this being the server launcher.

* What if I only want a headless or non-headless version of the server launcher. Do I always have to build both?
  * We can make this optional by providing CMAKE flag to specify which server types to build.

