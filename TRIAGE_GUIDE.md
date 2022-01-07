# # O3DE Network SIG - Issue Triage Guide

# Overview
This guide covers how to triage GitHub issues for SIG-Network. Maintainers are encouraged to use and update this guide to ensure
any contributor to SIG-Network understands how issues are handled.


##  Issue Triaging
Triaging is the process used to handle intake of issues into the SIG-network backlog. The process aims to ensure issues are both relevant to SIG-Network
and contain sufficient information so that the community can take action.

Process aims to ensure that:
* Issues are appropriate for SIG-Network. Issues are an actual issues, rather than a request for help or an issue for another SIG.
* Issues have clear information to enable SIG-Network to address the problem or request.
* Issues are regularly maintained and updated until they are resolved.  
* Load is balanced across SIG maintainers where action is required.
* All the SIG-Network community can participate.

# Process
SIG-Network triages issues once a week on [Thursdays](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39342&eventid=1263668&calstart=2022-01-20). Anyone is welcome to attend. Triage will be led by SIG chair, co-chair or maintainer (referred to below as *Triage Leader*)

If time permits, prior to the start of meeting, Triage Leader will create a new thread in SIG-Network Discord channel and add the repositories to triage links below.
* Recommendation is that Triage Leader sets the thread to automatically archive after 24 hours.

## Repositories to Triage
* Main O3DE repository: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-triage+label%3Asig%2Fnetwork 
* Multiplayer Sample: https://github.com/o3de/o3de-multiplayersample/issues
* NetSoak Test: https://github.com/o3de/o3de-netsoaktest/issues

## Triage Leader Guide
1. Join the SIG-Network discord voice channel
2. Announce yourself as Triage Leader and wait a few minutes for others to join the call.
3. Use the *Individual Issue Triage* guide below to process all new issues for SIG:
   1. Process all [new main repository issues](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-triage+label%3Asig%2Fnetwork )
   2. Process all the new [MultiplayerSample](https://github.com/o3de/o3de-multiplayersample/issues) and [NetSoak](https://github.com/o3de/o3de-netsoaktest/issues) issues in a similar way.
      1. Look for issues less than 14 days old that do not have priorities attached. 
      2. Note: These repositories do not have the full set of labels, so parts of individual issue triage process may not apply directly.

If there are questions about what to do with an issue please raise questions with SIG Chair(s) or start a conversation in SIG-Network.

### Individual Issue Triage
1. (Recommendation) Announce issue number and title to those in Discord voice channel so others can follow along. 
2. (Main issues only) Ensure issue is for SIG-Network. If the issue is not for SIG-Network, remove the `sig/network` label and comment on the issue. If the correct SIG is known 
    assign issue to that SIG. Otherwise, add the `needs-sig` label so the general O3DE issue triage meeting can find the appropriate owners. 
3. Review the issue and comments to see if it can be accepted. 
   1. If issue is a bug, does it have enough information for someone to reproduce/understand the issue? Do we understand platforms issues is reported against? 
   2. Review the technical implications of a feature request. If it's a large change then issue should become an RFC or be brought to SIG-Network meeting for discussion. Ask requestor to bring issue back as RFC or as a discussion topic and add to next SIG-Network meeting agenda, if that would be more appropriate. 
   3. Assign a reviewer, if required, to handle follow-up comments, to reproduce the issue or ask for further clarifying information. 
   4. If issue is a bug add the `kind/bug` label, if issue is for a feature request add `kind/feature`  or `kind/enhancement` as appropriate.
4. If issue is rejected, assign commenter to reject issue and provide reason for rejection.
5. If issue is accepted, remove `needs-triage` label, set priority for issue and add `triage/accepted` label. 

### Additional Labels to Consider for Contributors
* Consider adding `good-first-issue` for new contributors for the SIG, if issue is straightforward to fix (config, docs, comments or test changes are good candidates).
* Consider adding `help-wanted` for issues that do not have immediate resourcing, and external contributors can likely contribute to.

If time permits:
* Review any open [blocker](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+label%3Apriority%2Fblocker) and [critical](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+label%3Apriority%2Fcritical) issues in the main repository:
  * Ensure priority is still valid
  * Assign any required commentators or ask for updates
* Review any issue open for more than [90 days](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+sort%3Acreated-asc) and see if issue is still valid.
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

# Stale issues

SIG will periodically audit for stale items. If during triage, you encounter stale issues, use the guidance below to see if issue should be closed.

## Sig Assigned But No Action
If an issue with the SIG-Network label has had no updates for a while (14 days), followup with the SIG, either through
Discord chat channel, triage or standard meeting. Consider attending a SIG-Network meeting to raise the issue for discussion.

## No Activity for 90 days
An issue can be removed if it has been abandoned by the requestor. Issues are considered abandoned if there has been no active for 90 days, esp if issue has had `triage/needs-information` label with no followup from requestor.

Part of this guide was informed by the [Kubernetes Triage Guide](https://github.com/kubernetes/community/blob/master/contributors/guide/issue-triage.md)