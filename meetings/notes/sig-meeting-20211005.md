# Networking SIG  meeting - 2021-10-05

**Facilitator/Note taker:** Pip Potter (lmbr-pip)

[Agenda](https://github.com/o3de/sig-network/issues/12)

## SIG Updates

### General Notes
* Notes from last meeting - https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20210914.md
* Next meeting will be 10/26 due to [O3DE Con](https://o3decon2021.sched.com/) clash on 10/12
* [O3DE Game Jam](https://itch.io/jam/o3de-jam-1) is happening Oct October 18th - November 15th 2021
* Added new ```final-comment-period``` label to attach to RFCs in 5 day final comment period

### Networking Updates
* Moved core components from MultiPlayerSample to Multiplayer gem (ie Rigid body support)
* Entity migration logic inc rigid bodies when they migrate from one authority to another
* Network hierarchies first round of changes has been submitted with unit tests and benchmarks
* Lots of optimizations including looking up the net bind components
* Reworked the way networked entities spawned 
     - replaced custom code and marker components that broke entity id references, prevented parent child relationships
     - code has been reworked to use standard spawnables
* Relocated some of the jinja templates to work with engine as an SDK
* Some doc updates

### CloudService Updates
* Moved AWS C++ SDK packages into O3DE 3P Package System
     - Updates to SDKs are in works, but delayed
     - Providing build scripts so anyone can update and built SDKs
* Working on adding matchmaking to AWS GameLift Gem
* Making a clean-up pass on scripting extensions

## Agenda item 1 - Review Charter

[Charter](https://github.com/o3de/sig-network/blob/main/governance/SIG%20Network%20Charter.md)

Noticed that amendments asked for in June were not made to the charter:
* Need to add responsibilities ie ```Responsible for communication, transport, protocols  and connectivity in O3DE (See in Scope)```
    * Does O3DE have any pillars / tenets we can lean on?
* Remove: "for identity, matchmaking, profiles, persistence."
* Complete SIG Links and lists
* Fix "SIG Docs adheres to the standards for roles and organization management as specified by . " line

For Networking add (some of these are from [June meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20210624.md))
* Need to call out encryption as a cross-cutting issue with sig-security
* Need to call out testing as a cross-cutting issue with sig-testing
* Add callout that Networking owns networking specific serialization (as part of the transport layer)
* Add callout that Sig networking should maintain Multiplayer Sample project

Out of scope:
* Versioning of communication protocols
* sig-network does not support peer-to-peer currently, O3DE only focuses on server client

Cloud add / change (some of these are from [June meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20210624.md))
* Remove:  Define and implement 3rd party distribution platform services interfaces
* Add: contributors submitting cloud gems are responsible for support the gems 
* Update: Not responsible for cloud or network services or interopability with such.




## Agenda Item 2 - Open RFCs
Move to close the following RFCs that have been in final comment period:

### [Scriptable Networking Input](https://github.com/o3de/sig-network/issues/6)
Only comment was a general concern about network scripting that will be addressed in another RFC as its not specific to this change

Proposed for acceptance by Gene. Seconded by Pip, Sergey and Karl. No objections raised in meeting.

**RFC accepted by SIG**

### [Entity Hierarchies](https://github.com/o3de/sig-network/issues/5)

Had concern about order of sibling processing:
- Multiple child at the same level - are there concerns around sibling orderings, is processing consistent/definable?
- Do normal transform process children in a fixed order? It depends on how entities connect to their buses
- There is possibly an issue with order? We should define the order ie by entity id

Request that RFC adds note about determinism for sibling input processing.

Proposed for acceptance by Olex. Seconded by Pip, Karl, Rajiv and accepted by SIG. 
No objections raised in meeting.

**RFC accepted by SIG**

## Agenda Item 3 - Maintainer Proposals

Had two maintainer issues open:
* https://github.com/o3de/sig-network/issues/11
    * 3 maintainers in agreement, no objections in meeting
    * **Maintainer accepted by SIG**

* https://github.com/o3de/sig-network/issues/10
    * 1 maintainer in agreement, needs more maintainer to support promotion.
    * **No Decision** - will revist in next sig meeting

## Agenda Item 4 - Networking Bug Bash
Proposed that we have community-organized bug bash for networking.
However, only 8 bugs in the backlog: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+
- Proposed that a bug bash may still be useful to  uncover testing and performance issues and encourage community engagement
- Add testing, stress testing as part of this effort

- Discussing proposed that we revisit  post Game Jam
- JT: has a lot of experience running bug bash, will work with Stephen to discuss this offline, may be a cross SIG effort


### Action items
Pip: Follow up on DFAD for public roadmap (carried over from last time, still in progress)
- Also provide a maturity matrix for networking components: what is stable, in active development, pending etc.

Pip: Make updates to charter and bring back for review

Pip: Move accepted RFCs to RFC archive.

Pip: Promote Junbo75 to maintainer.
