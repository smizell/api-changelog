# API Changelog - An OpenAPI Specification Extension for Evolving APIs

An OpenAPI document alone doesn't tell you what's been deployed. It doesn't say what's planned. It might say what's deprecated, but it doesn't say how long or when the functionality will be removed. We need something to help us here.

API Changelog is an [OpenAPI Specification Extension](https://github.com/OAI/OpenAPI-Specification/blob/f1852bd4bd89bf96ed929179f9f52cfa6621accb/versions/3.0.0.md#specification-extensions) for describing how and when your API will change. It's meant to capture plans and changes so you can generate changelogs, communicate future features to consumers, show what's been deployed to production, and improve the design overall process.

## Overview

The API Changelog extension focuses on three main areas of the OpenAPI document.

1. [Operation Object](https://github.com/OAI/OpenAPI-Specification/blob/f1852bd4bd89bf96ed929179f9f52cfa6621accb/versions/3.0.0.md#operationObject)
1. [Parameter Object](https://github.com/OAI/OpenAPI-Specification/blob/f1852bd4bd89bf96ed929179f9f52cfa6621accb/versions/3.0.0.md#parameterObject)
1. [Schema Object](https://github.com/OAI/OpenAPI-Specification/blob/f1852bd4bd89bf96ed929179f9f52cfa6621accb/versions/3.0.0.md#schemaObject)

These are the main areas where an API can evolve. There are three types of ways change can be communicated for these areas.

1. **Initial** - this is the initial design of the area in which the extension is found
1. **Modification** - this is a change for an area that has already been deployed
1. **Deprecation** - this is when an area is marked as deprecated
1. **Removal** - this is when an area has been removed

Every change that is introduced can go through several states. This allows designers and developers to be able to not only look at an OpenAPI document and know what has or hasn't been deployed, but it also allows tooling to hide areas that are in the design phase. Tools would be able to build different versions of the OpenAPI document depending on the context—for the public, only deployed features. For internal teams, everything.

The states for these types of changes are:

1. **proposed** - the change has been proposed to the stakeholders and is up for discussion
1. **accepted** - the change has been accepted by the person or people with that responsibility
1. **development** - the development team is working on implementing the change
1. **ready** - the change is ready to be deployed
1. **deployed** - the change has been deployed into production

All of these states are optional depending on the teams' workflows.

## Example

This is an example of the changelog used in an OpenAPI document. There are other examples shown throughout the specification below.

```yaml
{
   "openapi": "3.0.0",
   "info": {
      "version": "1.0.0",
      "title": "Simplified Petstore",
   },
   "servers": [
      { "url": "http://petstore.swagger.io/v1" }
   ],
   "paths": {
      "/pets": {
         "get": {
            "summary": "List all pets",
            "x-changelog": {
              "changes": [
                {
                  "type": "initial",
                  "status": "accepted",
                  "title": "List all pets",
                  "description": "This change will introduce a new operation to list pets",
                  "plannedDate": "2019-06-10",
                  "activity": [
                    {
                      "statusChange": "proposed",
                      "by": "smizell",
                      "date": "2019-06-01"
                    },
                    {
                      "statusChange": "accepted",
                      "by": "smizell",
                      "date": "2019-06-02"
                    },
                  ]
                }
              ]
            }
            "operationId": "listPets",
            "tags": [ "pets" ],
            "responses": {
               "200": {
                  "description": "A paged array of pets",
                  "content": {
                     "application/json": {
                        "schema": {
                           "$ref": "#/components/schemas/Pets"
                        }
                     }
                  }
               }
            }
         }
      }
   },
   "components": {
      "schemas": {
         "Pet": {
            "required": [
               "id",
               "name"
            ],
            "properties": {
               "id": {
                  "type": "integer",
                  "format": "int64"
               },
               "name": {
                  "type": "string"
               },
               "tag": {
                  "type": "string"
               }
            }
         },
         "Pets": {
            "type": "array",
            "items": {
               "$ref": "#/components/schemas/Pet"
            }
         }
      }
   }
}
```

## Specification

This specification uses [MSON](https://github.com/apiaryio/mson/blob/master/MSON%20Specification.md) as a way to describe the JSON for the specification extension.

### Field Name

The field name to use for the extension is `x-changelog`.

#### Example

This shows a changelog embedded in an operation. Some of the information has been omitted for the sake of brevity.

```js
{
  // ...
  "paths": {
    "/pets": {
      "get": {
        "operationId": "findPets",
        "x-changelog": { /* ... */ }
      }
    }
  }
}
```

### Extension Object

- `version`: 0.1 (string, required, fixed)
- `changes` (array[[Changelog Object](#changelog-object)])

#### Example

```js
{
  "version": "0.1",
  "changes": []
}
```

### Changelog Objects

The section defines the various changes an API can go through.

#### Initial Change

An initial change is the first time a feature is proposed in an API description document. It is the first changelog item that should occur.

- `type`: initial (string, fixed, required)
- `status`: proposed, accepted, development, ready, deployed (enum, required)
- `title` (string, optional)
- `description` (string, optional) - description for the change in Markdown
- `plannedDate` (string, optional) - date in which the change will be deployed
- `activity` (array[[Activity Object](#activity-object)])

##### Example

```js
{
  "type": "initial",
  "status": "accepted",
  "title": "Find customer operation",
  "description": "This change will introduce a new find customer operation",
  "plannedDate": "2019-06-10",
  "activity": [
    {
      "statusChange": "proposed",
      "by": "smizell",
      "date": "2019-06-01"
    },
    {
      "statusChange": "accepted",
      "by": "smizell",
      "date": "2019-06-02"
    },
  ]
}
```

#### Modification Change

Modification can occur in an API when an area of the design is changed. These modifications happen to existing features, so a modification MUST proceed an initial changelog for an area. 

- `type`: modification (string, fixed, required)
- `status`: proposed, accepted, development, ready, deployed (enum, required)
- `title` (string, optional)
- `description` (string, optional) - description for the change in Markdown
- `plannedDate` (string, optional) - date in which the change will be deployed
- `breakingChange` (boolean, optional) - specifies whether or not a change will be breaking
  - Default: false
- `activity` (array[[Activity Object](#activity-object)])

##### Example

```js
{
  "type": "modification",
  "status": "proposed",
  "title": "Add email field for customers",
  "description": "This change adds the email address field for customers",
  "plannedDate": "2019-06-10",
  "activity": [
    {
      "statusChange": "proposed",
      "by": "smizell",
      "date": "2019-06-01"
    }
  ]
}
```

#### Deprecation Change

Areas of the document can be marked according to the OpenAPI specification as deprecated. This field is usually a boolean field. Using the API Changelog extension, API providers can include information about when and why a deprecation is happening. It can also be used as a way to generate information that can be used at runtime like [Sunset HTTP Header](https://tools.ietf.org/id/draft-wilde-sunset-header-03.html).

- `type`: deprecation (string, fixed, required)
- `status`: proposed, accepted, development, ready, deployed (enum, required)
- `title` (string, optional)
- `description` (string, optional) - description for the change in Markdown
- `plannedDate` (string, optional) - date in which the change will be deployed
- `removalDate` (string, optional) - date in which the area will be removed
- `breaking_change` (boolean, optional) - specifies whether or not a change will be breaking
  - Default: false
- `activity` (array[[Activity Object](#activity-object)])

##### Example

```js
{
  "type": "deprecation",
  "status": "proposed",
  "title": "Remove email field for customers",
  "description": "This change removes the email address field for customers",
  "plannedDate": "2019-06-10",
  "removalDate": "2020-06-10",
  "activity": [
    {
      "statusChange": "proposed",
      "by": "smizell",
      "date": "2019-06-01"
    }
  ]
}
```

### Activity Object

The activity object is for capturing who changed a status and which it happened. This helps to create a trail of how an area has evolved over time.

- `statusChange`: proposed, accepted, development, ready, deployed (enum, required)
- `by` (string, optional) - an identifier for the person who made the status change
- `date` (string, required) - date in which the activity happened

#### Example

```js
{
  "statusChange": "proposed",
  "by": "smizell",
  "date": "2019-06-01"
}
```
