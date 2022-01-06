# # O3DE Network SIG - Issue Triage Guide

# Overview
This guide covers how to triage GitHub issues for SIG-Network. Maintainers are encouraged to use and update this guide to ensure
any contributor to SIG-Network understands how issues are handled.


##  Issue Triaging
Triaging is the process used to handle intake of issues into the SIG-network backlog. The process aims to ensure issues are both relevant to SIG-Network
and contain sufficient information so that the community can take action.

Process aims to ensure:
* The issue is appropriate for SIG-Network.
* The issue has clear information as to the nature of the issue.
* Issues are regularly maintained and updated until they are resolved.  
* That reported issues are an actual issues, rather than a request for help or an issue for another SIG.


# Process
SIG-Network will triage issues once a week on [Thursdays](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39342&eventid=1263668&calstart=2022-01-20). Anyone is welcome to attend. Triage will be led by SIG chair or maintainer.

Triaging aims to:
* Ensure issues in backlog are in a ready state for the community to take action upon.
* Ensure load is balanced across SIG maintainers and participants.
* Involve the SIG-Network community so all can participate.

If time permits, on the day of triage and before the meeting, create a new thread in SIG-Network and add triage links below.
* Set the thread to automatically archive after 24 hours.

## Triage Links
* Main O3DE repository: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-triage+label%3Asig%2Fnetwork 
* Multiplayer Sample: https://github.com/o3de/o3de-multiplayersample/issues
* NetSoak Test: https://github.com/o3de/o3de-netsoaktest/issues

## Brief Overview
1. Join the SIG-Network discord voice channel
2. Start with main repro link
3. Process all new main repository issues. New issues in main repository should have labels `needs-triage` and `sig/network`
   1. Announce issue number and title to those in Discord voice channel so others can follow along
4. Ensure issue is for SIG-Network. If the issue is not for SIG-Network, remove the `sig/network` label and comment on the issue. If the correct SIG is known 
    assign issue to that SIG. Otherwise add the `needs-sig` label so the general O3DE issue triage meeting can find the appropriate owners.
5. Review the issue and comments and see if it can be accepted
6. Review the technical implications. If a large change, issue should become an RFC, ask requestor to bring issue back as RFC or convert to a feature request, if that would be more appropriate.
7. Assign a reviewer, if required, to handle follow-up comments, to reproduce the issue or ask questions.
8. If issue is rejected, assign commenter to reject issue. 
9. If issue is accepted, remove `needs-triage` label, set priority for issue and add `triage/accepted` label.
10. If issue is a bug add `kind/bug`, if a feature request add `kind/feature`  or `kind/enhancement` as appropriate.
11. For Multiplayer and NetSoak issues, look for issues less than 14 days old that do not have priorities attached.
    1. These repositories do not have the full set of labels, so some of the above process may not apply directly.


If time permits:
* Review any open [blocker](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+label%3Apriority%2Fblocker) and [critical](is:issue is:open label:sig/network label:priority/critical) issues in the main repository:
  * Ensure priority is still valid
  * Assign any required commentators or ask for updates
* Review any issue open for more than [90 days](is:issue is:open label:sig/network sort:created-asc) and see if issue is still valid.
* Review any open `needs-triage` and `needs-sig` issues that may be for SIG-Network in [O3DE issues](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-sig+label%3Aneeds-triage+).

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

SIG will periodically audit for stale items. If during triage, you encounter stale issues, use the guidance below to see if issue should be closed.

## Sig Assigned But No Action
If an issue with the SIG-Network label has had no updates for a while (14 days), followup up with the SIG, either through
Discord chat channel, triage or standard meeting. Consider attending a SIG-Network meeting to raise the issue for discussion.

## No Activity for 90 days
An issue can be removed if it has been abandoned by the requestor. Issues are considered abandoned if there has been no active for 90 days, esp if issue has had `triage/needs-information` label with no followup from requestor.

Part of this guide was informed by the [Kubernetes Triage Guide](https://github.com/kubernetes/community/blob/master/contributors/guide/issue-triage.md)