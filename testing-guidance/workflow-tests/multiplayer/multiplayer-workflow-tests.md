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
  * Ensure the ServerLauncher is built, as well as the Editor and ClientLauncher (if testing via the client launcher).
* Alternatively: A project has been created and built that includes the Multiplayer Gem and subsequent changes have been made to enable functionality ([O3DE Multiplayer Gem](https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/configuration/)).
  * A new level should be created in the project that utilizes the following to act as identifiers for whether multiplayer connections are working as intended:
    * Player Spawners 
    * Network Prefabs

## Workflows

| Workflow                            | Requests                                                                                                                                                                                                                                                                                                                                                                          | Things to Watch For                                                                                                                                                                                        |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Editor Game Mode**                | <ol><li>Run the network enabled level (network prefabs, spawners, etc.) in game mode in Editor.</li><li>Leave game mode.</li></ol>                                                                                                                                                                                                                                                | <ul><li>Network player prefabs are never spawned when they should</li><li>Error logs in Editor Console.</li><li>Connection drops.</li><li>Server process doesn't shut down</li></ul>                       |
| **Server/Game Launchers**           | <ol><li>Launch the server launch and configure to use the network enabled level.</li><li>Launch the client/game launcher and connect to the server.</li></ol>                                                                                                                                                                                                                     | <ul><li>Network player prefabs are never spawned</li><li>Error logs in either console `~`.</li><li>Connection drops.</li></ul>                                                                             |
| **Auto Component Creation/Editing** | <ol><li>Edit an existing auto component or create a new one <ul><li> EG: `MultiplayerSample\Gem\Code\Source\AutoGen\NetworkHealthComponent.AutoComponent.xml`</li></ul></li><li>Compile the project.</li><li>Add the new/edited component to an entity in the level.</li><li>Check the level in Editor game mode.</li><li>Check the level in Server and Game Launcher</li></ol>   | <ul><li>Compile errors that don't seem to result from user error.</li><li>Component doesn't display in Component List in Editor.</li><li>Error logs in Editor, Server or Game Launcher consoles.</li></ul> |
