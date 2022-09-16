# O3DE Network SIG - Issue Triage Guide

## Overview
This guide covers how to triage GitHub issues for SIG-Network.

Maintainers are encouraged to reference and update this guide to ensure
all contributors to SIG-Network understand how issues are handled and accepted by the SIG.

This guide uses the [general SIG triage guide](https://github.com/o3de/community/blob/main/sigs/docs/sig-issues-triage-guide.md) as a base.

##  Issue triage
Triaging is the process used to handle intake of issues into the SIG-network backlog. The process aims to ensure issues are both relevant to SIG-Network
and contain sufficient information so that the community can take action.

Process aims to ensure that:
* Issues are appropriate for SIG-Network. Confirms that issues are actual issues, rather than requests for help or issues for another SIG.
* Issues have clear information to enable SIG-Network to address the problem or request.
* Issues are regularly maintained and updated until they are resolved.  
* Issue load is balanced across SIG maintainers when action is required.
* All the SIG-Network community can participate.

# Triage process
SIG-Network triages issues once a week on [Thursdays](https://lists.o3de.org/g/o3de-calendar/viewevent?repeatid=39342&eventid=1263668&calstart=2022-01-20). Anyone is welcome to attend. Triage will be led by SIG chair, co-chair or maintainer (referred to below as *Triage Leader*)

If time permits, prior to the start of meeting, the Triage Leader will create a new thread in the SIG-Network Discord channel and post the links from the *Triage Links* section.
* Recommendation is that the Triage Leader sets the thread to automatically archive after 24 hours.

## Links for triage
1. Open issues with `needs-sig` label: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-sig
2. Main O3DE repository: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-triage+label%3Asig%2Fnetwork 
3. Multiplayer Sample: https://github.com/o3de/o3de-multiplayersample/labels/needs-triage
4. NetSoak Test: https://github.com/o3de/o3de-netsoaktest/issues

## Triage leader guide
1. Join the SIG-Network discord voice channel
2. Announce yourself as the Triage Leader and wait a few minutes for others to join the call.
3. Use the *Individual Issue Triage* guide below to process all new issues for SIG:
   1. Review any open issues with [needs-sig](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-sig) that may be for SIG network
        1. Remove `needs-sig` and add `sig/network`. These items will now show up when reviewing issues below.    
   2. Process all [new main repository issues](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Aneeds-triage+label%3Asig%2Fnetwork)
   3. Process all the new [MultiplayerSample](https://github.com/o3de/o3de-multiplayersample/labels/needs-triage) and [NetSoak](https://github.com/o3de/o3de-netsoaktest/issues) issues in a similar way.
      1. Look for issues less than 14 days old that do not have priorities attached. 
      2. Note: NetSoakTest and MultiplayerSample repositories do not have the full set of labels, so parts of individual issue triage process may not apply directly.

If there are questions about what to do with an issue please raise questions with SIG Chair(s) or start a conversation in SIG-Network.

### Individual issue triage guide

#### General triage guidance
* (Optional but highly recommended) Share you screen so others can follow along.
* Announce issue number and title to those in Discord voice channel, so those who are just listening can also follow along
* Pause to let others read and process the information.
* Always Add comments to issue, when appropriate, to capture issue triage decisions.

#### Issue triage 
1. Ensure issue is for SIG-Network ([od3de/o3de](https://github.com/o3de/o3de/issues) issues only)
   1. If the issue is not for SIG-Network, remove the `sig/network` label and comment on the issue as to reason why issue is not for SIG-Network. 
   2. If the correct SIG is known assign issue to that SIG. Otherwise, add the `needs-sig` label so the general O3DE issue triage meeting can find the appropriate owners. 
2. Review the issue and any comments to see if it can be accepted by SIG.
   1. **If issue is a bug**: Check that report has enough information for someone to reproduce or understand the issue.
   2. **If issue is a feature request**: Review the technical implications of the request. 
      1. If it's a large change then issue should become an RFC or be brought to SIG-Network meeting for discussion. Ask requestor to bring issue back as RFC or start a discussion topic. Add the issue to the next SIG-Network meeting agenda, if that would be more appropriate. 
4. If issue can be **accepted** then:
   1. If issue is a bug, add the `kind/bug` label.
   2. If issue is for a feature request, add either `kind/feature` or `kind/enhancement`. Add `feature/networking` or `feature/cloud-service` as appropriate.
   3. Set a priority for issue based on impact (ask other SIG members on call for guidance).  
   4. Mark the issue as `triage/accepted`.
5. If the issue **requires more information** or is **rejected**, then:
   1. Assign a reviewer, if required, to handle follow-up comments, to reproduce the issue or ask for further clarifying information.
   2. **If issue is rejected**: Reviewer/triage leader should reject issue and provide reason for rejection. 
      1. Mark the issue as `triage/declined`.
   3. **If issue needs more information**: Reviewer/triage leader should add clear comments requesting the additional information. 
      1. Mark the issue with `triage/needs-information`. Its recommended that all issues in this state have an assigned reviewer who will track updates until all required information is received. Issue can then be reconsidered for acceptance.
6. Remove the `needs-triage` label from issue if SIG/network is only assigned SIG.
   1. See [general guidance](https://github.com/o3de/community/tree/main/sigs/docs) for issues requiring input from multiple SIGs.

### Additional Labels to consider for contributors
* Consider adding the `good-first-issue` label to identify issues that have straightforward/simple fixes for new contributors to fix. Examples could include config, docs, comments and testing changes.
* Consider adding the `help-wanted` label for issues that do not have immediate resourcing and contributions by others would be welcome.

## Additional triage tasks
If time permits select one or more of the following tasks:

* Review all open [bugs without acceptance](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+-label%3Atriage%2Faccepted) and ensure they have `triage/accepted`.
  * Ensures issue that have been assigned to SIG have been triaged correctly. 
* Review any open [blocker and critical](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Akind%2Fbug+label%3Apriority%2Fcritical%2Cpriority%2Fblocker+label%3Asig%2Fnetwork) issues in the main repository:
  * Ensures priority is still valid. 
  * Ensures issues are still valid.
  * Assign any required commentators or ask for updates.
* Review any PRs for the SIG that are more than [30 days old](https://github.com/o3de/o3de/pulls?q=is%3Apr+is%3Aopen+label%3Asig%2Fnetwork+sort%3Acreated-asc).
  * Ensures PRs are progressing and are not blocked on contributor or maintainer action.
* Review issues open for more than [90 days old](https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork+sort%3Acreated-asc).
  * Ensures issues are still relevant to SIG. 


## Issue Workflow
If you are assigned an issue to validate or reproduce, work with requestor to get enough information to validate the issue.

If it can be reproduced then:
* Add comment to confirm reproduction.
    * Can work with SIG-Chair(s) to add `triage/accepted` labels and define priority or wait for next issue triage meeting.
* Ensure issue is not a duplicate.

If issue cannot be reproduced then:
* Comment on the issue and ask the requester for more information to aid reproduction, add the `triage/needs-information` label.
* Or close the issue if both parties agree that this is no longer an issue or not reproducible.

If the issue is not clear or needs more information:
* Comment on the issue and add the `triage/needs-information` label to show that the requestor needs to provide more information.

# Stale or abandoned issues
SIG will periodically audit for stale items. If during triage, you encounter stale issues, use the guidance below to see if issue should be closed.

## SIG assigned but no action
If an issue with the SIG-Network label has had no updates for a while (14 days), follow-up with the SIG, either through Discord chat channel, triage or standard meeting. Consider attending a SIG-Network meeting to raise the issue for discussion.

## No activity for 90 days
An issue can be removed if it has been abandoned by the requestor. Issues are considered abandoned if there has been no activity for *90* days, especially if issue has had `triage/needs-information` label applied and there has been no follow-up from issue reporter.

## FAQ
1. What should I do if triage rejects my issue?
   1. Issues should be rejected with clear comments that provide reason for rejection. If you disagree or want to discuss the reason please start a chat in SIG-Network or add as an agenda item for SIG-Network's public meeting.
   2. If you still do not support SIG-Network's decision, then please raise with the [O3DE TSC](https://github.com/o3de/tsc).
2. What should I do if I have an urgent issue that cannot not wait for public issue triage?
   1. Please raise the issue in SIG-Network chat and ask for triage, this is so all SIG has visibility.
   2. SIG-Chair(s) can appoint a reviewer to ensure issue is triaged as soon as possible.
   3. If the intent is for you to work on the issue immediately, then please self-assign or work with a maintainer to assign.

## Acknowledgments
Part of this guide was informed by the [Kubernetes Triage Guide](https://github.com/kubernetes/community/blob/master/contributors/guide/issue-triage.md) and by 
the [O3DE SIG Issue Triage guidelines](https://github.com/o3de/community/blob/main/sigs/docs/sig-issues-triage-guide.md), originally written by [ForHalle](https://github.com/forhalle).
