---
title: Developing APIs
author: Kelly Rich
last updated: 3/23/2022
---

In a broad sense, a Fortellis **API Developer** implements an API service that runs on the Fortellis platform.

Fortellis API services interact with data stores used in the dealership vertical, including DMS, CRM, OEM, and other systems used in the industry. API Developers leverage Fortellis to give their APIs visibility within the automotive industry.

When an API implementation is market-ready, it can be made public and listed in the [API Directory]($[apiReferenceUrl]), the location where App Developers look to find new APIs that they can integrate into the solutions they offer.

## Platform Support

Fortellis supports the development of APIs by providing the following:

* Platform Support to API Developers
* Up-to-date tools and resources for developing and deploying APIs
* An interface that streamlines the development process
* Self-service configuration options

Fortellis lowers the cost for an API Developers to develop APIs and by using the Fortellis Developer Network and platform, API Developers can:

* Offer APIs to multiple App Developers
* Use a proxy to provide security for their APIs
* Manage access to their APIs through Fortellis
* Publish and circulate industry-standard API Specs

## API Specifications

An API specification defines the inputs, outputs, and functionality of an API.

A Fortellis *API spec* is formatted in a YAML or JSON descriptor file that follows the [OpenAPI Specification](https://swagger.io/specification/). The spec defines the inputs and outputs of the RESTful *methods* supported by the service.

The API Specification provided with an API must fully define a service as it's considered to be the "contract" between the API Developer, who provides the service, and the App Developer, who integrates the service into an app.

> For information on creating specs, see [Creating API Specs](/docs/tutorials/spec-writing/creating-api-specs).

## The Fortellis Proxy Server

API Developers implement services that interact with dealership DMS systems and users make REST requests to interact with the services.

As an API hosted on the Fortellis platform, the Fortellis proxy acts as the service host for requests made to the API (`$[apiFortellisUrl]`), and the API Gateway routes all calls to the proper service endpoints.

With OAuth 2.0 used as the authorization mechanism, Fortellis offers a secure platform for the APIs it hosts.

## Using the Testing Environments

Fortellis provides three environments where you can host your API at different stages of the implementation process:

* **Dev** — Use this environment for testing within your Organization.
* **Test** — Use this environment for testing across Organizations.
* **Production** — Use this environment to offer the API to an unrestricted audience as a General Release product.

> For more on the Fortellis test environments, see [API Environments](/docs/tutorials/api-lifecycle/api-environments).

## Making an API Public

APIs are created in a Fortellis Organization and once created, the API is visible to all the members of that Organization.

By default, new APIs are "private" and are visible to only those within the Organization containing the API. To give access to an API to those outside your Organization, you can invite a new member into your Organization, or you can explicitly share the API outside of your Organization.

You can also turn the API into a "public" API, enabling you to list the API on the API Directory where you can share it with the Fortellis-member audience.

When making APIs public and listing them in the API Directory, be aware of these best practices:

* [Best Practices for Listing APIs and Apps](/docs/general/public-apis-apps/listing-apis-apps)
* [Marketing APIs and Apps](/docs/general/public-apis-apps/promoting-apis-apps)

> For more, see [API Development Lifecycle](/docs/tutorials/api-lifecycle/api-lifecycle).
