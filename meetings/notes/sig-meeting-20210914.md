# Networking SIG  meeting - 2021-09-14

**Facilitator/Note taker:** Pip Potter (lmbr-pip)

[Agenda](https://github.com/o3de/sig-network/issues/4)

## SIG Updates

### Networking Updates
Updates to Networking code since last public meeting
- Bug fixes to existing features including crtl+g experience and session termination
- Added preliminary filtering for entities
- Work to the interpolation and motion of networked entities
- General improvements around code gen to help with stability and readability
- Brought in some new Multiplayer analytics and ImGUI features for debugging
 	- Traffic rates, data for specific entities
- Change to specific packets as part of the handshake logic, provide own methods to verify handshake after the AZNetwork handshake completes

- Doc updates are on the way

### CloudService Updates
* Moved AWS SDK Building into O3DE 3P Package System
* Providing build scripts so anyone can update and built SDKs
* Delivering GameLift integration changes

### Other Updates
sig-network public [issue triage](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39342&eventid=1262761&calstart=2021-09-09) is every Thursday at 10.30-11am PT

[O3DE Con](https://o3decon2021.sched.com/) is October 11-12th 2021
- Has networking talks planned

[O3DE Game Jam](https://itch.io/jam/o3de-jam-1) is happening Oct October 18th - November 15th 2021


Have 2 open RFCS in sig-network:
1. [Scriptable Network Input](https://github.com/o3de/sig-network/issues/6)
1. [Network Entity Heirachies](https://github.com/o3de/sig-network/issues/5)


## Agenda item 1 - Updated Charter

[Charter](https://github.com/o3de/sig-network/blob/main/governance/SIG%20Network%20Charter.md)
- No new additions proposed
- Will review updates in detail in next meeting

## Agenda Item 2 - Meeting Cadence
[sig-network pubic meeting](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1262939&calstart=2021-09-14) will be every two weeks at 9am PT, every other tuesday
- SIG to regularly check in if we have members that cannot make this time and need to run more Asia Pacific friendly times.
- Need owners to run sig meetings during this alternative time, if required.



### Open questions
Community-organized bug bash for networking?
- No one to represent this item (will be added to next meeting agenda)

Had question about state of code for bug bash
- Demo code state? Karl has posted videos of current demo code.
- Team would love to get input about Multiplayer sample 

Public Roadmap for Network?
Raised an issue that without visibility community can waste time on issues.
Without roadmap, everyone is encouraged to engage with SIG on discord, cut issues and RFCs in the mean time

Guidance from Karl:
- everything on our backlog prior to launch is whats required to build a top tier AAA
- core starter of networking, with weapons and vehicles
- Delivering into multiplayer gems and sample

We do have an Minimum Loveable Product (MLP) list with whats critical - not public at this time

Royal: will help if team can provide 1 liners with maturity


### Action items
Pip: Add community bug bash idea / community engagement questions from this meeting to the agenda of the next meeting

Pip: Follow up on DFAD for public roadmap
- Also provide a maturity matrix for networking components: what is stable, in active development, pending etc.

