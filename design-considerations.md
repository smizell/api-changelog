# Design Considerations

This document explains some of the design considerations behind this extension along with some questions and concerns around it. The [README](./README.md) is meant to be a short introduction to the idea with a short specification for this extension. This document allows us to go deeper into the discussion to understand why it exists and why some of the features were chosen.

## Considerations

### Change as part of the API description

The foundational idea here is that change should be part of the API description. We should evolve our APIs over time, and that evolution should be part of how we describe our APIs. This is something we already do in part. We have discussions about versioning, breaking changes, and deprecation. OpenAPI offers some support, but not much, and that's fine. The point of this extension is to augment what OpenAPI already does. It's not a proposal to change the OpenAPI specification but rather extend upon it.

I have to get a tad philosophical for a moment before moving forward. I'm defining "API description" in an abstract way here. An API description can be used as a contract, for validation, for mock servers, for testing, for governance, and so on, but it isn't primarily any of those things. When I talk the OpenAPI specification, I'm not (in theory) talking about an API description, but as a format used to capture an API description. This means that while I believe change and evolution are be part of the API description, it doesn't mean they have to be part of the OpenAPI specification. We can use extensions or other formats for that, all of which combine to form a complete API description.

I also think of change abstractly the same way I think of an API description. We capture change somewhere if not in an extension like this. We put it in JIRA tickets, or pull requests, or Git commits. We have some review and approval process we go through. We create API roadmaps and define API lifecycles for our teams. So we already think about change. We should consider how we could capture that and use it for tools and workflows.

Because of this, I've used an extension as a place to anchor the discussion. It doesn't mean this _must_ be an extension. But instead of creating a separate format for this, I'm using an extension because people are already familiar with OpenAPI. People already add things to their OpenAPI documents for tools like API gateways. An extension then feels like a good place to start the conversation. It doesn't mean this couldn't be a separate format. It's just a way to make change part of the everyday design process for an API.

### Standardizing change to help guide people

The API community does a good job of instructing people on how to write an API description from the beginning, where we start from nothing and create that original design. But information for going beyond that and evolving an API is not as straightforward or readily available. To make the jump to evolving an API, a person has to go beyond OpenAPI and dig deeper. They need to read articles about versioning and look through deprecation and sunset RFCs. They need to buy books. The need to learn about breaking changes. They need to consider how they might communicate changes with their customers. This takes time, and is learned through experience. We can't expect newcomers to know these things without spending a considerable amount of time in the API design world.

This extension exists to make this evolution process more concrete. It's for finding ways to capture change so people see it, talk about it, and consider how it might be communicated. Even if people don't use this extension, it can provide a workflow for people to think through when discussing change management. My hope is that at least this extension can help drive a broader discussion.

While the foundational idea of this extension is to make change a key part of the design process, the secondary goal is to make it more approachable to newcomers. It's for providing guidance. It's for giving teams and companies a way to bring change into the discussion. If this extension isn't the way to do it, we should continue pushing to find other ways that don't lock people into tools or specific workflows.

### The types of change

This extension captures the types of change that happens throughout the evolution of an API. There is the "initial change" that happens when a new feature or functionality is introduced into the design. This can be at the genesis of an API or it can be later on. Next, there is a "modification change" that is for changing something that exists. These changes can be additive or breaking—where the latter should be avoided if possible. Lastly, there is a "deprecating change" where some feature is marked as deprecated for removal for a period of time. 

Capturing these types of change can provide valuable information to tooling. For instance, if a deprecation is proposed an accepted, governance could check the information to make sure the deprecation is long enough. Maybe a company wants to say if something is set to "deprecated," it must be deprecated for at least three months. This extension does not set these rules. That is left up to the providers.

### The status of change

While we can capture the types of change, we can also define the state of a given change. We can state that we've proposed to deprecate some functionality with a planned date and removal/sunset date. We can change the state to "accepted" so that everyone knows it's planned but maybe not implemented in the API. Developers can then add headers, link relations, documentation, etc. to implement the change. Finally, the change can be deployed to production and the world can be notified.

This extension allows for capturing this entire workflow, though it doesn't prescribe a specific workflow. This functionality is there to show what could be captured in a standard way for tooling and sharing with others. This extension tries to stay close to the heart of the deprecation and sunset RFCs so the information can be used as part of implementations for those RFCs. This isn't meant to replace those but augment those.

A benefit of this is that designers and developers can work independently. A designer can propose a change to an API and it can be accepted without being implemented, and this information can exist alongside the rest of the API description. A designer could open a PR, have discussion, get approval, and merge the changes without adding code. Later, a developer can implement the changes in a separate PR. This is currently not straightforward. Maybe this extension could help make it easier.

An OpenAPI document alone tells you nothing about what is deployed or planned. This extension is meant for sprinkling in that information when applicable.

### Capturing activity around change

This extension also allows people to capture activity around the changes. When a feature is first proposed, it has a status of "proposed." It can later move to "accepted" when the feature has been agreed upon by the stakeholders, and this change in state can be captured as activity. Activity isn't required, and for some it may go too far. But it's there as a way to understand how the change changed over time.

This information can be used to specify rules around how change happens. It can show when features get stale. It can give an idea of when something was implemented and by who. This information can be used to give a glimpse of how the API changes over time.

## Questions and Concerns

I want to address questions and concerns that arise along the way.

### "This shouldn't be part of the contract."

The point of this extension is to capture information about how an API description changes over time. It isn't intended to be part of the contract. However, it is meant to live alongside other API information as part of the API description. Tools can remove information around this changelog to ensure this information isn't part of a defined contract with consumers.

Tools can also remove features that are planned though not implemented. This could be a valuable feature. There could be a single source of truth that contains a lot of releveant information around an API, and tools could generate OpenAPI documents or documentation depending on the context. For public, the tooling may strip out all of the planned features. For internal, it may include everything. This is left up to tooling designers.

### "How does this relate to the deprecation and sunset RFCs?"

The [Deprecation HTTP header field](https://tools.ietf.org/html/rfc8594) and [Sunset HTTP header field](https://tools.ietf.org/html/rfc8594) define how an API might communicate deprecation and sunset information at runtime. This OpenAPI extension is a way to define this same information, but it's meant to be captured at design time. It can later be used at runtime with these headers.

The hope would be that people could build tools that took information from the OpenAPI extension and add it into headers and link relations where applicable. So this extension is meant to work with RFCs like this, not to compete against them.
