# # O3DE Network SIG - Issue Triage Guide

# Overview
This guide covers triaging issues for SIG-Networking. Maintainers are encouraged to use and update this guide to ensure
any contributor to sig-network understands how issues are handled.


##  Issue Triaging
Triaging is the process of ensuring a smooth intake of issues into sig-network backlog to ensure issues are both relevant to sig-network
and contain sufficient information so that the community can take action.

Process aims to ensure:
* The issue is appropriate for sig-network.
* The issue has clear information as to the nature of the issue.
* Issues are regularly maintained and updated until they are resolved.  
* The issue is an actual issue, rather than a request for help or an issue for another SIG.


# Process

sig-network will triage issues once a week on [Thursdays](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39342&eventid=1263668&calstart=2022-01-20). Anyone is welcome to attend. Triage will be led by SIG chair or maintainer.

Triaging aims to:
* Ensure load is balanced across sig maintainers and participants.
* Involve the sig-network community so all can participate.

If time permits create a new thread in SIG-network and add triage links below.

## Triage Links
* Main repro: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-triage+label%3Asig%2Fnetwork 
* Multiplayer Sample: https://github.com/o3de/o3de-multiplayersample/issues
* NetSoak Test: https://github.com/o3de/o3de-netsoaktest/issues

## Brief Overview
1. Join the SIG-network discord voice channel
2. Start with main repro link
3. Process all new issues. New issues in main repro should have labels `needs-triage` and `sig/network`
   1. Announce issue number and title to those in voice channel
4. Ensure issue is for sig-network. If the issue is not for sig-network, remove the sig/network` label and comment on the issue. If the correct sig is known 
    assign it to that SIGm otherwise the general O3DE `needs-triage` meeting will then triage and find the appropriate owners.
5. Review the issue and comments and see if it can be accepted
6. Review the technical implications. If a large change, issue should become an RFC, ask requestor to bring back as RFC.
7. Assign a reviewer, if required, to handle follow-up comments, repro, questions or comments.
8. If issue is rejected, assign commenter to reject issue
9. If issue is accepted, remove `needs-triage` label, set priority for issue and add `triage/accepted`
10. For Multiplayer and NetSoak issues, look for issues less than 14 days old that do not have priorities attached.
    1. These repros do not have the full set of labels.


If time permits:
* Review any open and critical issues
  * Ensure priority is still valid
  * Assign any required commentators or ask for updates
* Review any open `needs-triage` and `needs-sig` issues that may be for sig-network

## Issue Workflow

If you are assigned an issue to validate, work with requestor to get enough information to validate the issue.

If it can be reproduced then:
* Add comment and add `triage/accepted` labels
* Define priority with SIG Chair(s)
* Ensure issue is not a duplicate

If issue cannot be reproduced then:
* Comment on the issue and ask the requester for more information to aid reproduction, add `triage/needs-information`
* Or close the issue if both parties agree that this is not an issue/not reproducible.

If the issue is not clear or needs more information
* Comment on the issue and add the `triage/needs-information` label to show that the requestor needs to provide more information.

# Labels for Contributors
Consider adding `good-first-issue` for new contributors for the SIG.

Consider adding `help-wanted` for issues that do not have immediate resourcing, and external contributors can likely contribute to.


# Stale issues

## Sig Assigned But No Action
If an issue with the sig-network label has had no updates for a while (14 days), followup up with the SIG, either through
Discord chat channel, triage or standard meeting. Consider attending a sig network meeting to raise the issue for discussion.

## No Activity for 31 days
An issue can be removed if it has been abandoned by the requestor. If there has been no active for 31 days, issue can be considered abandoned 
and can be closed out.
