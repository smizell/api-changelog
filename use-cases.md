# Use Cases

This document explores some of the use cases as to why this API Changelog Extension might be useful to API providers and consumers. This extension was created from real-world experiences and stories heard from people as described below. While there are many others uses for this extension, these are the main reasons behind the extension.

## Keeping People in the Loop

An OpenAPI document describes a desired state for an API, but it doesn't tell what's been deployed nor what is planned. It doesn't communicate how the API is changing. This information is stashed away in Pull Requests and user stories. But what if developers—even developers across other teams—could look at a single point of truth to see where an API is going? What if the developers building the API knew they had to plan for upcoming changes before they make it into the OpenAPI document? The extension exists as a way to keep people on the team in the loop.

> When an API designer wants to communicate change, they can use the API Changelog Extension, so the upcoming plans are viewable alongside the existing implementation.

This works the same for API consumers. API providers can keep the consumers in the loop on changes that have been accepted by the team.

> When an API provider is planning some changes, they can add those changes using the API Changelog extension, so API consumers are aware and can plan for changes on their end accordingly.

## Defining Business Rules Around Change

Many style guide tools for APIs define how an API can be, but they don't define or give feedback on how an API can change. What happens if someone introduces a breaking change? How long should that be communicated to consumers before it lands in production? How long can something be deprecated before it's removed? These rules can be captured and added to API style guides to better express how APIs are allowed to evolve in an organization.

> When an API designer is changing an API using the API Changelog Extension, they can be notified of rules about those changes, so that API across organizations and teams change with the same standards and guidelines.

## Having a Central Place for Deprecation Information

OpenAPI allows for deprecating items, but it doesn't provide a way to capture deprecation information like why or when it will be removed. This information is useful to planning. It's also useful for API consumers to know as early as possible. Once a deprecation is accepted, consumers can be immediately notified. Additionally, the deprecation information can be used in that API at runtime using the [Sunset Header](https://tools.ietf.org/id/draft-wilde-sunset-header-03.html). Lastly, documentation can be built from this information so that it is available to find through a browser.

> When API providers decide to deprecate an item, they can include this information in their API description using the API Changelog Extension, so that that information can be used in tooling and at runtime.
