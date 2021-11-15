# rfc-net-20211026-1
* Intake issue: https://github.com/o3de/sig-network/issues/9

### Summary
 
[FlexMatch](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-intro.html) is a Amazon GameLift matchmaking service which allows multiplayer game developers to customize and build multiplayer matchmaking experiences to best fit their games. It gives flexibility to customize key aspects of the matchmaking process, including fine-tuning the matching algorithm. This RFC covers integrating Amazon GameLift FlexMatch into the [AWS GameLift Gem](https://o3de.org/docs/user-guide/gems/reference/aws/aws-gamelift/).
 
### What is the relevance of this feature?
 
Integrate AWS GameLift Gem with FlexMatch feature to support multiplayer game matchmaking so that players can be easily grouped with other compatible players for each game match. 
 
#### Personas
 
O3DE multiplayer game developer who wants to build matchmaking experience to evaluate and select compatible player for each match of their game.
 
#### User Stories
 
* As an O3DE multiplayer game developer, I want O3DE to offer an integration with Amazon GameLift FlexMatch feature to build matchmaking experience.
* As an O3DE multiplayer game developer, I want to be able take advantage of backfill modes in FlexMatch to improve the speed of matchmaking for players as well providing mechanism to bring in other players when players fail to join.
* As an O3DE multiplayer game developer, I want to have a CDK example that shows how to model FlexMatch resources to help me manage and deploy matchmaking resources.
* As an O3DE multiplayer game developer, I want to have step by step documentation about how to use Amazon GameLift FlexMatch in O3DE, including a tutorial level.
 
#### In Scope
 
* Amazon GameLift FlexMatch matchmaking integration
* Game client local matchmaking ticket system to poll Amazon GameLift FlexMatch matchmaking ticket status
* Amazon GameLift FlexMatch automatic & manual backfill matchmaking integration
 
#### Out of Scope
 
* Game client service to track Amazon GameLift FlexMatch matchmaking ticket event notifications. See open question at end of RFC.
 
### Feature design description
 
This design proposes integrating FlexMatch matchmaking support into the existing AWS GameLift Gem such that developers of multiplayer games can easily build, experiment and deploy matchmaking features to group players and place them in game sessions. The O3DE AWS GameLift gem will support the FlexMatch workflows outlined [here](https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/gamelift-match.html) and in the workflow section.
 
FlexMatch also supports two backfilling games so games can start with fewer players to accelerate matchmaking and new players can be added to the game in cases such as players failing to join.
 
The AWS GameLift Gem will support two backfill modes: manual and automatic. In manual mode, the developer’s service code can make the decision if and when to backfill; no backfill will occur unless the server explicitly starts a backfill request. In automatic mode, the GameLift server SDK will automatically start backfill requests if the sessions starts without a full complement of players.  Note: backfill mode is managed in the GameLift matchmaking configuration.
 
#### Requirements & Goals
 
* The feature **MUST** get integrated with Amazon GameLift FlexMatch including matchmaking and backfill matchmaking process
* The feature **MUST** support matchmaking features on both client and server
* The feature **MUST** provide CDK application sample for deploying FlexMatch required resources
* The feature **MUST** function on all supported GameLift Windows and Linux instances (Amazon Linux 2) for dedicated server
* The feature **MUST** get deployed successfully through CDK application sample or manual instructions
* The feature **SHOULD** have configurable polling frequency for game client local matchmaking ticket system
* The feature **SHOULD** be flexible to replace game client local matchmaking ticket system with client event notification service
 
#### Terminology
 
* **Matchmaker** The customized process to build a game match, which includes matchmaking configuration and rule set. It manages the pool of matchmaking requests received, forms teams for a match, processes and selects players to find the best possible player groups, and initiates the process of placing and starting a game session for the match.
* **Matchmaking Configuration** Guidelines for use with FlexMatch to match players into games. All matchmaking requests must specify a matchmaking configuration.
* **Matchmaking Rule Set** Set of rule statements, used with FlexMatch, that determine how to build player matches. Each rule set describes a type of group to be created and defines the parameters for acceptable player matches. Rule sets are used in MatchmakingConfiguration objects.
* **Player Acceptance** If matchmaking configuration requires acceptance, all players must be given the option to accept or reject a proposed match. A match must receive acceptances from all players in the proposed match before it can be completed.
* **Match Backfill** Fill the empty player slots in an existing game session with well-matched new players. Customize when and how new players can be requested, and use the same custom match rules to find additional players. 
* **Backfill Mode** Backfill mode is managed in matchmaking configuration, it can be either AUTOMATIC or MANUAL. For manual backfill, it gives developer the flexibility to decide when to trigger a backfill request. For automatic backfill, Amazon GameLift service begins automatically generating backfill requests for it if game session starts with open player slots.
* **Client Local Ticket Tracker** A local ticker manager consistently describes matchmaking ticket for player. When an acceptable match is found, matchmaking ticket will get updated with required information for game session connection.
 
#### Workflow
 
##### Matchmaking
 
1. Request matchmaking for player against Amazon GameLift FlexMatch by calling `StartMatchmaking`.
 
    * Amazon GameLift service will generate matchmaking ticket and put in ticket pool. Ticket will remain in pool until it is matched or time limit is reached.
    * Amazon GameLift service will evaluate ticket based on ticket age and build match based on matchmaking configuration.
 
2. Track matchmaking ticket by using client local ticket tracker.
 
    * [Player Acceptance Enabled] Player needs to `AcceptMatch` to fulfill match ticket - if any player in proposed match rejects, ticket will go to FAILURE and won't be processed; for players accept, their ticket will go back to pool.
    * When acceptable match is found, Amazon GameLift service will create new game session automatically.
 
3. Once game session is created, local ticket tracker will get updated ticket with game session connection data, like ip address and created player session id for player.
4. Connect player to the match.
 
**Note:** Player can cancel match at any time by calling `StopMatchmaking`. If there is ongoing local ticket tracker process, it should be stopped as well.
 
##### Backfill matchmaking
 
The workflow is mostly the same with standard matchmaking workflow, except **step 2** as game session is existed when acceptable match is found:
 
* Amazon GameLift service will signal game session updated and game server needs to update existing game with latest match data in `OnUpdateSessionBegin`.
 
**Note:** For manual backfill, developer has responsibility to send backfill request on server side by calling `StartMatchBackfill` whenever a matched game has one or more open player slots, like a match game starts with open player slots, or player leaves an ongoing match game.
**Note:** Depending on the game style, developer might want to stop backfill after reaching specific conditions, like game has 5mins left, etc. Then server can stop backfill by calling `StopMatchBackfill`.
 
### Technical design description
 
* The feature requires an update to existing session interface as it introduces 3 new public client APIs: `AcceptMatch`, `StartMatchmaking` and `StopMatchmaking`.
* The feature requires an update to existing session notifications with new callback `OnUpdateSessionBegin` to support existing game session update.
* The feature requires to create new server interface, `StartMatchBackfill` and `StopMatchBackfill`, to support Amazon GameLift FlexMatch manual matchmaking backfill mode.
* The feature requires to create a new component, `AWSGameLiftClientLocalTicketTracker`, to poll matchmaking ticket periodically.
 
#### Client APIs
 
Update [`ISessionRequests.h`](https://github.com/o3de/o3de/blob/development/Code/Framework/AzFramework/AzFramework/Session/ISessionRequests.h) by adding client public API requests.
 
```
struct AcceptMatchRequest
{
    bool m_acceptMatch;
    AZStd::vector<AZStd::string> m_playerIds;
    AZStd::string m_ticketId;
};
 
struct StartMatchmakingRequest
{
    AZStd::string m_ticketId;
};
 
struct StopMatchmakingRequest
{
    AZStd::string m_ticketId;
};
```
 
Update [`ISessionRequests`](https://github.com/o3de/o3de/blob/development/Code/Framework/AzFramework/AzFramework/Session/ISessionRequests.h#L107) for `AcceptMatch`, `StartMatchmaking` and `StopMatchmaking`
 
```
virtual void AcceptMatch(const AcceptMatchRequest& request) = 0;
 
virtual AZStd::string StartMatchmaking(const StartMatchmakingRequest& request) = 0;
 
virtual void StopMatchmaking(const StopMatchmakingRequest& request) = 0;
```
 
Update [`ISessionAsyncRequests`](https://github.com/o3de/o3de/blob/development/Code/Framework/AzFramework/AzFramework/Session/ISessionRequests.h#L136) and [`SessionAsyncRequestNotifications`](https://github.com/o3de/o3de/blob/development/Code/Framework/AzFramework/AzFramework/Session/ISessionRequests.h#L162) for `AcceptMatch`, `StartMatchmaking` and `StopMatchmaking`
 
```
virtual void AcceptMatchAsync(const AcceptMatchRequest& request) = 0;
virtual void OnAcceptMatchAsyncComplete() = 0;
 
virtual void StartMatchmakingAsync(const StartMatchmakingRequest& request) = 0;
virtual void OnStartMatchmakingAsyncComplete(const AZStd::string& matchmakingTicketId) = 0;
 
virtual void StopMatchmakingAsync(const StopMatchmakingRequest& request) = 0;
virtual void OnStopMatchmakingAsyncComplete() = 0;
```
 
Add AWS GameLift Gem matchmaking requests `AWSGameLiftAcceptMatchRequest`, `AWSGameLiftStartMatchmakingRequest` and `AWSGameLiftStopMatchmakingRequest` under [`Gems/AWSGameLift/Code/AWSGameLiftClient/Include/Request`](https://github.com/o3de/o3de/tree/development/Gems/AWSGameLift/Code/AWSGameLiftClient/Include/Request) as Amazon GameLift service requires extra attributes for matchmaking request [6](https://docs.aws.amazon.com/gamelift/latest/apireference/API_StartMatchmaking.html)
 
#### Server Notifications
 
Update [`SessionNotifications`](https://github.com/o3de/o3de/blob/development/Code/Framework/AzFramework/AzFramework/Session/SessionNotifications.h) with `OnUpdateSessionBegin`
 
```
virtual void OnUpdateSessionBegin(const SessionConfig& sessionConfig, const AZStd::string& updateReason) = 0;
virtual void OnUpdateSessionEnd() = 0;
```
 
#### Server APIs
 
Update [`IAWSGameLiftServerRequests`](https://github.com/o3de/o3de/blob/development/Gems/AWSGameLift/Code/AWSGameLiftServer/Include/Request/IAWSGameLiftServerRequests.h) with backfill server APIs
 
```
class IAWSGameLiftServerRequests
{
public:
   virtual bool StartMatchBackfill(const AZStd::string& ticketId, const AZStd::vector<AWSGameLift::AWSGameLiftPlayer>& players) = 0;
   virtual bool StopMatchBackfill(const AZStd::string& ticketId) = 0;
};
```
 
#### Components
 
`AWSGameLiftClientLocalTicketTracker` Instead of having a backend client service to track FlexMatch event notification, AWS GameLift Gem will have a client local ticker manager consistently polls and describes matchmaking ticket.
 
* `m_ticketPollFrequency` should be at least 10 seconds to prevent AWS request throttle (frequency could be configurable through setting registry file)
* Start ticket polling only after `StartMatchmaking` succeeds; Stop ticket polling when ticket status is COMPLETED or `StopMatchmaking` is called
* There is only one ticket polling process at one time
* Developer has responsibility to replace it with more scalable solution for public release game, like publishing ticket event notifications to Amazon SNS topic.
 
`AWSGameLiftClientLocalTicketTracker` should be flexible to replace by implementing internal interface `IAWSGameLiftMatchmakingTicketRequests`
 
```
class IAWSGameLiftMatchmakingTicketRequests
{
public:
    virtual void StartPolling(const AZStd::string& ticketId, const AZStd::string& playerId) = 0;
    virtual void StopPolling() = 0;
};
```
 
### What are the advantages of the feature?
 
* The feature extends O3DE’s GameLift integration to support multiplayer matchmaking features that are used in most multiplayer games.
* The feature saves developer efforts to integrate multiplayer matchmaking features with O3DE networking components.
* The feature usage cost is included in fees for GameLift as hosting games on GameLift servers.
* The feature utilize AWS CDK tool kit to deploy and manage related FlexMatch resources.
 
### What are the disadvantages of the feature?
 
* The feature is designed for dedicated server hosted on Amazon GameLift only.
* The feature supports only one matchmaking ticket in flight per client at one time; developers may also need to implement extra code if manual backfill is required (such as player on early quit) but automatic backfill is configured.
* The feature is hard to automatically test as GameLiftLocal does not support FlexMatch testing. Tests must deploy the required AWS resources with the downside of test run time time and added costs.
* Amazon GameLift has limited free tier, requiring developers to be aware of potential costs they may incur.
 
### How will this be implemented or integrated into the O3DE environment?
 
* Most of code will be provided through AWS GameLift Gem, some code changes maybe required in AzFramework and AWS GameLift CDK application sample.
* Settings registry file and other config will be provided for configurable settings.
 
### Are there any alternatives to this feature?
 
Yes, customer are free to implement their own multiplayer matchmaking mechanism without using Amazon GameLift FlexMatch. Customers can also use GameLift without FlexMatch features.
 
### How will users learn this feature?
 
* The feature will provide detailed documentation for the feature, as per most O3DE gems.
* Ideally the feature should be introduced with a tutorial multiplayer level. (note: tutorial level won’t be delivered as part of this RFC).
 
### Are there any open questions?
 
**Q:** Should the feature provide client service solution to track matchmaking event notification (including CDK application sample to setup required resources)?

**Context:** As suggested by Amazon GameLift service, all games in public release should use notifications regardless of volume. The continuous polling approach is only appropriate for games in development with low matchmaking usage, because large volume `DescribeMatchmaking` calls in short period time can easily get your requests throttled which will delay client to get latest match status.

**Solution:** Setup event notifications through Amazon Cloudwatch events or Amazon SNS topics. See details in https://docs.aws.amazon.com/gamelift/latest/flexmatchguide/match-notification.html

**Cost:** The feature need to add Amazon Cloudwatch events or Amazon SNS topics setup in CDK application sample, and cost will depend on corresponding service usage price.