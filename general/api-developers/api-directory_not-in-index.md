---
title: API Directory Listings
author: Kelly Rich
last updated: 12/31/2021
---

The [API Directory]($[apiReferenceUrl]) is where **API Developers** and **App Developers** meet.

When you navigate to the API Directory, all the APIs available to you and your currently-active Organization are listed in alphabetical order. Click on any API tile to view the API's *Details* page.

An *API Developer* must pass their API through a Fortellis review process before they can list their API in the API Directory. Fortellis APIs are REST APIs that use an industry-standard API specification format (OpenAPI) to describe their use. This gives *App Developers* a consistent platform on which they can build their apps.

Fortellis APIs have many commonalities. They all use the REST architecture with JSON payloads, and the authorization model across APIs is OAuth. For API Developers, the management of app activations can be done through a common API (the Admin API), and many APIs interoperate with each other.

> **Note:** The API Directory lists both REST APIs and Async APIs, although there are currently no async APIs in production.

## Viewing APIs

Not all APIs are visible to all Fortellis members. One of the following conditions must be met before an API displays for you in the API Directory:

* The API is public API and open to all Fortellis accounts
* The API is private and:  
    * You are a member of the Organization where the API was registered (and that Organization is active)
    * You are a member of an Organization with which the API is shared (and that Organization is active)
    * Someone directly shared the API with you

To find a specific API, use the **Search API Directory** box to find an API by name, or click the appropriate API **Category** button to view APIs by type.

![The API Directory]($[docsUrl]/static/images/viewingAPIDirectory.PNG)

### Beta Tags

API Developers can request their API listing includes a "beta" tag to indicate the API is in the beta phase.

To request to have the beta tag applied to your API listing, contact [Support](mailto:support@fortellis.io).

## The API Details Page

Each API Details page offers an interface that includes the following tabs:

* **Overview** — A high-level description of the API
* **Specification** — A visual rendering of the API Spec showing the API's methods, input requirements, and outputs
* **Pricing** — Pricing information on the API
* **Documentation** — Lists the user and reference documentation provided by the API Developer
* **Support** — You can use this tab to get in touch with the support that the API Developer has provided.

All together, the information on these tabs describe the API to the level of granularity provided by the API Developer.

Each Details page also includes **API Spec** and **Build with the API** buttons, which perform the following functions:

* **API Spec** lets you download the OpenAPI specification that details the design of the API. To view the specification in a GUI interface, use the [Swagger Editor](https://editor.swagger.io/).
* **Build with this API** opens a UI flow that begins the API integration process by attaching the API to an app that you register.

![API Details page]($[docsUrl]/static/images/landingPageForSpecs.PNG)

### The Overview Tab

The **Overview** tab contains a high-level description of API. The tab is populated from information the API Developer added to the `description` field of the API Spec, and can include the following information:

* What the API is
* What the API does
* Who the intended audience for the API is

### The Specification Tab

The **Specification** tab contains a graphic rendering of API, which is generated from the API Spec.

Drill down into each method to see how to call the method, the required and optional HTTP headers and URL parameters, and the request and response payloads. If provided in the API Spec, this tab also displays example payloads for each method.

#### The API Spec

Click **API Spec** to download the OpenAPI specification for the API.

For more information on API specifications, see [APIs and API Specifications](/docs/general/api-developers/api-specs).

### The Pricing Tab

If provided by the API Developer, the **Pricing** tab contains details on the cost of using the API. Information can include:

* For transactional pricing, the amount charged for each API call
* For API subscription pricing, the amount charged on a per-subscription basis
* The contact email for API Developer  
    **Note:** Click the **Publisher** button on the Pricing tab to contact the API Developer regarding inquires on API pricing.

### The Documentation Tab

This tab lists all the PDF documentation offered by the API Developer. Documentation can include a *Developer Guide*, an *API Reference Guide*, or other updates and information.

### The Support Tab

The **Support** tab details on how to contact the API Developer, and any other support options that are available for the associated API.

Support details include:

* The contact phone number for the API Developer
* A link to the support page provided the API Developer
* A link to the terms of service uploaded API Developer
* If appropriate, additional terms from services integrated with the API

#### Sending Messages to the API Developer

To send a message through the Developer Network interface:

1. Click **Contact Support**.
1. Enter your **Message**, then click **Submit** at the bottom of the form.  
    When you send a message through Developer Network, Fortellis sends you an email confirming the message was sent.

## Resource URL

The **Resource URLs** provide the full path and endpoint.

## Request Body Structure

The **Request Body Structure** provides the structure and format of the request body for `POST` requests.

![Body Structure]($[docsUrl]/static/images/requiredIndicator.PNG)

### Response

The **Response** shows the structure and format of the response that the API sends back to the app consumers.
