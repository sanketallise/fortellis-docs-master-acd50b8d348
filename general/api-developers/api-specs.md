---
title: APIs and API Specifications
author: Kelly Rich
last updated: 12/31/2021
---

A production-ready API consists of the *API service implementation* and the *API Specification* that describes the programmatic interfaces of service.

Remember that the API Spec is not the implementation of the service. It is only a description of the public-facing aspects of the service. The implementation of the service stands apart from the specification and it is up to the API Spec to accurately describe the inputs and outputs of the service.

During the initial phase of API development, developers with access to the API Spec can start developing apps based on the specification, and the accuracy of the API Spec can be verified in concert with the service implementation.

For the best-in-class API service, API Developers should provide an API Spec that completely and accurately defines the usage and functionality of the API's methods. The specification should include information on how to make a call to each method in the API, the details of each method's input and output payloads, and the possible HTTP response statuses.

To help App Developers consume the service, API Specs should also provide accurate request and response examples for each method. Although, comprehensive details on how to use the service can be included in an accompanying *Developer Guide*. For more, see [Creating API Developer Guides](/docs/tutorials/api-lifecycle/creating-api-developer-guides).

## API Spec Details

API Specs are OpenAPI compliant and contain the following information:

* The *basePath* used by the API
* The *authorization* method used by the API  
    In addition to any security model required to access the API, all services must use Fortellis authorization to authorize on the Fortellis platform.
* The *methods* supported by the API  
    Methods are defined by and HTTP method and an associated endpoint URL.
* For each API method, the spec must define:
    * *HTTP request* and *response headers* used by the method
    * *URL parameters* used by the method
    * The *request payload* required by the method, if used
    * The *response payload* for the method, if returned
* Example data for each request and response

For more details on specifications, see [Creating API Specs](/docs/tutorials/spec-writing/creating-api-specs).
