# rfc-net-20211207-1
* Intake issue: https://github.com/o3de/sig-network/issues/23

### Summary:
In multiplayer networking solutions, one of the hardest developer tasks is debugging state desynchronization. Practically speaking, state can desync for any number of reasons. A missed variable set, a missed function call, a missing cvar on an endpoint, etc. In a multiplayer environment, detailed and performant diagnostic logging to help identify the source of these desyncs and provide detail to their nature will be incredibly useful.

### What is the relevance of this feature?
**Challenge**: Desynchronization of networked values

In the Multiplayer Gem model, certain networked values are autonomous. The values can be modified locally despite not being the authority. When the logic between endpoints are not synchronized, the state of a networked value can diverge requiring the authority to send a correction. 

The root cause of a desynchronization of a networked value can occur for any number of reasons and at unpredictable frequency. The ability to detect the source of a desync without transmitting additional data between endpoints to do so will enable surfacing of information related to desyncs even in production optimized builds.

**Goal**: Configurable logging to assist in identifying and debugging network desynchronization

### Feature and Technical design description:
#### Terminology

- Desync - A divergence of simulation state between two endpoints in a networked simulation
- Authority - The source of truth for a piece of data in a networked simulation
- Autonomous - A non-authoritative piece of data that can be modified locally but is not the authority. Used to run predictive simulations.
- Proxy - A non-authoritative piece of data that is strictly read only and whose values are determined by the authority
- Endpoint - A participant in a networked simulation which can be connected to anywhere from one to many other endpoints
The feature set for logging desyncs of networked values will be separated into multiple verbosity levels. As verbosity increases, a tradeoff is made of performance for information. The higher the verbosity, the greater the performance cost incurred for access to more information. 

#### Current State

Currently, the Multiplayer Gem has logic to detect a desync for Autonomous proxies in non-release builds. On detection, the detecting endpoint serializes its state history for the desynced value and sends to its opposite endpoint where state can be compared. The block is notably from the function HandleSendClientInputCorrection which means that in this function and in SendClientInputCorrection on the server, the occurrence of the desync can be logged along with the frame ID it occurred at and the current and expected values.

This approach requires serializing state between two endpoints to compare state as perceived by each. Due to its cost, it is not used on Release builds. Additionally, this approach only shows changes over time for the desynced value and provides no context on what actions/occurrences led to the desync.

```
#ifndef AZ_RELEASE_BUILD
        if (cl_EnableDesyncDebugging)
        {
            AZLOG_INFO("** Autonomous Desync - Corrected clientInputId=%d ", aznumeric_cast<int32_t>(inputId));
            auto iter = m_predictiveStateHistory.find(inputId);
            if (iter != m_predictiveStateHistory.end())
            {
                // Read out state values
                AzNetworking::StringifySerializer serverValues;
                GetNetBindComponent()->SerializeEntityCorrection(serverValues);
                PrintCorrectionDifferences(*iter->second, serverValues);
            }
            else
            {
                AZLOG_INFO("Received correction that is too old to diff, increase cl_PredictiveStateHistorySize");
            }
        }
#endif
```
#### Verbosity Level 1 - High Performance

At the minimum level, an O3DE developer would want to be able to compare perceived networked state for the client and server. By utilizing the fact that the simulation is ultimately deterministic and already has awareness of when a desync occurs in order to issue a correction, logging can be provided detailing the step that resulted in a desync. This approach would require no additional data to be sent between end points. While this would provide no context on what actions led to the desync, it would be performant enough to run in Release builds.

#### Verbosity Level 2 - Audit Trail

At the next level, additional data will be sent when a desync is detected in order to provide more detail on the nature of a desync. Networked values that desync are generally the result of actions taken on each endpoint resulting in different outcomes. In order to debug these occurrences with greater detail, information of every network related action recorded that remains in network history can be attached to the desynced endpoint. Both can then be outputted to determine if any divergence exists between the contents and order of each.

Furthermore, desynced values that are predictable feature rewindable history. This data can also be appended with timing information such that the value can shown at each step of the audit trail. 

Once again let us look at LocalPredictionPlayerInputComponent:

```
#ifndef AZ_RELEASE_BUILD
            if (cl_EnableDesyncDebugging)
            {
                StateHistoryItem inputHistory = AZStd::make_unique<AzNetworking::StringifySerializer>();
                while (m_predictiveStateHistory.size() > cl_PredictiveStateHistorySize)
                {
                    m_predictiveStateHistory.erase(m_predictiveStateHistory.begin());
                }
                GetNetBindComponent()->SerializeEntityCorrection(*inputHistory);
                m_predictiveStateHistory.emplace(m_clientInputId, AZStd::move(inputHistory));
            }
#endif
```
Here predictive state of an autonomous proxy is stored so that it can be used in the comparison detailed in the code block discussed in Current State. A similar approach can be utilized in which emplace the input ID, frame ID and data value for a networked property. On desync a client can display the history for a given property as a log ordering the values in history chronologically. This is similar used to the approach in the function PrintCorrectionDifferences in LocalPredictionPlayerInputComponent. Alternatively, history can be aggregated for all properties so changed can be displayed across multiple values sorted by frame ID and input ID to form a broader audit trail of changes to network properties. This sorted data could then be logged out in sort order. Long term this data could be made available so that visual tools (such as an imgui integration) could provide a more interactive UX.

#### Verbosity Level 3 - Audit Trail + User Supplied Logging

At the highest level, users can additionally add logging to the audit trail via macro. This will involve serializing user specified logs between end points but offers users the ability to add custom logging. This will leverage the same structures utilized to implement Verbosity Level 2. 

### What are the advantages of the feature?
This feature provides various levels of logging to assist users in identifying and debugging state desynchronization. 

### What are the disadvantages of the feature?
While the lowest verbosity level of this feature is intended to be performant enough for production use, all higher levels of verbosity involve transport of additional data between endpoints to facilitate debugging. This extra cost should be kept in mind by users.

### How will this be implemented or integrated into the O3DE environment?
This feature will be a part of the Multiplayer Gem which can be fully disabled or run at various verbosity levels via cvar.

For example,

cl_DesyncLogginVerbosity

0 = Fully disabled
1 = High performance mode
2 = Audit Trail logging
3 = Audit Trail + User Supplied logging

### Are there any alternatives to this feature?
This feature provides various levels of operation for users for debugging desynchronization between two endpoints. Alternatives to this approach include traditional style logging in each endpoint separately and then comparing that output. This could be facilitated by forwarding logs to an aggregation service. The user would be responsible for adding requisite information to differentiate endpoints and sessions. This would mean that compared to the proposed solution, the user would have to identify which endpoints a desync occurred between and then manually inspect logs to root cause the source of the desync.

### How will users learn this feature?
This feature will have public facing documentation provided detailing each verbosity level and how to interface with them. Surfacing will occur via log file and visual debugging tools such as the Multiplayer ImGui Gem. 

### Are there any open questions?
N/A
