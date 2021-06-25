# Networking SIG  meeting - 2021-06-24

**Facilitator:** Derek Bronson (dbbronso)
**Notes taker:** Pip Potter (lmbr-pip)

[Agenda](AGENDA_ISSUE_#)

## SIG Updates

Initial meeting of SIG, no updates.

[Agenda](https://github.com/o3de/sig-network/issues/1)

## Agenda item 1 - Review Charter (Networking)

[Charter](https://github.com/o3de/sig-network/blob/main/governance/SIG%20Network%20Charter.md)
* Stephen Tramer pushed a new PR to fix formatting: https://github.com/o3de/sig-network/pull/2

[x] Need to call out encryption as a cross-cutting issue with sig-security
* Networking offers full DTLS for UDP
* Networking owns the code, but security needs to review the code/design decision
               
Discussed if we own networking simulation tools for testing ?
* Does this relate to headless/Bots? Do we need a framework? Or Headless clients?
* Multiple servers, multiple clients, peer-to-peer (not in scope for O3DE)
* Doable from the Multiplayer Sample POV, bot testing is very game specific
    * May have demonstration here for accuracy, hit detection etc

[x] Cross-cut with testing and we deliver recommendations about testing
* testing sig provides framework based on our recommendations
* Orchestrating layer for testing at scale (ala Battle Royale), may include port allocation
 
Maybe callout that we handle reconciliation, local-prediction etc?
* Not doing determinism, not a rollback model
* Its component driven so devs can remove local prediction and other methods
 
[x] Add callout in docs that we do not support peer-to-peer currently, O3DE only focuses on server client
* clients can host, but client is the server.
 
[x] Add callout that Networking owns networking specific serialization (as part of the transport layer)
* Sig owns compression, security and serialization relating to networking, separate from core serialization
 
[x] Maintaining Multiplayer Sample should be called out as in scope
 
Do we need to add versioning to the Charter?
- What is the backwards compatibility strategy? We don't support versioning currently.
- Has impact on serialization
[x] Add that we own protocol negotiation
  
### Open questions

### Action items

* Need to call out encryption as a cross-cutting issue with sig-security
* Need to call out testing as a cross-cutting issue with sig-testing
* Add callout in docs that we do not support peer-to-peer currently, O3DE only focuses on server client
* Add callout that Networking owns networking specific serialization (as part of the transport layer)
* Add callout that Sig networking should maintain Multiplayer Sample project

## Agenda Item 2 - Review Charter (Cloud)

[Charter](https://github.com/o3de/sig-network/blob/main/governance/SIG%20Network%20Charter.md)
Cloud is in scope in charter
* encompasses any cloud service integration not just those for client->server/games
 
Discussed making a common service interface?
- wrap specific implementations behind generic interfaces
- potential cross-cuts with sig-platform
- would need async, request/response wrapper
 
Session providing services?
- Could wrap generic interfaces around specific services, maybe a good starting point for RFC

### Open questions
* Companies submitting cloud gems must commit to support the gems.
- Unclear if this should be in the charter?

* Ensure that common interfaces work across cloud platforms, we cannot force specific services on customers
- Need to bring back as an item for discussion? No clear action item identified in meeting.

### Action Items
* None


## Agenda Item 3 - Agree on Meeting Cadence
Have folks in European and AP timezones
 
Schedule meetings with EU/Pacific friendly times, can have multiple meetings per meeting time
 
Agreed to hold timezone friendly meetings
- Meeting every two weeks
- Every other meeting, switch timezones from (9am PDT) EU  to (9PM PDT) Asia Pacific
 

## Miscellaneous

Was there crosstalk or other useful discussion that happened during the meeting, but wasn't _specifically_ related to any of the agenda items?

## Postponed due to time

None

## Action Items
Derek: Will make RFC for Sig Charter to update with action items from Agenda
- Will followup with commenters about points made to ensure everything is captured

Derek: Will update meeting scheudle