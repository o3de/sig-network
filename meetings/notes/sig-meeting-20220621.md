## Meeting Details

- **Date/Time:** [June 21, 2022 @ XX:XXpm UTC / XX:XXpm ET](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1544490&calstart=2022-06-21)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/63)

## SIG Updates

**What happened since the last meeting?**

### General SIG announcements

Elections are upcoming for SIG chair(s), look for announcement about how you can step up to be SIG-Network or other SIG leads.

### Networking
* Address exception during rebuilding a not fully active networked entity hierarchy
* Corrected payload size calculations for compressed TCP to make Multiplayer Compression gem function with Multiplayer again.
* Fixed some asserts when creating network prefab
* For SIG/content - Fixed Lua IDE breakpoint stall and failure to register self as host, to make lua debugging functional.
* Added missing spawn ticket ref to SpawnIfAuthority.sc and fix misnamed cfg var in Multiplayer Sample.

### Cloud Services
* Working on RFC: https://github.com/o3de/o3de/issues/8281 to migrate AWS and Amazon gems from O3DE/O3DE
* 
## Meeting Agenda
* Review RFC:  https://github.com/o3de/sig-network/issues/64
* Review Public Roadmap Proposal
  
## Open Discussion Items
* No recent TSC updates impacting SIG
* Still have an open request for input on Multiplayer Starter Pack Gem (See https://github.com/o3de/o3de/issues/8281)

## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220412.md):
* _In Progress_: Need to remove blockers to public roadmap updates for SIG-Network: ETA May 2022 (@lmbr-pip)
     * Proposed list of SIG items, polling partners for contributions.
     * Waiting for LF to provide update on template and location for roadmap.
* _Completed_: SIG completes feature matrix and provides requested dependency graphs
    * Setup working group again to complete matrix due to loss of original notes
    * lmbr-pip will update and push PR changes https://github.com/o3de/community/pull/128
* _Completed_: Set up new meeting cadence. Announce to SIG and update calendar 

* _NEW_: Need to decide on SIG/network's responsibility towards cloud service gems once they migrate from O3DE/O3DE (may require charter update).