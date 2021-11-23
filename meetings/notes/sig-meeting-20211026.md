# Networking SIG  meeting - 2021-10-26

**Facilitator/Note taker:** Pip Potter (lmbr-pip)

[Agenda](https://github.com/o3de/sig-network/issues/16)

## SIG Updates

### General Notes
* Notes from last meeting - https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20211005.md
    * (No Update) Still waiting on public roadmap
    * (Update) Charter updates are in PR: https://github.com/o3de/sig-network/pull/15
* O3DECon happened, videos are now on youtube channel: https://www.youtube.com/c/Open3DEngine

### Networking Updates
* Submitted code for hierarchy code work and network scripting input (see recent RFCs)
* Adding various bug fixes and integration tests
* Adding automation and periodic builds for NetSoakTestTest
* Added new benchmarks for performance, building this out

### CloudService Updates
* Delivered code for Linux GameLift support
* Delivered code for GameLift FlexMatch support
* Various bug fixes
* Working on automation improvements

## Meeting Agenda

* Charter updates: https://github.com/o3de/sig-network/pull/15 
* Maintainer Nominations: https://github.com/o3de/sig-network/issues/10 
* Open RFCs review: https://github.com/o3de/sig-network/issues?q=is%3Aissue+is%3Aopen+label%3Arfc-feature

## Outcomes from Discussion topics

* Brief discussion about charter.
* Maintainer nomination accepted by SIG.
* GameLift FlexMatch RFC accepted by SIG.

## Action Items

* (lmbr-pip): Update charter with feedback from meeting. Will reach out to Royal and others around overview.

## Open Discussion Items

Q. How should external contrbutors think about adding cloudservices?
* Have a team interested in implementing around Digital Ocean?
    * What is the state around documentation / guidance
    * Do we have a formula / recipe for using another Provider?
* We do not have abstract feature layer/API that service providers can plug into currently. This is something in the charter, but is not defined anywhere. This would require RFCs, discussions which have not happended yet.
* Providers can still build into O3DE. Would encourage folks to bring RFCs, feature requests etc in this area.
