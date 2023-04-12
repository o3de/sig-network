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

## MultiplayerSample Gameplay Validations

<details> 

**<summary>Player Connection</summary>** 

* Players are able to connect to the server without issue.
* Game is able to start. 

</details> 

<details> 

**<summary>Player Movement & Combat</summary>**

* Moving, Jumping, and basic controls work as expected.
* Aiming and Primary Weapon Fire work as expected.
* Pistol firing VFX should be visible.

</details> 


<details>

**<summary>UI/UX</summary>** 

* Connected player count should display the correct number of players.
* Round counter should start at the end of the round.
* Round counter should end at round end.
* Scoreboard (opened with `Tab`) should accurately display scores. 
  * Increase on gem pick up.
  * Decrease on player death by `20%`.
* `Esc` Menu - Continue and Quit should work as expected. 

</details> 

<details> 

**<summary>Basic Gameplay</summary>** 

* Collected gems should disappear from the field.
* Jumping out of bounds should have the player respawn.
* Start of Round should respawn players.

</details> 

<details> 

**<summary>Victory/Defeat</summary>** 

* Player with the most gems wins the round.
* Winning player can tell they have won.
* Losing players can tell they did not win. 

</details> 

<details> 

**<summary>Stability</summary>** 

* No stability issues are present when playing
* Ensure no errors are present in the server or game logs. 
  * `user/logs/server.log`.
  * `user/logs/game.log`.

</details> 

## Workflows

### Area: Client-Server Building

**Docs**

* [O3DE Multiplayer Sample Windows Download & Install](https://github.com/o3de/o3de-multiplayersample/#download-and-install)
* [O3DE Multiplayer Sample Linux Download & Install](https://github.com/o3de/o3de-multiplayersample/blob/development/README_LINUX.md#download-and-install)
* [O3DE Tutorial: First Multiplayer Component](https://www.o3de.org/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/)

**Prerequisites**

* A Multiplayer level exists in the project (must contain a **netbind** component).

**Product:**

* A functional server launcher.
* A functional client launcher.

**Suggested Timebox:** 1 hour per platform

| Workflow                                 | Requests                                                                                                                                                                                                                                                                                                                                                    | Things to Watch For                                                                                                                                                                                           |
|------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Build Existing Project Launchers**     | <ol><li>Build all 3 launchers using CLI<ul><li>`cmake --build build/windows --target MyProject.GameLauncher MyProject.ServerLauncher MyProject.UnifiedLauncher --config profile -- -m`</li></ul></li></ol>                                                                                                                                                  | <ul><li>Project launchers build successfully.</li><li>Server, Client and Unified launchers are built in the project build directory.</li><li>Project can be launched in **O3DE Editor** once built.</li></ul> |
| **Build New Project Launchers**          | <ol><li>Create a new multiplayer project.</li><li>Build all 3 launchers using CLI<ul><li>`cmake --build build/windows --target MyProject.GameLauncher MyProject.ServerLauncher MyProject.UnifiedLauncher --config profile -- -m`</li></ul></li></ul></ol>                                                                                                   | <ul><li>Project launchers build successfully.</li><li>Server, Client and Unified launchers are built in the project build directory.</li><li>Project can be launched in **O3DE Editor** once built.</li></ul>                                                                |
| **Build Project with Project Manager**   | <ol><li>Launch `o3de.exe`.</li><li>Add or create a project with the Multiplayer Gem enabled.</li><li>Select Build Project.</li></ol>                                                                                                                                                                                                                        | <ul><li>Project launchers build successfully.</li><li>Server, Client and Unified launchers are built in the project build directory.</li><li>Project can be launched in **O3DE Editor** once built.</li></ul>                                                                |
| **Test Hosting with Unified Launcher**   | <ol><li>Build an existing or new multiplayer project's launchers - using either Project Manager or CLI.</li><li>Launch the `project.UnifiedLauncher`.</li><li>Open the Console `~`.</li><li>Enter `host` into the console.</li><li>Load a multiplayer level.</li><li>Launch the `project.GameLauncher`.</li><li>Enter `connect` into the console.</li></ol> | <ul><li>Console displays text confirming the session is operating in ClientServer mode.</li><li>Game Launcher client is able to connect to the ClientServer.</li><li>Project can be launched in **O3DE Editor** once built.</li></ul>                                        |
| **Compile Autogen File**                 | <ol><li>Use either MultiplayerSample or create an autogen file following the [O3DE Tutorial: First Multiplayer Component](https://www.o3de.org/docs/learning-guide/tutorials/multiplayer/first-multiplayer-component/) instructions.</li><li>Edit the selected autogen file if using an existing one.</li><li>Build the Project.</li></ol>                  | <ul><li>No compile errors when building.</li><li>If the edits included a component field/component, the changes show in the Editor.</li><li>Project can be launched in **O3DE Editor** once built.</li></ul>                                                                 |
---

### Area: Client-Server Communications

**Prerequisites**

* Ensure the **ServerLauncher** is built, as well as the **Editor** and **ClientLauncher** (if testing via the client launcher).

| Workflow                            | Requests                                                                                                                                                                                                                                                                                                                                                                          | Things to Watch For                                                                                                                                                                                        |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Editor Game Mode**                | <ol><li>Run the network enabled level (network prefabs, spawners, etc.) in game mode in Editor.</li><li>Leave game mode.</li></ol>                                                                                                                                                                                                                                                | <ul><li>Network player prefabs are never spawned when they should</li><li>Error logs in Editor Console.</li><li>Connection drops.</li><li>Server process doesn't shut down</li></ul>                       |
| **Server/Game Launchers**           | <ol><li>Launch the server launch and configure to use the network enabled level.</li><li>Launch the client/game launcher and connect to the server.</li></ol>                                                                                                                                                                                                                     | <ul><li>Network player prefabs are never spawned</li><li>Error logs in either console `~`.</li><li>Connection drops.</li></ul>                                                                             |
| **Auto Component Creation/Editing** | <ol><li>Edit an existing auto component or create a new one <ul><li> EG: `MultiplayerSample\Gem\Code\Source\AutoGen\NetworkHealthComponent.AutoComponent.xml`</li></ul></li><li>Compile the project.</li><li>Add the new/edited component to an entity in the level.</li><li>Check the level in Editor game mode.</li><li>Check the level in Server and Game Launcher</li></ol>   | <ul><li>Compile errors that don't seem to result from user error.</li><li>Component doesn't display in Component List in Editor.</li><li>Error logs in Editor, Server or Game Launcher consoles.</li></ul> |
---

### Area: Debug Desync

**Prerequisites**

* These scenarios all require a server and at least one client to be set up and connected utilizing the **SampleBase** level.
  * See [Multi-Machine MultiplayerSample Workflows](../multi-machine-multiplayer-workflow-tests.md) for reference. 
  * This can also be done with a server and client on the same machine for easier set up. 
* With reduced situations causing desync, it may be required to use a tool similar to **[Clumsy](http://jagt.github.io/clumsy/)** (or your preferred tool) to create artificial network issues.

**Audit Feature CVARS**

* `cl_EnableDesyncDebugging`
* `cl_DesyncDebugging_AuditInputs`
* `net_DebutAuditTrail_HistorySize` 

**Product:** A Network **Audit Trail** that provides the expected information.

**Suggested Timebox:** 30 minutes per platform.

| Workflow                              | Requests                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Things to Watch For                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **CVARS can be adjusted at runtime**  | <ol><li>Bring up console `~` in Server Launcher.</li><li>Set `cl_EnableDesyncDebugging` to true.</li><li>Set `cl_DesyncDebugging_AuditInputs` to true.</li><li>Set `net_DebutAuditTrail_HistorySize` to something other than the default of `20`.</li><li>Try to force a desync through button mashing, alt-tabbing, general chaotic inputs on one of the game launcher client windows.</li><li>In the server launcher, press the `Home Key`.</li><li>Go to **Multiplayer → Audit Trail**.</li></ol> | <ul><li>The server console window will note when a desync has occurred.</li><li>Desync debugging can be seen in the `Home Key → Multiplayer → Audit Trail` menu after enabling `cl_EnableDesyncDebugging`.</li><li>Positional desync debugging can be seen in the `Home Key` menu after enabling `cl_DesyncDebugging_AuditInputs`.</li><li>The new `net_DebutAuditTrail_HistorySize` value is echoed in the console window after changing it.<ul><li>The number of items in the **Desync Debugging** list is capped at this value with the desync titles counted toward the total.</li></ul></li></ul> |
| **Desync Audit Search functionality** | <ol><li>All audit features are enabled.</li><li>In the Audit Trail window `HOME Key → Multiplayer → Audit Trail` enter a value (case sensitive) into the search field that corresponds to some subset of currently capture information.</li></ol>                                                                                                                                                                                                                                                    | <ul><li>Audit trail only lists items that meet the entered search term.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| **Refresh Button**                    | <ol><li>All audit features are enabled.</li><li>In the `Home Key → Multiplayer → Audit Trail` window press the **Refresh** button.</li></ol>                                                                                                                                                                                                                                                                                                                                                         | <ul><li>The audit list updates to show additional desync records that occurred since the last refresh, up to the defined history size.</li></ul>                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
---

### Area: Player Spawners

**Prerequisites**

* These scenarios all require a server and at least one client to be set up and connected utilizing the **SampleBase** level.
  * See [Multi-Machine MultiplayerSample Workflows](../multi-machine-multiplayer-workflow-tests.md) for reference. 
  * This can also be done with a server and client on the same machine for easier set up. 

**Product:** A Multiplayer Game Session that spawns players in across player spawners.


**Suggested Timebox:** 30 minutes per platform.

| Workflow                                                                                 | Requests                                                                                                                                                                                                                                                                                                                                                                                              | Things to Watch For                                                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Existing player spawners in MultiplayerSample sample_base level function as expected** | <ol><li>Launch the **Server Launcher** and set it up to host by entering the `host` command in the console.</li><li>Change the level to **SampleBase**.</li><li>Connect multiple **Game Launcher Clients** to the server by entering the `connect` command entered in game launcher console.</li></ol>                                                                                                | <ul><li>As clients connect, they spawn at the player spawner locations in a round robin fashion.</li><li>Each player spawns in a new location until all player spawners have been used, then restarting at the beginning location.</li></ul>                                      |
| **Add additional player spawners**                                                       | <ol><li>Open the **O3DE Editor**.</li><li>Open the **SampleBase** level.</li><li>Add a new, or duplicate an existing, player spawner entity with the player spawner component added and configured.</li><li>Save and close the **O3DE Editor**.</li><li>Launch and setup the **Server Launcher** before connecting multiple **Game Launcher Clients** to the server.</li></ol>                        | <ul><li>As clients connect, they spawn at the player spawner locations in a round robin fashion.</li><li>Each player spawns in a new location until all player spawners (including the newly added spawners) have been used, then restarting at the beginning location.</li></ul> |
| **Snap to Ground**                                                                       | <ol><li>Open the **O3DE Editor**.</li><li>Open the **SampleBase** level.</li><li>Select all **Spawner** entities and toggle **Snap to Ground** off.</li><li>Raise all the entities by adjusting the **Z Translate** value.</li><li>Save and close the **O3DE Editor**.</li><li>Launch and setup the **Server Launcher** before connecting multiple **Game Launcher Clients** to the server.</li></ol> | <ul><li>Players should spawn at the new Z height as they connect.</li></ul>                                                                                                                                                                                                       |
---

### Area: Play NewStarBase in Editor

**Docs**

* [O3DE MultiplayerSample Player Control Guide](https://github.com/o3de/o3de-multiplayersample/#player-controls)

**Prerequisites**

* **MultiplayerSample** project has both the **Server Launcher** & **Game Client** built.

**Product:** A built project that is playable through the **O3DE Editor**.

**Suggested Timebox:** 30 minutes per platform.

| Workflow                                    | Requests                                                                                                                                                                                                      | Things to Watch For                                                                                                                                                                                                                               |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Load and Launch into O3DE Editor Client** | <ol><li>Load **NewStarBase** level in **O3DE Editor**.</li><li>Hit play.</li></ol>                                                                                                                            | <ul><li>Level should load in the **O3DE Editor**.</li><li>Green text in bottom right should show `Connected`.</li></ul>                                                                                                                           |
| **Basic Player Movement & Player Combat**   | <ol><li>**NewStarBase** level is in game mode in the **O3DE Editor**.</li><li>Move around the level using the movement controls.</li><li>Utilize the combat controls while moving around the level.</li></ol> | <ul><li>Moving, Jumping, and basic controls work as expected.</li><li>Aiming and Primary Weapon Fire work as expected.</li><li>Pistol firing VFX should be visible.</li></ul>                                                                     |
| **Basic Gameplay**                          | <ol><li>Collect Gems.</li><li>Go **Out of Bounds (OOB)**.</li><li>Play through until a new **Start of Round**.</li></ol>                                                                                      | <ul><li>Collected gems should disappear from the field.</li><li>Jumping out of bounds should have the player respawn.</li><li>Start of Round should respawn players.</li></ul>                                                                    |
| **Basic UI/UX**                             | <ol><li>Round Counter.</li><li>Player Health/Shields.</li><li>Scoreboard (Press `Tab`).</li><li>Scoreboard (End of Round).</li><li>`Esc` Menu</li></ol>                                                       | <ul><li>Round counter should start at the end of the round.</li><li>Round counter should end at round end.</li><li>Scoreboard should accurately display scores.</li><li>`Esc` Menu - **Continue** and **Quit** should work as expected.</li></ul> |
---

### Area: Play NewStarBase in Standalone Launchers

**Docs**

* [O3DE MultiplayerSample Player Control Guide](https://github.com/o3de/o3de-multiplayersample/#player-controls)

**Prerequisites**

* **MultiplayerSample** project has both the **Server Launcher** & **Game Client** built.
* **Server Launcher** is configured and launched in **Headless Mode** with **NewStarBase** level.

**Product:** A built **Server Launcher** & **Game Client** that is able to connect to the server and interact with other game clients.

**Suggested Timebox:** 60 minutes per platform.

| Workflow                                                            | Requests                                                                       | Things to Watch For                                                                |
|---------------------------------------------------------------------|--------------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| **2 Game Clients connect and play through until end of the round**  | <ul><li>Players play through the session until the end of the round.</li></ul> | [MultiplayerSample Gameplay Validations](#MultiplayerSample-Gameplay-Validations)  |
| **3+ Game Clients connect and play through until end of the round** | <ul><li>Players play through the session until the end of the round.</li></ul> | [MultiplayerSample Gameplay Validations](#MultiplayerSample-Gameplay-Validations)  |
