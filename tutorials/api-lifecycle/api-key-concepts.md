---
title: API Key Concepts
author: Nathaniel Sandberg
last updated: 7/6/2021
---

This topic covers information API Developers should know before they begin developing on the Fortellis platform. For more information on the Fortellis platform itself, see [Fortellis Key Concepts](/docs/general/ecosystem/fortellis-key-concepts).

## APIs

APIs provide the following:

* A service that conforms to an API Spec
* Endpoints that create, read, update, or delete resources accessed by the service

## Authorization

All requests made to a Fortellis API must have a valid Subscription ID plus an OAuth token for authorization.

Fortellis uses [OAuth 2.0](https://oauth.net/2/), [OpenID Connect](https://openid.net/connect/), and [JSON Web Tokens](https://jwt.io), all industry standards, as the authorization architecture for all requests made through the Fortellis platform.

For more, see [Authorization on Fortellis](/docs/turorials/solution-integration/auth).

## API Specs

API Specs completely describe the public features of an API Service and act as the "contract" between the API Developer and App Developer.

Using API Specs, App Developers can work with API Developers to asynchronously develop and refine their APIs and apps.

For more, see [APIs and API Specs](/docs/general/api-developers/api-specs)

The API Specs you create as an API Developer should closely adhere to the following standards:

* [OpenAPI 2.0](https://www.openapis.org/) (formerly [Swagger](https://swagger.io/)) standard
* Custom Fortellis Rules  

For more, see [Linting API Specs](/docs/tutorials/spec-writing/linting-api-specs).

## Routing

Fortellis routes calls made through the platform to the correct API Developer. The following steps outline the process:

1. App Buyers choose an app in Marketplace.
1. App Buyers choose an API that works with that app.
1. The app calls Fortellis APIs on behalf of the App Buyers at `$[apiFortellisUrl]`.
1. Fortellis routes the call to the chosen API service.
1. Fortellis returns the response to the app.

With Fortellis, API Developers provide API services to users who:

* Activate apps that use their API.
* Agree to the API Developer's terms and conditions.

**Note:** App users must have an active `Subscription-Id` before they can interact with an API via an app.

## The API basePath

The `basePath` specified in an API Spec points the API Developer's host for their service implementation, and all method endpoints in the specification are based on the specified basePath URL.

Fortellis uses the specified basePath to route requests to the API, as follows:

* All apps use the Fortellis proxy URL (`$[apiFortellisUrl]`) as the host for all Fortellis API requests  
* Fortellis proxies requests to `$[apiFortellisUrl]/{organizationNamespace}[-dev|test]/{basePath}`

For more, see the [API Spec Introduction](/docs/general/api-design/api-spec-introduction).

## Admin API

The Admin API does the following for API Developers:

* Monitors activation requests
* Allows API Developers to manage activations and connections
* Collects the following app activation information:
    * The `subscription-id`
    * The App Buyers Organization information

For more information about the callback, please see [Fortellis Connection Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback) and [Registering APIs](/docs/tutorials/api-lifecycle/registering-apis).

For API Developers that don't implement the Admin API, remember the following:

* Do not enter the Admin API URL when you are registering.
* Fortellis can automatically complete the connection or you can manually complete the connection if you do not implement the admin API.
* APIs that allow automatic connections must check the activation context every time they need to fulfill an API request.

## Connections

We define a connection as the access that an App Buyer has to an API. You can think of the connection request in three contexts:

* When an App Buyer requests access to an API, that is a connection request.
* When an API Developer accepts a connection, Fortellis allows the App Buyer to access the API from the application.
* Fortellis automatically completes connections for APIs that do not use the Admin API or did not choose to accept the requests manually.

The connection represents the information going from the API to the app. You can complete these connections in three ways:

* Automatically
* Manually
* The Admin API

## Admin API Callbacks

If API Developers use the Admin API URL, they must employ the connection callback endpoint to accept connection requests.

1. The API Developer receives the activation request at `https://providerAdminAPIURL/activate` that they enter when registering the API.
1. The API Developer uses the `client_id` and `client_secret` to get a token.
1. The API Developer uses the token in the authorization header and sends the call back to the connection callback endpoint.

See the [Admin API Overview](/docs/tutorials/admin-api/overview) for more information on how to authorize requests from app users.

## Private Pricing Plans

API Developers can create private pricing plans for their App Buyers when they are updating their APIs.

For more, see [Entering Your Pricing Information](/docs/tutorials/api-lifecycle/updating-apis/#entering-your-pricing-information).

## Backend System

We use the term *backend system* to generically refer to the API Developer's systems that may not directly respond to the requests routed from Fortellis.

Remember the following about backend systems:

* The backend system may currently exist.
* The API Developer may need to create this system before exposing their APIs on Fortellis.
