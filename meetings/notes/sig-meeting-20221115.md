## Meeting Details

- **Date/Time:** [November 15th, 2022 @ XX:XXpm UTC / XX:XXpm ET](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39350&eventid=1557573&calstart=2022-11-15)
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/79)

## SIG Updates

### General SIG announcements
AWS is looking to contribute a [major update](https://github.com/o3de/o3de/issues/13198) to the 3rd person Multiplayer Sample

[O3DCon 2022](https://events.linuxfoundation.org/o3dcon/) talks now on [YouTube](https://www.youtube.com/playlist?list=PLCQwFpnHSZQgzCpMmbxruFkWr3d73ZfEJ). Of interest to SIG:
    * [Building Multiplayer Content for Large Worlds](https://www.youtube.com/playlist?list=PLCQwFpnHSZQgzCpMmbxruFkWr3d73ZfEJ)
    * [Scale Testing Multiplayer Games on AWS](https://www.youtube.com/playlist?list=PLCQwFpnHSZQgzCpMmbxruFkWr3d73ZfEJ)
    * [Keynote: Supercharging Gameworld Performance Using the Cloud](https://www.youtube.com/playlist?list=PLCQwFpnHSZQgzCpMmbxruFkWr3d73ZfEJ)
    * [Keynote: O3DE on AWS The Future of 3D in the Cloud ](https://www.youtube.com/watch?v=jg_BJVNc5Xc&list=PLCQwFpnHSZQgzCpMmbxruFkWr3d73ZfEJ&index=26)
    * [Workshop: Building a Networked Game from Scratch: Multiplayer Drag Racing](https://www.youtube.com/playlist?list=PLCQwFpnHSZQgzCpMmbxruFkWr3d73ZfEJ)

### Networking
* ClientServer mode in the Editor - simplifies Ctrl+G 
* Client/server split work in PR
* Component version mismatch detection in PR
* New scripting sub gem added Multiplayer Gem with sample level.

#### Multiplayer Sample updates
* Content iteration for new sample is underway. All content is planned to be open source.
* New character controller
* Adding gravity to existing "Burt" character
* Adding audio support
* ETA for new sample is Feb 2023, but we expect to deliver initial version sooner into the [o3de/o3de-multiplayersample](https://github.com/o3de/o3de-multiplayersample) repository.

### Cloud Services
* Merged in support for EC2 credentials support in AWS Core Gem
* Unit test update for AWSCore Gem
* Updating the [AWS Cloud Development Kit](https://aws.amazon.com/cdk/) version CDKv2 for all projects/Gems that use it. 
    * Impactful change notice should be posted this week, most likely Nov 16h 

## Meeting Agenda
* Reviewed feature roadmap:
  * AWS added client/server split and networking versioning work to roadmap
  * AWS added new MultiplayerSample work to roadmap: https://github.com/o3de/o3de/issues/13198

* SIG/elections
   * lmbr-pip to look for volunteer external to SIG to help run SIG election, aim to hold elections first week in December so new chair(s) can be in place for December meeting.
      * Election guide for all SIGs: https://github.com/o3de/community/blob/main/O3DE%20Elections/O3DE%20Elections%20Guide.md
   
## Open Discussion Items
* None

## Action Items
Carried from [previous meeting](https://github.com/o3de/sig-network/blob/main/meetings/notes/sig-meeting-20220719.md):
* _TODO_: Need to decide on SIG/network's responsibility towards cloud service gems once they migrate from O3DE/O3DE (may require charter update).

* _NEW_: Find volunteer to run SIG/Network Chair elections