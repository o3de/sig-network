# Testing Multiplayer Launchers on Multiple Machines

This test is intended to verify successful connection between separate machines on the same network.  Ideally this is completed with a Linux machine as server and any number of windows machines as clients, but it can be completed with any combination.

## Prerequisites

**All machines used for this test should be on the same Network**

**Acquire and Compile MultiplayerSample project** ( [https://github.com/o3de/o3de-multiplayersample](https://github.com/o3de/o3de-multiplayersample) )

**Set Up for the Server Machine:**

These steps only need to be done on the 1 machine that will be acting as the server/host for the test

### Linux:

1.  Open a terminal window
2.  Enter “sudo ufw allow from any to any port 33450 proto udp”
3.  Enter “sudo ufw status verbose”
4.  Check that rule(s) exist for port 33450 with action “ALLOW IN”
5.  Enter “ifconfig”
6.  Note the inet ip address found in the first block of information, this address will be used by the clients to connect to the server machine

Cleaning up after testing:

1.  Open a terminal window
2.  Enter “sudo ufw status numbered”
3.  In the list, note the number for the first entry of your created 33450 rules
4.  Enter “sudo ufw delete #” where ‘#’ is the number assigned to the rule
5.  Press ‘y’ to confirm
6.  Repeat steps 2-5 to remove the second entry (the number for the entry will have updated after deleting the first)

### Windows:

1.  Open Windows Defender Firewall
2.  Click Advanced Settings
3.  Click Inbound Rules
4.  Click New Rule
5.  Click Port, then Next
6.  Click UDP
7.  If it’s not already selected, select Specific local ports and enter 33450, then Next
8.  Click Allow the connection, then Next
9.  Select Network types you’d like to allow connection over (Public shouldn’t be necessary, but you can leave it as the default), then Next
10.  Name the rule, then Finish
11.  Open a cmd window
12.  Enter “ipconfig”
13.  Note the IPv4 address, this will be used by the clients to connect to the server machine

Cleaning up after testing:

1.  Open Windows Defender Firewall
2.  Click Advanced Settings
3.  Click Inbound Rules
4.  Find the rule with the name that was given
5.  Right-Click the rule and Select Delete
6.  Confirm the dialog

## What To Watch For

1.  Number of clients connected
2.  Performance on clients (FPS, input response)
3.  Logs from clients and server after testing

## Testing

1.  Server machine has completed the above prerequisites
2.  All machines have built MultiplayerSample project(for minimum compiling - the server machine should at least have MultiplayerSample.ServerLauncher built and the clients should have MultiplayerSample.GameLauncher) - [https://github.com/o3de/o3de-multiplayersample](https://github.com/o3de/o3de-multiplayersample)
3.  Server machine launches MutiplayerSample.ServerLauncher
    1.  If server.cfg is being used and is still the default entries of “host” and “LoadLevel Levels/SampleBase/SampleBase.spawnable” then nothing more should need to be done on this machine
    2.  If server.cfg is not being used, the user will need to press “~” to bring up the console after launching the launcher and enter “host” followed by “loadlevel samplebase”
4.  Client machines launch MultiplayerSample.GameLauncher
    1.  If client.cfg is being used, add an entry before the default “connect” entry that says cl\_serveraddr ###.###.#.### where the numbers are the IP address of the server machine that was noted during server set up.  This needs to be done **before** launching the game launcher
    2.  If client.cfg is not being used, once the game launcher is up the user will need to press “~” to bring up the console and enter “cl\_serveraddr ###.###.#.###” (the noted server machine ip address) followed by “connect”
5.  At this point the client machines should have loaded the level that the Server machine defined and can move around in the level