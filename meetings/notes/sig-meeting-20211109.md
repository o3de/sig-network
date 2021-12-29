## Meeting Details

- **Date/Time:** November 9th, 2021 @ XX:XXpm UTC / XX:XXpm ET
- **Location:** [Discord SIG-Network Voice Room](https://discord.gg/62nq7HP5mP)
- **Moderator:** Pip Potter (lmbr-pip)
- **Note Taker** Pip Potter (lmbr-pip)

[Agenda Issue](https://github.com/o3de/sig-network/issues/19)

## SIG Updates

### Networking
* Changes around running on Linux
* Changes on stability on debugging the editor experience around ctrl+g
* Fixes in the transport layer wrt timeout queues, heartbeats and disconnects

### CloudServices
* Updated GameLift documentation for server testing and flexmatch
* Various bug fixes wrt GameLift inc unit tests, gem.json fixes 

## Meeting Agenda

* Discuss requirements for server targets - logging, truly headless etc. 
* Cover current state of MultiplayerSample - suggestions for improvements.
* Performance metrics - What maybe useful performance metrics for SIG to track?


## Outcomes from Discussion topics

Q: Server targets?
- Architecture is componentized, so you can disable many or replace components to meet a specific games requirements.
    - ie disable server rollback
- Monolithic builds are currently disabled - DLLS are support issue.
- Build targets
    - Support for Graviton64 was requested
    - Containerization has highlighted problems in other projects such as fixed ports.
        - Incremental builds on containerized assets can also highlight issues
        - Has not been tested currently, various SIGs are tracking containerization as potential effort.
        - Running in EKS could be a goal.

Q: Do we have test levels that show different networking features/game types?
- We have an action FPS genre sample
- Limited resources require us to have a focus for samples

Q: Performance?
	- SIG could set goals but no clear metrics discussed
	- Engine is not targeting a limited subsets
	
Q: Which hurdles are most interesting?
- Running on mobile. 
- Add mobile input - can be faked 
- Add extra things, dial up, scale till it breaks
- Demonstrate rollback and culling.
- Demonstrate network primitives are reliable
- IMGUI to demonstrate throughput

## Action Items

* Cut issue to support monolithic server builds (@lmbr-pip)
* O3DE Marketing would like screenshots of MPS running on mobile (@lmbr-pip)
* Requests for blog posts around networking items: Entity Hierarchies, input scripting
  * @lmbr-pip to cut issues to highlight networking updates
* Add containerization of runtimes to feature backlog (@lmbr-pip)
* Ensure requirements around mobile are captured in future demo planning (@lmbr-pip)

## Open Discussion Items

Q: New World networking and how is O3DE networking exposed?
- We do not share any code with New world / Lumberyard
- O3DE Networking is fully server authoritative
- Really the focus should be on what inputs you send, the rate should be carefully guarded. Networking has a lot of guards to prevent abuse. But you can generate designs and changes that can circumvent guards
- Basically be very careful what you put inside inputs sent from the client.
- O3DE currently doesn't have networked chat or other features from O3DE.

Chat is out-of-scope unless samples require it.

Encourage community to raise security concerns
- Many contributors work for AWS, so security trumps new features as a central tenet
- Encourage folks to experiment and raise any concerns


Q: Someone did a shader toy implementation for O3DE?
- Bringing web browser into O3DE, how to approach?
- Requires frequent patching, so cost of ownership is high so unlikely to be central.
- We have a basic HttpRequestor to support fetching data in RESTFul way.

