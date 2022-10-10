# Multiplayer Testing

These tests are designed to cover the basic functionality related to running a level from a Multiplayer project.

General Documentation for this feature set: [https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/](https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/).

## Prerequisites

*   MultiplayerSample project is cloned and built ( [https://github.com/o3de/o3de-multiplayersample](https://github.com/o3de/o3de-multiplayersample) )
    *   If using this approach, the SampleBase level can be used for most tests
*   Alternatively: A project has been created and built that includes the Multiplayer Gem and subsequent changes have been made to enable functionality ( [https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/configuration/](https://docs.o3de.org/docs/user-guide/gems/reference/multiplayer/multiplayer-gem/configuration/) )
    *   If using this approach, a new level should be created in the project, The level should utilize a player spawner and network prefabs, as these will act as identifiers for whether multiplayer connections are working as intended.

## Common Issues

*   Most issues will be caught through log errors, which can be seen in the Editor and Launcher consoles or in logs after running tests
*   In addition, network prefabs can be a visual clue that something isn't working as expected (objects disappearing or not appearing in the first place)
*   Processes that remain after closing applications - this can result in errors when trying to launch subsequent times

## Workflows

| Workflow                        | Requests                                                                                                                                                                                                                                                                                                                                                    | What To Watch Out For                                                                                                                                                                   |
|---------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Editor Game Mode                | *   Run the network enabled level (network prefabs, spawners, etc.) in game mode in Editor<br>*   Leave game mode                                                                                                                                                                                                                                           | *   Network player prefabs are never spawned<br>*   Error logs in Editor Console<br>*   Connection drops<br>*   Server process doesn't shut down                                        |
| Server/Game Launchers           | *   Launch the server launch and configure to use the network enabled level<br>*   Launch the client/game launcher and connect to the server                                                                                                                                                                                                                | *   Network player prefabs are never spawned<br>*   Error logs in either console (~)<br>*   Connection drops                                                                            |
| Auto Component Creation/Editing | *   Edit an existing auto component or create a new one (existing example: MultiplayerSample\\Gem\\Code\\Source\\AutoGen\\NetworkHealthComponent.AutoComponent.xml )<br>*   Compile the project<br>*   Add the new/edited component to an entity in the level<br>*   Check the level in Editor game mode<br>*   Check the level in Server and Game Launcher | *   Compile errors that don't seem to result from user error<br>*   Component doesn't display in Component List in Editor<br>*   Error logs in Editor, Server or Game Launcher consoles |
