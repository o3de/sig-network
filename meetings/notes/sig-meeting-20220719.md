## Meeting Details

- **Date/Time:** [July 19th, 2022 @ XX:XXpm UTC / XX:XXpm ET](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1544490&calstart=2022-07-19)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/66)

## SIG Updates

**What happened since the last meeting?**

### General SIG announcements
Elections are upcoming for SIG chair(s), look for announcement about how you can step up to be SIG-Network or other SIG leads. 

Looking to improve the Multiplayer Sample to make it more of an actual game (will solicit SIG for feedback)

[O3DCon 2022](https://events.linuxfoundation.org/o3dcon/) call for papers closes July 29th (extended).

### Networking
* Added type validation serializer
* Added changes to control local port numbers during connect
* Fixes for entity migration 
* Fixes for entity hierarchies and sort order of spawnables with network components
* Better disconnect handling for client and server
* Improvements to the ctrl+ g experience including turning on editor server logs.
* Fixed camera jitter in MPS (improved interploation)
* Fixed icons in MPS
* Improvements to spawning perf test level

### Cloud Services
* Fix build issues with AWS Metrics Gem
* Continue to work on planning for AWS Gem migration from o3de/o3de
* Continue work on AWS CDK v2 updates for Gems

## Meeting Agenda
* Reviewed open RFC for remote tooling gem: https://github.com/o3de/sig-network/issues/68
   * No concerns or comments from SIG at this time, move to accept RFC once final comment period is over
  
* Review posted [Public Roadmap](https://docs.google.com/spreadsheets/d/1U_XAw9YJoPE3FlYeBKUz4kYjFRDS09NS3_YuZSZJ2qA/edit?usp=sharing)
  * No comments or concerns from SIG

## Open Discussion Items
* No recent TSC updates impacting SIG.
* Still have an open request for input on [Multiplayer Starter Pack Gem](https://github.com/o3de/o3de/issues/8281)

## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220621.md):
* _Completed_: Need to remove blockers to public roadmap updates for SIG-Network
* _TODO_: Need to decide on SIG/network's responsibility towards cloud service gems once they migrate from O3DE/O3DE (may require charter update).