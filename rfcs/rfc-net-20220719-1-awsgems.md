# rfc-net-20220719-1
* https://github.com/o3de/sig-network/issues/64

## Summary:
Currently, there are 6 Amazon-specific gems within the main [o3de/o3de](https://github.com/o3de/o3de) repo: AWSCore, AWSClientAuth, AWSMetrics, AWSGameLift, the Twitch gem, and HttpRequestor (which is essentially a wrapper around AWSNativeSDK types and methods for making REST calls). These gems are unique in that they are optional and vendor-specific, yet live within the main engine source code. This RFC proposes extracting these gems to their own, Amazon-owned GitHub repositories and removing (over time) the versions of these gems that live within the engine source. This will improve the vendor neutrality of the engine itself, while allowing for development of these gems to continue in a leaner, more decoupled way from that of the entire engine.

## What is the relevance of this feature?
This work will change where 6 gems within o3de/o3de are housed, how they’re pulled into O3DE projects, and (potentially) how they’re advertised to the community. The biggest impact, from a user perspective, will be that once this work is completed, users will need to register the individual Amazon gem repositories with the project manager before being able to see and select them for inclusion in a project (unless other mechanisms are identified - see “open questions”, below). Users of the existing gems would need to migrate to use the external gems in order to continue pulling in the latest fixes & features.

## Feature design description:

At a high level, this work would entail making the following changes, in order:

1. Publish the 6 Amazon gems in public, Amazon-maintained repositories. It’s likely they won’t all be published at the same time, but AWSCore will need to be extracted first so that subsequent external gems can depend on it. These external Amazon gems should be functionally equivalent to the existing “included” gems and also maintained as permissively licensed open source projects.
2. Communicate to the community that the external versions of the Amazon gems are now available, along with instructions for migration and links to their docs (if docs are moved. See “Open Questions”, below). This communication should include, at a minimum, announcements in the relevant SIGs and on Discord, published notices on the [o3de.org](http://o3de.org/) docs pages for the effected gems, and comments in the engine source Amazon Gems which indicate they are being deprecated in favor of their external counterparts. It may also include, pending agreement of SIG-Release, release notes in the final (and/or penultimate) versions of engine which include the Amazon gems that they will be removed in a future release.
3. After a pre-determined amount of time, automated review (AR) tests run on the Amazon gems within the engine will be switched to non-blocking, or removed entirely. New issues cut against the Amazon Gems will be routed to the external repositories (exact mechanisms for this are TBD, will need to established in further conversations with SIG-Network). 
4. After a pre-determined amount of time (at least one release cycle), the Amazon Gems will be red coded from https://github.com/o3de/o3de. 
5. As development of the extracted Amazon gems continues, new versions of the AWSNativeSDK will be stored in, and pulled from, a separate distribution point (vs. the central 3rd party system), since they will not be a dependency of anything in the engine. Versions of the AWSNativeSDK already in use by released versions of the engine (which include old versions of the Amazon gems) will remain in the central system for backward compatibility.

## Technical design description:
The new “external” Amazon gems should be functionally equivalent to the existing source versions of the gems, but will need to differ slightly in terms of build setup and AR due to their new locations. Those differences may include:

* slightly different `gem_name` values and CMAKE target names, so that they don’t conflict with the source versions of the gems during the initial period of time when they may both exist in the same project.
* Pulling the AWSNativeSDK from a separate distribution point which will be appended to the `LY_PACKAGE_SERVER_URLS` variable.
* Be compatible with the head of o3de/o3de development at a delay. Testing and development against the last official release of the engine will take priority over integration with bleeding edge changes.

Adding the external versions of the gems to an O3DE project will be done as it is now for external gems. See the relevant documentation pages:

* https://docs.o3de.org/docs/user-guide/project-config/project-manager/#configure-gems
* https://docs.o3de.org/docs/user-guide/project-config/register-gems/

## What are the advantages of the feature?

Advantages for O3DE:
* Leaner O3DE engine source code, without gems that exist for primarily commercial benefit.
* Less O3DF engineering and compute resources spent building and testing gems as part of AR on each pull request.

Advantages for Amazon Gem users:
* Faster, more decoupled development will allow fixes and changes to be incorporated into these gems more quickly.

## What are the disadvantages of the feature?
* Developers who wish to use the Amazon gems in projects need to complete an extra step to configure them (i.e., register the external gem URL(s) with the project manager).
* Reduced discoverability of the Amazon Gems from within the main O3DE code repo and docs.
* O3DE will lack “out of the box” cloud service integrations.

## Are there any alternatives to this feature?
An alternative would be to leave the Amazon gems in the main o3de/o3de repo and continue to maintain them there. However, this may set a confusing precedent for adding similar optional vendor gems in the main engine source, which over time would encumber maintenance and testing of the core engine. For example: asking SIG-Network maintainers to incur costs on personal AWS accounts, or other commercial platforms, to fix bugs in the main engine repo is not fair or sustainable. 

## How will users learn this feature?
Advanced notice of the Amazon gems’ removal from the main engine repo and comprehensive migration instructions will be available and circulated at least one release cycle prior to its execution. As mostly enumerated above in “Feature design”, we plan make the community aware of this change by:

* Making advanced announcements in relevant SIG meetings and on Discord.
* Adding notices on the [o3de.org](http://o3de.org/) docs pages for the effected gems.
* Adding code comments in the o3de/o3de source versions of the Amazon gems which indicate they will be deprecated in favor of their external counterparts, with links to the new  repositories.
* Including release notes, in the final (and/or penultimate) versions of engine which include the Amazon gems, that they will be removed in a future release.
* Blog post(s) and tutorials on how to migrate to the external gems.

## Are there any open questions?
* Once these gems are removed from https://github.com/o3de/o3de/tree/development/Gems, whether or not their docs will continue to live at [o3de.org](http://o3de.org/) needs to be decided. We plan to work with [SIG-docs-community](https://github.com/o3de/sig-docs-community) to determine where they should go.
* An explicit definition of which sorts of gems should, and should not, be kept in the main o3de/o3de repo has yet to be agreed upon. Although we are taking a stance with this RFC that *Amazon gems* should be kept and maintained outside the engine itself, proposing formal criteria for inclusion that might apply to all gems is beyond the scope of this RFC.
* How and where we, as a community, promote discovery of gems external to https://github.com/o3de/o3de/tree/development/Gems. This is likely a topic to discuss with the [marketing committee](https://github.com/o3de/community/tree/main/committee/committee-marketing) and SIG-Content. 
* Whether there’s value in building, and keeping in the main engine repository, an alternative to the [HttpRequestor](https://github.com/o3de/o3de/tree/development/Gems/HttpRequestor) gem. Generic REST call functionality may useful and simple enough to reasonably include with the engine, however it shouldn’t rely on vendor-specific SDKs as [HttpRequestor](https://github.com/o3de/o3de/tree/development/Gems/HttpRequestor) does now.
