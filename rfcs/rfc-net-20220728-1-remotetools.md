# rfc-net-20220728-1
* https://github.com/o3de/sig-network/issues/68

### Summary:
The Remote Tools Gem provides functionality for O3DE applications to connect to each other for debug and tooling purposes.

The Remote Tools Gem is the proposed successor to the TargetManagement system in AzFramework which is currently used to connect O3DE Applications to the Lua IDE. This Gem can serve as the connection layer between any O3DE applications that supports Gem loading. Existing potential use cases could include ScriptCanvas debugging and AssetProcessor. It would also become the connection solution for Lua IDE debugging as part of replacing TargetManagement.

### What is the relevance of this feature?
The Remote Tools Gem enables connecting to remote applications for remote debugging and tooling. As an example, the Lua IDE could use the gem to act as a host while a game build could use the gem to connect to the host as a client in order to debug lua. The gem also enables multiple host support which allows for an instance of an application to connect to a range of hosts. A practical example of this would be the Editor connecting to the Lua IDE for debugging in addition to ScriptCanvas debugging.

The gem allows users to more easily author transport dependent debug and tooling features for O3DE applications communicating through the use of a defined communication protocol and AzNetworking's security. Specifically, it will leverage AzNetworking to provide secure authenticated communication over localhost and TLS encrypted connections which provides secure connection handshake and encrypted data transport. This will allow O3DE developers to rapidly standup debug utilities and other non-release mode utilities requiring a socket based connection for communication.

Furthermore, the replacement of TargetManagement with the Remote Tools Gem will untangle several component dependency issues caused by TargetManagement.

Since the Remote Tools Gem is intended for debug and tooling purposes, it will be compiled out for RELEASE builds meaning it will not provide any change to consumers of O3DE based products.

### Feature design description:
The new Remote Tools Gem will contain the equivalent of AzFramework's TargetManagementComponent.

TargetManagement provides the ability for O3DE applications to connect with each other for debug purposes. In it, an application can declare itself a host while client applications will poll on TCP looking for a host. Once connected, the host can choose which target to communicate with via UI.

The Remote Tools Gem aims to address the following issues in TargetManagement:

* TargetManagement only supports one host which is currently expected to be the Lua IDE
* TargetManagement only supports one active connection vs being able to broadcast to multiple clients
* TargetManagement forks two threads on top of AzNetworking for a single connection
* TargetManagement has a tangled component registration pattern that would be greatly simplified by gemification
* TargetManagement primarily uses EBUS to communicate

The Remote Tools Gem will, via AZ::Interface, provide the ability to register and poll for multiple hosts. AZ::Events will be added to replace EBUS usage for forwarding packets to handlers. Two threads will be used by Remote Tools. One to poll for hosts and will cease operation if all hosts are connected to and one for processing outbox messages for all connections.

Connections between clients and hosts will be restricted to localhost.

Once gemification is complete, TargetManagement will be red coded which will also correct related component registration issues. All usages of TargetManagement will be migrated to Remote Tools.

### Technical design description:
The new Remote Tools Gem will communicate exclusively on TCP via AzNetworking. Currently, TargetManagement communicates via two types of packets. One that performs a connection handshake and one that sends a buffer of data. The buffer packets are sent to handlers via EBUS who can then deserialize and handle the buffer content as desired. This pattern offers flexibility and allows users to implement their own handling similar to Lua IDE and will be migrated to a combination of AZ::Interface and AZ::Events from EBUS.

Remote Tools will update this model to allow listeners to register a handler to a prescribed IP/Port combo. This will facilitate supporting a client connecting to multiple hosts. A practical example of this would be the Editor application being connected to the Lua IDE and Asset Processor. The goal will be for this API to be implemented via an AZ::Interface named IRemoteTools. Presently, TargetManagement starts polling for a host by default at startup. With multiple hosts, the user would have to opt in to this behavior via IRemoteTools. IRemoteTools would be implemented in AzFramework with an implementation registered in the Remote Tools Gem.

Once traffic is received, it will be routed via AZ::Event to the handler registered for the prescribed IP/port. For example, a hosting Lua IDE will register its handler against a pre-defined listen port such that all traffic on related connections would be routed to said handler.

Connections for Remote Tools are secured via DTLS as implemented by OpenSSL including passworded private keys on the server side. Connections in TargetManagement are hard-wired to localhost only. This will be removed to allow for remote debugging, leveraging passworded DTLS to enforce security and a restriction to localhost to reduce target surface area by allowing users to restrict connections to their specified surface area.

TargetManagement's threading model also needs to be revised. Currently, there are two extra threads in use. One polls for a host at a low interval by attempting to connect. Connection failure is a slow event that should be investigated for speedup (potentially by authoring a query as an alternative). The other processes outbox messages for an already established connection. Two threads will continue to be used however they will be updated to handle multiple client/host connections vs just one. Both threads will be disabled if there is no outstanding work.

Once the Remote Tools Gem is authored, the TargetManagement Component can be removed and will no longer need to be registered by a patchwork mix of core libraries and AzNetworking.

### How will this be implemented or integrated into the O3DE environment?
- TargetManagement will be migrated to a Gem

- Component registration for TargetManagement will be migrated to said Gem

### Are there any alternatives to this feature? 
We could potentially implement this an AzNetworking.Tools however it is important for Remote Tools to function for Launcher applications.

### How will users learn this feature?
Documentation will be provided on the Remote Tools API. In addition, the Lua IDE will make use of this Gem and will consequently be an in engine example of usage.

### Are there any follow up features?
There is some potential for follow up features here including:

Converting AssetProcessor from its AzSocket based stack to this Gem
Updating Remote Tools to use raw TCP Socket buffer based transport over AzNetworking's packet layer
This would more easily enable usage of the Gem for features like Remote Console without affecting other usages of the Gem
Gemification of AzNetworking
Adding a broadcast mode to Remote Tools such that a host can broadcast data to all connected endpoints vs only a selected target.
Update Remote Tools Gem to connect over a range of IPs beyond localhost as permitted by a CIDR filter.

### Are there any open questions?
N/A
