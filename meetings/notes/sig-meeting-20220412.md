## Meeting Details

- **Date/Time:** [April 12, 2022 @ XX:XXpm UTC / XX:XXpm ET](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1305634&calstart=2022-04-12)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Gene Walters (AMZN-Gene)

[Agenda Issue](https://github.com/o3de/sig-network/issues/53)

## SIG Updates

**What happened since the last meeting?**

### General SIG announcements
* None

### Networking
* Converted AZNetworking to a shared library in order to deprecate LuaIDE's use of Gridmate
    * Will send notice of Gridmate/Gridhub removal.
* Fixed some editor connection issues 
* Investigating connect/disconnect/connect issues

### Cloud Services
* CDK update to fix installer builds not bringing CDK apps

## Meeting Agenda
* Release build testing: https://github.com/o3de/o3de/issues/8707 
* Server packing scripts: https://github.com/o3de/o3de/issues/8685
* MPS starter kit proposal: https://github.com/o3de/o3de/issues/8281

## Outcomes from Discussion topics
- Release build testing, this is general to O3DE not networking, issues sent to relevant SIGs
- Gamelift packaging script
  - Good to keep a script for packaging both profile (with loose assets) and release
  - Should build on `--target INSTALL` for servers
- Multiplayer Starter Pack Gem
  - Looking for input about shape of work
  - Custom script canvas node: IsNetRole -> 4 out for each of the roles (added to git feature request)

 - 
## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220329.md):
* _Updated_: O3DE Marketing would like screenshots of MPS running on mobile (@lmbr-pip)
     * Have screenshots, need to sync with marketing about where they want them delivered.
* _No Update_: Need to remove blockers to public roadmap updates for SIG-Network: ETA April 2022 (@lmbr-pip)
     * Still working with roadmap owners to get items on roadmap. Delayed by change of ownership.
     * Plan for users to see when large features will be available 
     * Partners will be able to see what's not getting worked and what they can own
* _In Progress_: SIG completes feature matrix and provides requested dependency graphs: ETA March 21st
	*  lmbr-pip pushing initial PRs from working group, open to SIG wide review

## Open Discussion Items
None proposed
