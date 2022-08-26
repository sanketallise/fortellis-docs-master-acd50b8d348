---
title: API Architecture
author: Nathaniel Sandberg
last updated: 3/31/2022
---

In the following, you can learn more about API architecture to create better APIs on Fortellis.
This article includes the following considerations to help you understand how to create better APIs:

* Stakeholders
* Design decisions
* Platform benefits
* Building APIs
* Domain principles
* API language decisions

With this information, you can make better APIs on Fortellis with more information from your stakeholders and better design decisions.

## Definition of API Architecture

You can see API architecture from the following perspectives:

* Architecture may refer to the architecture of the complete solution.
* API architecture may refer to the entire portfolio and how the enterprise manages it.
* API architecture may refer to the API descriptions.

API platforms like Fortellis provide an infrastructure for doing the following:

* Developing APIs
* Proxying Requests
* Managing API subscriptions
* Marketing APIs

## API Architecture's Importance

Design the API for nonfunctional properties, such as the following:

* Security
* Performance
* Availability

## Stakeholders in APIs

You must support three API stakeholders
when you create a spec to implement:

* API Developers
* App Developers
* API End Users

### API Developers

API Developers build and expose APIs.

### App Developers

App Developers build components that interact with the API and deliver information to the end user.

### End Users

The end user uses the app or web site that the App Developers built.

## API-related Design Decisions

You need to make a few architectural design decisions discussed below.

### Apps Commonalities

All apps have the following in common:

* They all use distributed systems consisting of a client and a server.
* Apps may be one of the following:
    * Mobile apps
    * Cloud apps
    * Web apps

### Functionality in the Client or the App

You cannot put some functionality in the app, including the following:

* Heavy computations
* Persistent data storage
* Storage across severs
* Storing sensitive data
* Accessing real-time data

### Use an Existing API or Build a New API

App Developers prefer to use an existing API for the following reasons:

* They can more quickly integrate with an existing API rather than building a new one.
* They can usually build the API client more quickly if the API already exists.

App Developers sometimes have to create their own APIs
because they cannot find an API that fits their exact requirements.

## API Platform Architecture

You can use the Fortellis platform for the following reasons:

* API Developers can focus on building APIs that consumers love.
* API Developers can reuse existing processes for creating APIs.
* API Developers can build the APIs more quickly.

You can use the following components on the Fortellis platform:

* With the Developer Network, you can do the following:
    * Implement APIs using your specs.
    * Manage your interactions with end users.
    * Create apps for APIs.
* With the API Directory, you can do the following:
    * Display your spec to external users.
    * Show your example response payload in a UI.
    * Include additional documentation for you users.

## Fortellis Developer Network Platform

Fortellis offers the following tools for creating your APIs on Fortellis:

* Spec building blocks
* YAML guides
* API spec instructions
* An API Directory
* Self-service options

### Building Blocks for Creating Your API

* **Security:** Use Fortellis and the Fortellis security patterns:  
    * Use one of the following methods to verify Fortellis users:  
        * Fortellis OAuth 2.0  
        * External OAuth 2.0  
        * Custom OAuth 2.0  
        * Basic Authentication  
        * API Key and Secret  
    * Use the `Subscription-Id` to identify the Subscribers to your API.  
    * Use the token to determine when your users authentication has expired.  
* **REST:** Use the standards of REST to send and receive data through Fortellis.  
* **JSON:** Use Fortellis to transfer data efficiently in a JSON format.  
* **Monetization:** Use Fortellis to monetize the API.  
    **Note:** Currently, Fortellis can display your transaction costs,
    but you must arrange for payments between you and your subscribers or App Developers.  
* **Analytics:** Use Fortellis to see analytics on your API usage.  
* **Cache:** Use Fortellis to cache requests and provide reduce the number of calls to your API.  

### Designing APIs with the OpenAPI Specification

You can use the standardized OpenAPI 2.0 or OpenAPI 3 to create your API specs and design your APIs.

To check your specs, Fortellis also provides the following tools that share the same rules:

* Fortellis CLI
* [Fortellis Spec Tools](https://marketplace.visualstudio.com/items?itemName=Fortellis.fortellis-spec-tools)
* Fortellis UI

You can also use the instructions and guides in [Creating Specs](/docs/tutorials/spec-writing/creating-api-specs).

### API Directory

Once you have created your spec and it has been approved,
consumers can do the following with your API spec:

* Find the spec on the API Directory.
* Find the parsed spec in the UI in the API Directory.

Once you have created your specs, consumers can find your specs in the API Directory.

### Self-Service Options

You can use the following self-service options on Fortellis:

* Create your specs.  
* Use tools to make sure they are correct.  
* Upload your specs.  
* Register your API.  

## API Configuration and Interactions

Fortellis provides the following environments:

* API Developers can use the following statuses:
    * Provisioning
    * Ready
* App Developers can use the following statuses:  
    * Registered
    * Submitted
    * Approved
    * Published

You can see the transition from each of these to the next in the Developer Network Platform.

### Managing User Identities

Fortellis identifies API Developers and App Developers with Fortellis authorization tokens and subscribers with the `Subscription-Id`.
You can verify the tokens with the Fortellis identity server,
and you can use the Admin API or the Connection Management UI to get the `Subscription-Id`.

### Information from Backends

The API needs to do the following with the information from the backend systems:

* Aggregates it
* Filters it
* Processes it
* Standardizes the data

### API Principles

You can use the following principles to make easily understood APIs for your API consumers:

* Service orientation
* Reuse
* Separation of concerns
* Loose coupling

## Fortellis API Domain Principles

### Consistency

Use consistency guidelines
when you are designing APIs within your `basePath`
so your users can predict how your API works.
Design your consistency guidelines around the following:

* Use a common structure for resources and representations.
* Use the same representations for the same fields on different APIs.
* Use common patterns across all APIs in the portfolio.
* Use input validation rules across all your APIs.
* Use the Fortellis authorization tokens or your authorization tokens to ensure that the API requests are authorized.

### Reuse

You should determine how to reuse different elements of an API at the beginning of development.
You can reuse the following:

* The same API schemas across APIs to keep your requests and your responses consistent.
* The API patterns of other APIs.
* Complete APIs.
    * Your users should use the API in several solutions.

### Discoverability

Use the following strategies to let developers find your APIs on Fortellis:

* You can let your App Developers find your documentation on the [API Directory]($[apiReferenceUrl]).
* Use HATEOAS to let App Developers find other methods automatically through the links in the returned response.
* You can also list the `OPTIONS` for that API with the `OPTIONS` method on a particular endpoint.

### Change Management

You have to manage the change of the API on Fortellis for the following reasons:

* The API has a published interface.
* App Developers are often unwilling to change the apps to accommodate changes from the API Developer.
* You may have to contact all your consumers to let them know you are changing the API.
* You may not have control over the different apps consuming your API.

You may face the challenge of evolution for the following reasons:

* Usually, the business would like to publish the API and monetize it as quickly as possible.
* The App Developers want to start developing with live data from the API.

You can introduce the API to a few pilot consumers to compromise between the IT and the business pressures.

#### Classifying API Evolution

You can classify two types of changes to APIs:

* Non-Breaking Changes
* Breaking Changes

##### Non-Breaking Changes

You consider a change non-breaking if an app can still interact with an old API.
See [Non-Breaking Change](/docs/tutorials/api-lifecycle/api-versioning/#non-breaking-changes) for a list of these.

##### Breaking Changes

You consider a change breaking if an app cannot interact with a new API.
See [Breaking Changes](/docs/tutorials/api-lifecycle/api-versioning/#breaking-changes) for a list of these.

#### Dealing with Evolution in APIs

Use [URI Versioning](/docs/tutorials/api-lifecycle/api-versioning/#uri-versioning) to deal with evolution to keep the old APIs and apps working.

#### Anticipating and Avoiding Evolution

You can do the following to avoid evolution:

* Avoid changes that would break the APIs at all costs.
* Avoid changes that are not absolutely necessary.
* Make the API correctly the first time you publish it.

##### Prevent Feature Creep

You must implement new features conservatively to keep the API the following:

* Simple
* Easy to provide
* Easy to consume

## Additional Considerations

You should consider the following desirable traits for App Developers:

* Keep the front-end simple despite any complications in the backend system.
* Keep a low barrier of entry for App Developers.
* Provide documentation because some consumers prefer reading the documentation on the API.  
    **Note:** Consumers see the documentation
    that you provide to Fortellis in the [API Directory]($[apiReferenceUrl]).
    See [Entering Your Documentation](/docs/tutorials/api-lifecycle/updating-apis/#uploading-documentation) for more information on uploading your API documentation.
* Only show the consumers one object despite how many steps you take on the backend.
* Give consumers good performance from the API.
* Make the API to scale over time to accommodate more users.

### Client-Server Patterns

Use a separate component for the client and for the server.

## API Languages

The platform should use two different languages:

* An API description language that describes the API at a very high level
    * Fortellis uses the OpenAPI specification to describe APIs.
* An API development language that you can use to implement the API
