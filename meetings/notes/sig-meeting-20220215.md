## Meeting Details

- **Date/Time:** [Feb 15, 2022 @ 05:00pm UTC / 12:00pm ET / 9:00 AM PT](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1263672&calstart=2022-02-15)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/44)

## SIG Updates

**What happened since the last meeting?**

* SIG made request to SIG-Content for new [multiplayer Sample project template](https://github.com/o3de/o3de/issues/5730) as starting point for networked games
* SIG made request to SIG-Content to drop [MultiplayerGem for default template](https://github.com/o3de/o3de/issues/7478) as it was not correctly setup.
* SIG needs to patch OpenSSL, AWS finding resources to start work.

## Networking
* Bug fixes for desyncs related to rewindable rigid bodies (https://github.com/o3de/o3de/pull/7534)
* Work continues on new desync debug tools
* Work on changes to deactivate components, such as NetBindComponents, that cause assets/disconnection issues
* Working on issues relating to network prefabs, uncovered some issues with nested prefabs thats blocking new alias pipeline work.

## CloudServices
* Work on refactoring multiplayer session interface (https://github.com/o3de/o3de/pull/7338)
* Work on providing build scripts for GameLiftServer SDK (https://github.com/o3de/3p-package-source/pull/93)

## Meeting Agenda
1. Covered notable updates (see above)

2. Feature Matrix work

## Outcomes from Discussion topics

SIG contributed initial breakdown, Royal/TSC have started work on presentation/completion of data
- TSC: Have a tool to submit: https://o3de.github.io/community/features/form.html
- Code is in the repro: https://github.com/o3de/community/tree/main/features
- Discussion about tool: https://github.com/o3de/community/discussions/113 - looking for comments and feedback on matrix on this issue.

TSC would like to get dependency graphs for features, before GDC

Submit changes via PR to sig-community for data.

## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220201.md):
* _No Update_: O3DE Marketing would like screenshots of MPS running on mobile (@lmbr-pip)
     * Waiting on clarification of marketing needs, focus and content strategy for O3DE.
* _No Update_: Requests for blog posts around networking items: Entity Hierarchies, input scripting. 
     * SIG working on new documentation and tutorials first.
* _In Progress_: Migrate issue backlog fully to GitHub Issues: ETA now Feb 28th
     * Migrated most of critical issues. 
* _In Progress_: Remove blockers to public roadmap updates for SIG-Network: ETA Feb 28th
     * Working with roadmap owners to get items on roadmap.  
* _Completed_: Create issue to provide Network feature breakdown for feature matrix (@lmbr-pip)

New Items:
* _TODO_: SIG completes feature matrix and provides requested dependency graphs: ETA March 21st (All)

## Open Discussion Items

1. Starter guides
* Currently have some basic videos about enabling Multiplayer for O3DE but rather bare bones as starter guides.
* SIG contributors have a new video/tutorial in the works, currently being discussed with docs & community, around creating network controller 
* Royal recommended this as a good example of tutorial video with chapters: See https://www.youtube.com/playlist?list=PL80dW98K-QMe5jvzvXlln0utWgJ9sCIfR as an example of content that folks may want to see.
* O3DE has a resource coming on board for tutorial help, plus budget, if SIG needs help
