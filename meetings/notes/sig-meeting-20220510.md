## Meeting Details

- **Date/Time:** [May 10, 2022 @ XX:XXpm UTC / XX:XXpm ET](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1305634&calstart=2022-05-10)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Gene Walters (AMZN-Gene)

[Agenda Issue](https://github.com/o3de/sig-network/issues/56)

## SIG Updates

**What happened since the last meeting?**

### General SIG announcements
* Next O3DE release on track for 05/12.
* Open Source Path finding is being added to the engine.

### Networking
* Working on planning to move TargetManagement to own Gem to support other tooling paths
* Walk the Engine sessions (WtE) are happening for multiplayer, generating lots of issues to be fixed to make networking easier to use.
* Work continues on new spawnable alias multiplayer pipeline
* More PIX performance testing.

### Cloud Services
* Delivering fixes for Cognito spam at startup.
* Fixing issues in HttpRequestor Gem relating to unwanted calls to EC2 metadata service (due to use of HttpClient from AWS C++ SDK)

## Meeting Agenda
* Proposing moving SIG meeting to monthly cadence.

## Outcomes from Discussion topics
* Created discussion vote for move to monthly cadence, see https://github.com/o3de/sig-network/discussions/58
   * Move is to align with other SIG cadence
   * Spend more time on feature / roadmap focus for SIG general meeting.

## Open Discussion Items
Have a open request for input on Multiplayer Starter Pack Gem (See https://github.com/o3de/o3de/issues/8281)

## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220412.md):
* _No Update_: Need to remove blockers to public roadmap updates for SIG-Network: ETA May 2022 (@lmbr-pip)
     * Still working with roadmap owners to get items on roadmap. Delayed by change of ownership.
* _In Progress_: SIG completes feature matrix and provides requested dependency graphs
    * Setup working group again to complete matrix due to loss of original notes
    * lmbr-pip will update and push PR changes https://github.com/o3de/community/pull/128

* New: Decide on new meeting cadence in 5/24 meeting 
     * Announce and update calendar 

