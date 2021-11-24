# SIG Network Charter
This charter adheres to the Roles and Organization Management specified in <sig-governance>.
Team information may be found in the [readme.md](https://github.com/o3de/sig-network/blob/main/README.md)

Last Updated: **23rd Nov 2021**

## Overview of SIG
This SIG is responsible for communication mechanisms in O3DE related to multiplayer networking and cloud networking. The SIG defines and owns
communication protocols, methods and approaches to support O3DE client to server, as well as interaction points with cloud services.

Responsibilities include:
* Secure network transport including client+server interactions over TCP or UDP sockets.
* Network state replication.
* Restful HTTP(s) support.

## Goals

- Major goals that SIG seeks to generally achieve: TBD

## Scope

- Generalized overall scope of work

## In scope
- Define standard for communication negotiation.
- Define and maintain implementation to enable compression and secure communication between endpoints.
- Define and implement interface to network counters, monitoring, and statistics.
- Define and implement low level instrumentation APIs.
- Items that are the core responsibilities of SIG.
- Maintenance of network sample projects owned by SIG: [MultiplayerSample](https://github.com/o3de/o3de-multiplayersample), [NetSoakTest](https://github.com/o3de/o3de-netsoaktest).

###Network:
- Design and maintain client and server protocol communication.
- Define, publish, and maintain network protocol and data packet design model.
- Responsible for network presence and state replication.
- Create and Maintain network simulation tools for testing network conditions.
- Design and maintain multiplayer component architecture supporting client hosted and dedicated server strategies.
- Define and maintain specific implementation of multiplayer controller component.
- Networking specific serialization (as part of the transport layer).

###Cloud:
- Define, standardize, and maintain interfaces to feature based cloud services.
- Define and maintain code to enable cloud data services communication.

###Cross-cutting Processes
- Define client and server build targets for supported platforms, cross-cutting issue with `#sig-core` and `#sig-build`.
- Define communication protocol and stacks.
- Define and maintain entity replication network model.
- Responsible for abstraction of console design for dedicated / headless server implementation (core / init).
- Define and maintain implementation of communication and data transfer between network based tools.
- Responsible for design and implementation of controller component until matured and moved into simulation SIG.
- Publish multiplayer component standard architecture to be shared with SIGs for remote simulation (physics/animation)
- Define and maintain network layer encryption, as a cross-cutting issue with `#sig-security`.
- Define and maintain network testing, as a cross-cutting issue with `#sig-testing`.
- Items that span or require other SIGs or groups and how it relates to this SIG’s responsibilities.

## Out of Scope
- Not responsible for sample projects unless projects are approved/accepted by SIG.
- Not responsible for cloud or network services or interopability with such services.
- Not responsible for 3rd party gems and code for network services. Contributors submitting such integrations are responsible for support.
- Not responsible for best practices of network or services implementation.
- Not responsible for networked tools implementation, outside of core responsibilities for multiplayer and cloud services, but may advise teams on best practices.
- Versioning of communication protocols (not currently a responsibility of the SIG).
- Support for peer-to-peer connectivity (not currently a responsibility of the SIG).
- Definition and implementation of 3rd party distribution platform services interfaces.


## SIG Links and lists:
- Slack/Discord: See https://o3de.org/community/ for Discord link, join the conversation in `#sig-network`.
- Mailing list: https://lists.o3de.org/g/sig-network
- Issues/PRs: 
     - Issues: https://github.com/o3de/o3de/issues?q=is%3Aissue+is%3Aopen+label%3Asig%2Fnetwork
     - PRs: https://github.com/o3de/o3de/pulls?q=is%3Apr+is%3Aopen+label%3Asig%2Fnetwork
- Meeting agenda & Notes: 
     - Agendas: https://github.com/o3de/sig-network/labels/mtg-agenda
     - Meeting Notes: https://github.com/o3de/sig-network/tree/main/meetings 

## Roles and Organization Management

SIG-network adheres to the standards for roles and organization management as specified by <sig-governance>. This SIG opts in to updates and modifications to <sig-governance>

## Individual Contributors

Additional information not found in the sig-governance related to contributors.

## Maintainers

Additional information not found in the sig-governance related to contributors.

## Additional responsibilities of Chairs

Additional information not found in the sig-governance related to SIG Chairs.

## Subproject Creation

Additional information not found in the sig-governance related to subproject creation.

## Deviations from sig-governance

Explicit Deviations from the sig-governance.
