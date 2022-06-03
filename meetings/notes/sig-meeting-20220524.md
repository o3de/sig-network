## Meeting Details

- **Date/Time:** [May 24, 2022 @ XX:XXpm UTC / XX:XXpm ET](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1305634&calstart=2022-05-24)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/57)

## SIG Updates

**What happened since the last meeting?**

### General SIG announcements
* Release 2205 completed: 
* 
### Networking
* Plan being developed to move TargetManagement to own Gem
* Have held some Walk the Engine (WtE) sessions with users to identify network related usability issues, added to [backlog](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3AWF6) of items to address.
* Testing is happening on new spawnable alias multiplayer pipeline to unify spawning between networked and non-networked entities (RFC pending)
* More PIX performance testing.

### Cloud Services
* Log spam fixes for Cognito spam at startup submitted
* Fixing issues in HttpRequestor Gem, AWS C++ SDK to prevent accidental deletion
* Discussed with TSC approach for migrating AWS Gems out of the main [o3de/o3de](https://github.com/o3de/o3de) repro.

## Meeting Agenda
* Reviewed vote on moving to monthly meeting 

## Outcomes from Discussion topics
* 100% positive for [poll](ttps://github.com/o3de/sig-network/discussions/58), lmbr-pip to move meeting to monthly cadence.
  * Will move to the fourth Tuesday of each month, until such time we have more engagement.
  * Public issue triage will remain at same cadence.
  
## Open Discussion Items
Have an open request for input on Multiplayer Starter Pack Gem (See https://github.com/o3de/o3de/issues/8281)

## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220412.md):
* _No Update_: Need to remove blockers to public roadmap updates for SIG-Network: ETA May 2022 (@lmbr-pip)
     * Still working with roadmap owners to get items on roadmap. Delayed by change of ownership.
* _In Progress_: SIG completes feature matrix and provides requested dependency graphs
    * Setup working group again to complete matrix due to loss of original notes
    * lmbr-pip will update and push PR changes https://github.com/o3de/community/pull/128
* _New_: Set up new meeting cadence. Announce to SIG and update calendar 

