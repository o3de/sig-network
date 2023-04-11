# Multiplayer Testing Workflow Tests

These tests are designed to cover the basic functionality related to running a level from a Multiplayer project.

## General Docs 

* [O3DE Multiplayer](https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/)
* [O3DE Multiplayer Gem](https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/configuration/)

## Common Issues

* Most issues will be caught through log errors, which can be seen in the Editor and Launcher consoles or in logs after running tests.
* In addition, network prefabs can be a visual clue that something isn't working as expected (objects disappearing or not appearing in the first place).
* Issues connecting to the server are caused either by:
  * Processes that remain after closing applications - this can result in errors when trying to launch subsequent times.
  * The ServerLauncher has not been built or has not been built against the same commit as Editor/ClientLauncher.

## Prerequisites

* MultiplayerSample project is cloned and built [O3DE MultiplayerSample](https://github.com/o3de/o3de-multiplayersample)
  * If using this approach, the SampleBase level can be used for most tests.
* Alternatively: A project has been created and built that includes the Multiplayer Gem and subsequent changes have been made to enable functionality ([O3DE Multiplayer Gem](https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/configuration/)).
  * A new level should be created in the project that utilizes the following to act as identifiers for whether multiplayer connections are working as intended:
    * Player Spawners. 
    * Network Prefabs.

## Server Platforms

* Windows
* Linux

## Game Launcher Platforms

* Windows
* Linux

## Workflows

### Area: Client-Server Building

**Docs**

* [O3DE Tutorial: First Multiplayer Component](https://www.o3de.org/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/)

**Prerequisites**

* A Multiplayer level exists in the project (must contain a **netbind** component).

**Product:**

* A functional server launcher.
* A functional client launcher.

**Suggested timebox:** 1 hour per platform

| Workflow                                 | Requests                                                                                                                                                                                                                                                                                                                                                    | Things to Watch For                                                                                                                                                    |
|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Build Existing Project Launchers**     | <ol><li>Build all 3 launchers using CLI<ul><li>`cmake --build build/windows --target MyProject.GameLauncher MyProject.ServerLauncher MyProject.UnifiedLauncher --config profile -- -m`</li></ul></li></ol>                                                                                                                                                  | <ul><li>Project launchers build successfully.</li><li>Server, Client and Unified launchers are built in the project build directory.</li></ul>                         |
| **Build New Project Launchers**          | <ol><li>Create a new multiplayer project.</li><li>Build all 3 launchers using CLI<ul><li>`cmake --build build/windows --target MyProject.GameLauncher MyProject.ServerLauncher MyProject.UnifiedLauncher --config profile -- -m`</li></ul></li></ul></ol>                                                                                                   | <ul><li>Project launchers build successfully.</li><li>Server, Client and Unified launchers are built in the project build directory.</li></ul>                         |
| **Build Project with Project Manager**   | <ol><li>Launch `o3de.exe`.</li><li>Add or create a project with the Multiplayer Gem enabled.</li><li>Select Build Project.</li></ol>                                                                                                                                                                                                                        | <ul><li>Project launchers build successfully.</li><li>Server, Client and Unified launchers are built in the project build directory.</li></ul>                         |
| **Test Hosting with Unified Launcher**   | <ol><li>Build an existing or new multiplayer project's launchers - using either Project Manager or CLI.</li><li>Launch the `project.UnifiedLauncher`.</li><li>Open the Console `~`.</li><li>Enter `host` into the console.</li><li>Load a multiplayer level.</li><li>Launch the `project.GameLauncher`.</li><li>Enter `connect` into the console.</li></ol> | <ul><li>Console displays text confirming the session is operating in ClientServer mode.</li><li>Game Launcher client is able to connect to the ClientServer.</li></ul> |
| **Compile Autogen File**                 | <ol><li>Use either MultiplayerSample or create an autogen file following the [O3DE Tutorial: First Multiplayer Component](https://www.o3de.org/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/) instructions.</li><li>Edit the selected autogen file if using an existing one.</li><li>Build the Project.</li></ol>                  | <ul><li>No compile errors when building.</li><li>If the edits included a component field/component, the changes show in the Editor.</li></ul>                          |
---

### Area: Client-Server Communications

**Prerequisites**

* Ensure the **ServerLauncher** is built, as well as the **Editor** and **ClientLauncher** (if testing via the client launcher).

**Platforms**

* Windows
* Linux

| Workflow                            | Requests                                                                                                                                                                                                                                                                                                                                                                          | Things to Watch For                                                                                                                                                                                        |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Editor Game Mode**                | <ol><li>Run the network enabled level (network prefabs, spawners, etc.) in game mode in Editor.</li><li>Leave game mode.</li></ol>                                                                                                                                                                                                                                                | <ul><li>Network player prefabs are never spawned when they should</li><li>Error logs in Editor Console.</li><li>Connection drops.</li><li>Server process doesn't shut down</li></ul>                       |
| **Server/Game Launchers**           | <ol><li>Launch the server launch and configure to use the network enabled level.</li><li>Launch the client/game launcher and connect to the server.</li></ol>                                                                                                                                                                                                                     | <ul><li>Network player prefabs are never spawned</li><li>Error logs in either console `~`.</li><li>Connection drops.</li></ul>                                                                             |
| **Auto Component Creation/Editing** | <ol><li>Edit an existing auto component or create a new one <ul><li> EG: `MultiplayerSample\Gem\Code\Source\AutoGen\NetworkHealthComponent.AutoComponent.xml`</li></ul></li><li>Compile the project.</li><li>Add the new/edited component to an entity in the level.</li><li>Check the level in Editor game mode.</li><li>Check the level in Server and Game Launcher</li></ol>   | <ul><li>Compile errors that don't seem to result from user error.</li><li>Component doesn't display in Component List in Editor.</li><li>Error logs in Editor, Server or Game Launcher consoles.</li></ul> |
