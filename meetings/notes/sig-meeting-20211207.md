## Meeting Details

- **Date/Time:** December 7th, 2021 @ 05:00pm UTC / 12:00pm ET / 9:00 AM PT
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/28)

## SIG Updates

### Networking
* Fixes around project setup changes: logging, benchmark, project upkeep as well as component serialization
* Some bug fixes in the MultiplayerSample

### CloudServices
* AWS C++ SDK Upgrade is in progress, expect to migrate to 1.9.50 (or later)
* Leading initiative to set server build size targets for remote relocation such as on Amazon GameLift or other remote compute.
* AWS Gems had header/source relocation to support autogeneration of code documentation.

## Meeting Agenda
* Review action items for last meeting: https://github.com/o3de/sig-network/pull/2?
* Maintainer proposal : https://github.com/o3de/sig-network/issues/25
* Accepting RFC in final comment period: https://github.com/o3de/sig-network/issues/23
* Discus how to get community more engaged in RFCs

## Outcomes from Discussion topics
* Reviewer accepted by SIG. 
* RFC accepted by SIG
* How to improve RFC participation:
- Most RFCs are from AWS employees so have already had extensive discussion. Do not have mechanism to share internal AWS discussions.
- Mailing list to advertise RFCs 
- lmbr-pip has agenda item on TSC to discuss RFC processes to improve discoverability, engagement
- Central RFCs as a win - https://github.com/o3de/community/discussions/103

## Action Items
Carried from previous meeting(s):
* O3DE Marketing would like screenshots of MPS running on mobile (@lmbr-pip)
* Requests for blog posts around networking items: Entity Hierarchies, input scripting
  * @lmbr-pip to cut issues to highlight networking updates
* Add containerization of runtimes to feature backlog (@lmbr-pip)
* lmbr-pip to promote new maintainer
* lmbr-pip to archive accepted RFC: https://github.com/o3de/sig-network/issues/23
* 
## Open Discussion Items
* Question about entity migration between host/process and the state of the code.
- Karl will followup in discord
- Completed in terms of client host hand over for p2p for host migration.
- Np plans for deprecation but no current RFCs or roadmap items to expand functionality

* Request to understand where we are at with pain points versus nice-to-have
- O3DE has Rev The Engine intitiatve to drive key workflows, which is designed to identify pain points
- Community should engage around pain points they find: Bring as issues, engage in issues

