# Sig-Network RFCs

This is where Request for Change (RFC) documents that have been accepted by SIG-Network are kept.

## RFC Naming

RFC naming follows the pattern of:
```rfc-net-(YYYYMMDD)-(X)-(friendlyname)```

#### Notes

* RFC should be referred to as ```rfc-net-YYYYMMDD-X```, friendly name is just to aid discovery.
* ```YYYYMMDD```: Replace with the date of the sig-network meeting that accepted the RFC.
* ```X``` is a counter for the day, if there are multiple RFCs accepted in a single meeting.

## Recent Accepted RFCS

### 2023

| RFC                                                                   | Month   | Summary                                                                                                                                                                                                             |
|-----------------------------------------------------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [rfc-net-20230821-1-headless-server-launcher](rfc-net-20230821-1-headless-server-launcher.md) | August | Headless Server Launcher
### 2022

| RFC                                                                   | Month   | Summary                                                                                                                                                                                                             |
|-----------------------------------------------------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [rfc-net-20220118-1-clientserver](rfc-net-20220118-1-clientserver.md) | January | Client/Server build separation                                                                                                                                                                                      |
| [rfc-net-20220719-1-awsgems](rfc-net-20220719-1-awsgems.md)           | July    | Migration of Amazon Gems from O3DE main repository.                                                                                                                                                                 |
| [rfc-net-20220728-1-remotetools](rfc-net-20220728-1-remotetools.md)   | July    | Support for remote tools connections in O3DE                                                                                                                                                                        |
| [rfc-net-20221020-1-clientserver](rfc-net-20221020-1-clientserver.md) | October | Updates [rfc-net-20220118-1-clientserver](rfc-net-20220118-1-clientserver.md) to move from a Client and Server target with a CMake target to allow Client to include Server to a Client, Server and Unified target. |
| [rfc-net-20221020-2-netversion](rfc-net-20221020-2-netversion.md)     | October | Mechanism to generate a version for network layer to detect client and server network mismatches.                                                                                                                   |

### 2021
| RFC                                                                             | Month    | Summary                                                |
|---------------------------------------------------------------------------------|----------|--------------------------------------------------------|
| [rfc-net-20211005-1-inputscripting](rfc-net-20211005-1-inputscripting.md)       | October  | Exposes network input to script                        |
| [rfc-net-20211005-2-entityhierarchies](rfc-net-20211005-2-entityhierarchies.md) | October  | Defines network entity hierarchies                     |
| [rfc-net-20211026-1-flexmatch](rfc-net-20211026-1-flexmatch.md)                 | October  | Adds Matchmaking support for Amazon GameLift Gem       |
| [rfc-net-20211207-1-desync-log](rfc-net-20211207-1-desync-log.md)               | December | Defines logging extensions to support desync debugging |

