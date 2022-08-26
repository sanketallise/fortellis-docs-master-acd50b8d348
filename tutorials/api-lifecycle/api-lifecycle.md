---
title: API Development Lifecycle
author: Kelly Rich
last updated: 4/1/2022
---

In a nutshell, API Developers trace the following lifecycle when they implement new APIs:

1. Create an API specification that describes a new service.
1. Implement the service that's described by the API specification.
1. Test and finalize the service.
1. As needed, certify the API Spec and service and list it publicly.
1. Market the service in the API Directory.
1. Maintain the service.

Through the development process, API Developers use the Fortellis platform to:

* Create API specifications that adhere to a standardized format (OpenAPI), which is used throughout the industry.
* Create private APIs to protect proprietary information.
* Implement and test an API within a team whose membership they control.
* Share a private API across the Organization.
* Publish a public API to share outside the Organization.
* List the public APIs in the API Directory.

See the following resources for details for details about designing APIs:

* [API Spec Introduction](/docs/general/api-design/api-spec-introduction).
* [Creating API Specs](/docs/tutorials/spec-writing/creating-api-specs).

## Admin API

There's a couple of choices you must make upfront when you begin to implement an API, and one of those is how you will track and manage the connections to your API service.

You can implement the Admin API specification, which helps you:

* Choose and manage API activations.
* Accept connections.
* Retain information about active connections.
* Protect subscriber information.

For more, see the [Admin API Overview](/docs/tutorials/admin-api/admin-api-overview).

If you choose to implement an API without using the Admin API, you might need to do some of the following:

* Make repeated calls to Fortellis to get subscription context for API requests.
* Cache information to reuse at a later date to reduce the response time.  
    **Note:** API Developers can check user subscriptions at runtime, but this increases the latency of each call.

Each API call requires the Subscription ID of the App Buyer who makes the request. If the API Developer does not use the Admin API, they must get the Subscription ID of the App Buyer before making API requests.

For more, see [Validating a Subscription ID without an Admin API](/docs/tutorials/admin-api/no-admin-api).

## The API Lifecycle, Step by Step

API Developers often take the following path when creating Fortellis APIs:

1. Create an API Spec that describes the public functionality of the service you plan to implement.  
    For more, see [APIs and API Specs](/docs/general/api-developers/api-specs) and [Creating API Specs](/docs/tutorials/spec-writing/creating-api-specs).
1. Implement the service described by the corresponding API Spec.
1. API Developers register their API in Fortellis.  
    For more, see [Registering APIs](/docs/tutorials/api-lifecycle/registering-apis).
1. Implement the Admin API, if you choose this API user-management path.  
    API Developers should keep the following in mind if they choose to *not* implement the Admin API:  
    * If you do not automatically accept connection requests, you need to manually monitor and accept the requests
    * To make API requests, you must retrieve the Subscription ID in the context of the current user  
    For more, see [Admin API](/docs/tutorials/admin-api/admin-api-overview).
1. If desired, make the API public and list it in API Directory.  
    For more, see [Listing APIs](/docs/tutorials/api-lifecycle/listing-apis).
1. An App Developer selects your API to use in their app.
1. The API is monitored, maintained, and updated per user feedback and you own innovation.  
    For more, see [API Usage Report](/docs/tutorials/api-lifecycle/api-usage-report) and [API Versioning](/docs/general/api-design/api-versioning).
