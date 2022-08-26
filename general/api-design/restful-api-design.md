---
title: REST API Design
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can make APIs that use popular and standard guidelines.
Fortellis uses the REST compliant APIs because of the following:

* They provide lightweight JSON payloads in the API.
* They are industry standard.
* They are tried and tested and provide lots of documentation.

With Fortellis REST APIs, you can find reference material and transfer existing knowledge from other APIs to the work an API Developer is performing on Fortellis.

## Reasons to Use RESTful APIs

You can make great developer experiences using REST APIs for the following reasons:

* REST imposes restrictions on what you can do.
* REST makes your APIs simpler and clearer for developers.

You can use the Swagger API design language on the Fortellis Platform to perform the following:

* Design your APIs.
* Communicate that design to consumers and APIs of the API Spec.

The following are REST API design conventions which were defined in Roy Fielding’s thesis [Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm) and expanded upon later in his [blog posts](https://roy.gbiv.com/untangled/).

## Design APIs for the Consumer

Engage the API Developers while designing the API.

APIs should do the following:

* Meet the needs of a group of customers.
* Generalize an individual use case for one consumer to a group of consumers.
* Allow reuse.
* Provide solutions to common needs.

### App Developers

Perform the following to create a consumer-oriented API:

* Identify target consumers.  
    **Note:** Remember to distinguish between the needs and use cases of a particular consumer and all your consumers.
* Get to know their needs.
* Learn about the problems they are trying to solve with the API.
* Learn about the app architecture of consumers.
    * To make APIs that consumers will use, design for the interaction between the consumer and the API.
        * They can easily use the API.
        * They can easily integrate with the API.
        * They can understand the documentation for the API.
        * They can easily get started with the API.

You should do the following to create a consumer-oriented API:

* Design the API for your API's consumers, the App Developers using the API.
    * Ask the App Developers outside the organization to look at the API while you are designing it.
    * Use a contract first API design to keep the APIs consistent for your consumers.
    * Create specs with great examples to help App Developers understand how to implement your API.
    * To create an API for App Developers, do the following:
        1. Find out what the consumers need.
        1. Create an API with your backend system to fit that need.
        1. Take into account the data transformations that need to happen.
* Iteratively change the API before publishing.
    * Iterate with two parts in each phase:
        * A creative portion
        * A verification portion
    * Use these five phases:
        1. Domain Analysis
            * Build a resource taxonomy:
                * Take the users perspective.
                * Write down the usage scenario.
                * Select the nouns you would use.
                * Shortlist the nouns that would make sense as resources.
                * Analyze the relationships between the resources.
                * Express the states and transitions in a diagram.
                * Verify phase domain analysis completion with the following:
                    * A simulation
                        * Create the simulation with the API description using minimal effort.
                    * A demo app  
                    **Note:** You can just use a cURL request to demo the app.
                * Use a simulation and demo app to verify the following:
                    * The architecture
                    * Detailed design decisions
        1. Use one of the templates on [Fortellis](/docs/tutorials/spec-writing/creating-api-specs#example-specs) to architecturally design the API:  
            * Use REST and define the following:  
                * Resources
                * URIs
                * Representations
                    * Use a JSON format for REST APIs.  
                * Parameters
                * HTTP methods
                * HTTP status codes
                * Naming conventions
                * Validation  
        1. Prototype the API:
            * Prototype the API to learn as much as possible from the following groups with little effort:
                * Pilot consumers check the API for usability.
                    * Pilot consumers face the following problems with an unfinished API:
                        * Changing interfaces
                        * Broken clients
                        * Frequent updates
                        * Unavailability
                        * Low performance
                * Engineers check the API from a technical perspective.
            * You have two goals in prototyping:
                * Gain practical insights into critical implementation and usability issues by doing the following:
                    * Make the API prototype as realistic as possible.
                    * Use real data from real backends.
                * Use little effort for the creation of the prototype.
                * Use code generation to decrease the effort and time involved.
            * Do the following to create a more realistic backend:
                * If a real backend exists, integrate with it.
                * If a real backend does not exist, use a simulation.
        1. Implement the API spec for production:
            * Implement the API spec with the following in mind:
                * Implement the API spec as quickly as possible.
                * Implement the API spec fully within the company portfolio.
            * You should have done the following with the API spec at this point:
                * You have properly designed it.
                * You have sent the API spec through several feedback loops.
                * You have verified it from several perspectives.
            * Focus on properly engineering the API in this phase.
            * You must closely observe the non-functional requirements for the API at this point:
                * Security
                * Availability
                * Performance
        1. Publish the API implementation:
            * Transfer the responsibility of the API from development to operations.
            * Make the API implementation and it's documentation publicly available.
            * Freeze the interface spec.
            * Generate the API implementation and the spec from the API implementation each time to avoid discrepancies between spec and the implementation.  
            **Note:** You now have an contract to follow with all API consumers.
        1. Maintain the API implementation:
            * Clients must see the same consistent behavior from the API spec.
            * Monitor, measure, and analyze API implementation usage.
            * Include qualitative and quantitative analysis.
                * In qualitative analysis, perform the following:
                    * Draw conclusions.
                    * Discover patterns in usage.
                    * Compare all the APIs in the portfolio.
                * Figure out which use cases you correctly predicted and which you did not anticipate.
            * In quantitative analysis, do the following:
                * Determine the usage of the API.
                * Determine how successful the API is in contributing to apps.
    * You can make non-breaking changes to the API after publishing.
    * You must create a new API after every change that is not backward compatible.
* Make the API functional.
* Design the API well.
* Make the API easy to use.
* Make the API easy to integrate with.
* Document the API well.
* Make the API easy to get started with.

**Note:** Check the number of successful calls against the number of unsuccessful calls.

### Git Version Control for YAML

Use the API description of the proxy as the central source of truth for this API.

* It contains all important design decisions, including the following:
    * The version
    * All parameters
    * All status codes
    * All responses
* It provides a history of the API throughout the different versions.

You should provide the API design in a centralized repository such as git.

### Contract Between Parties

You can use the API description as the contract between the following:

* The API Developer
* The App Developer

With an API contract, API Developers and consumers can start developing at the same time.

### Use the API Spec for Code Generation

You can use the description to do the following when implementing the API spec:

* Create the endpoints for the API.
* Create the clients for the API.
* Create the tests for the API.

You have the following advantages with code generation on the client-side:

* You ensure that the client matches that contract.
* You speed up your development process.

### Client Discovery

The client can discover the API in one of the following ways:

* The App Developers understand the API spec and create the client.
* The client can explore and discover the capabilities of the API programmatically.

## Limitations of OpenAPI

The OpenAPI Specification cannot capture the following:

* The API behavior
* The backend design
* The non-functional properties

## Representing Resource Operations as HTTP Methods

Use HTTP methods to describe operations on the resources located by a URI.
HTTP methods provide well-defined semantics describing a desired action to be performed on a resource.
Use common HTTP methods to represent the following CRUD+Q operations:

* Create
* Read
* Update
* Delete
* Query

| Method | Description | Idempotent? |
|--------|-------------|-------------|
| GET    | **Reads** a resource or **Queries** a collection of resources identified by a URI. | Yes |
| POST   | **Creates** a resource. | No |
| PUT    | **Updates** (i.e. modifies) a resource. | Yes |
| DELETE | **Deletes** a resource. | No |

**Note:** When we say an HTTP method is idempotent,
we mean that
if you perform the same operation multiple times on a resource,
the resource maintains it's state.

## Representing Operation Responses as HTTP Status Codes

Use appropriate HTTP response status codes to provide a uniform interface for describing the responses of a REST operation.
This allows a client to make a well informed choice in how to interpret responses from your API.
The HTTP protocol defines five classes of response codes:

* 1XX - Informational
* 2XX - Success
* 3XX - Redirection
* 4XX - Client Errors
* 5XX - Server Errors

In most cases, you can think about what Success, Client Error, and Server Error responses an operation might return.

### Commonly Used Success HTTP Status Codes

| Status Code | Purpose       |
| ----------- | ------------- |
| 200         | **OK** - You can use this as the default status code for a successful response. You can use this in place of the others below, but it is less descriptive. |
| 201         | **Created** - You can use this to indicate when App Developers successfully create a resource.|
| 202         | **Accepted** - You can use this when an App Developer successfully requests the server perform some deferred action. It tells the client that the server will perform the requested action after receiving the response. You can use this with asynchronous behaviors.|
| 204         | **No Content** - You can use this to indicate success without a response body. You can use this to acknowledge commands where the response is implicit. For example, a 204 can be used to acknowledge successful processing of a DELETE operation on a resource. |

### Commonly Used Client-Error HTTP Status Codes

| Status Code | Purpose       |
| ----------- | ------------- |
| 400         | **Bad Request** - You can use this when the client provides invalid input in the request, including missing parameters, malformed data, or incorrect data types. You can also use this with specific API error codes. |
| 401         | **Unauthorized** - Return this when the client is not authorized and needs to authorize and try again. You may return this status code on any operation secured using access control. |
| 403         | **Forbidden** - Return this when the client is not authorized to perform the operation. Unlike a 401, you know the identity of the client. |
| 404         | **Not Found** - Return this when you cannot find a specific resource on the server. You query the collection and the result is empty. |w

### Commonly Used Server-Error HTTP Status Codes

| Status Code | Purpose       |
| ----------- | ------------- |
| 500         | **Internal Server Error** - You can use this as a catch-all for errors experienced by the server that either cannot be described more clearly using another code or where we choose not to share why the error occurred with the client. |
| 503         | **Service Unavailable** - You can use this status to indicate that the service temporarily unavailable. For example, you could use this to communicate the service is down for maintenance or the client has exceeded the rate limit. |
