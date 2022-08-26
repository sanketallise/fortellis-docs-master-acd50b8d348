---
title: Creating Mock APIs in Swagger
author: Nathaniel Sandberg
last updated: 6/10/2021
---

To speed up your development lifecycle, you can do the following:

* Go to the [Swagger Editor](https://editor.swagger.io).  
* Make a mock API that you can use for testing.  

You can create the mock API in the language of your choice to facilitate your development lifecycle.

## Creating the API in Swagger

1. Follow the instructions in [Creating Specs](/docs/tutorials/spec-writing/creating-api-specs) to prepare your spec file.
1. Once you have your spec, go to the [Swagger Editor](https://editor.swagger.io).
1. Click **Generate Server** in the UI.
1. Click the language of your choice.
    You can choose from the following programming languages:
    * Java
    * Node
    * .NET
    * Go
    * Haskell
    * Kotlin
    * Scala
    * PHP
    * Python
    * Rails
    * Restbed
    * Rust
    * Sinatra
1. Unzip the folder.

## Running the Mock Server in NPM

**Note:** For this tutorial, we use NPM to run the mock server, but you can use the language that's compatible with your environment.

1. Open Git bash in the server folder you got from the spec editor.
1. Start the `npm` server.  
    Type `npm run start`.  
    **Note:** Remember the following:
    * The mock service automatically downloads npm when you start it.
    * You may have to wait for a moment for the dependencies to completely download.

You can now see your APIs working at `localhost:8080`.

```json
{
    "items": [
        {
        "activityId": "ae9ae7e1-bfbc-4ae3-9e10-480dbc3decf8",
        "activityType": "Send Email",
        "activityDate": "2019-04-17T14:30:00Z",
        "leadId": "75abcf7b-581c-4e27-b7b1-d2fb19693b16",
        "leadReceivedDate": "2019-04-10T14:30:00Z",
        "primarySalesperson": {
            "firstName": "John",
            "lastName": "Doe"
        },
        "links": {
            "self": {
            "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items",
            "rel": "collection",
            "method": "GET",
            "title": "Lead Disposition"
            }
        }
        },
        {
        "activityId": "41053699-2761-47c9-b1a2-3475eb4f772d",
        "activityType": "Phone Call",
        "activityDate": "2019-04-17T14:30:00Z",
        "leadId": "75abcf7b-581c-4e27-b7b1-d2fb19693b16",
        "leadReceivedDate": "2019-04-10T14:30:00Z",
        "primarySalesperson": {
            "firstName": "John",
            "lastName": "Doe"
        },
        "links": {
            "self": {
            "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items",
            "rel": "collection",
            "method": "GET",
            "title": "Lead Disposition"
            }
        }
        }
    ],
    "totalItems": 20,
    "totalPages": 10,
    "pageNumber": 1,
    "pageSize": 2,
    "links": {
        "self": {
        "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items?page=1&pageSize=2",
        "rel": "collection",
        "method": "GET"
        },
        "first": {
        "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items?page=1&pageSize=10",
        "rel": "collection",
        "method": "GET"
        },
        "last": {
        "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items?page=2&pageSize=10",
        "rel": "collection",
        "method": "GET"
        },
        "next": {
        "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items?page=2&pageSize=2",
        "rel": "collection",
        "method": "GET"
        },
        "prev": {
        "href": "$[apiFortellisUrl]/leadDisposition/v0/leads/75abcf7b-581c-4e27-b7b1-d2fb19693b16/items?page=1&pageSize=2",
        "rel": "collection",
        "method": "GET"
        }
    }
}
```
