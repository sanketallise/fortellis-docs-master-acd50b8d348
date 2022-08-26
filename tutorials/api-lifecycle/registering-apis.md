---
title: Registering APIs
author: Nathaniel Sandberg and Kelly Rich
last updated: 4/1/2022
---

Registering an API lets you expose the API to other Fortellis App Developers. When registering an API, you create and configure the following:

* API Specification
* Namespace
* Version and Environment
* Target URL
* Visibility
* Access Control
* Lifecycle Status

On Fortellis, you register a new API in an Organization, and the Organization where you register the API determines who can initially see the API listing.

## Creating a New API

Complete the **New API** form to register an API. To navigate to the form:

1. Sign in to your [Developer Network Account]($[devNetworkUrl]).
1. Choose the Organization where you want to register the API:
    1. Hover over your name in the to right corner and click **Change**.
    1. Select the desired Organization and click **OK** to change Organizations.
1. In the Fortellis header, click your name, and then click **Developer Account**.
1. Click **New API** to view the *New API* registration form.

The initial page of the form lets you select the environment where you plan to run the service, plus an input box where you can upload your API Spec.

From this page, the rest of your API configuration depends on the type of API you are registering, a synchronous REST API, or an asynchronous API that uses Event Relay.

![New API registration form]($[docsUrl]/static/images/new-api-form.png)

### The API Environments

Select the environment for the API from the **Environment** drop-down.

You can run an API implementation in any of the following environments:

* Production
* Test
* Dev

The endpoints for the methods in the API are determined by the environment you select. For a description of the available environments, see [API Environments](/docs/tutorials/api-lifecycle/api-environments).

### The API Specification

You should upload a Swagger 2.0 API specification to define the interface exposed by Fortellis API Gateway and the technical reference in the Fortellis API Directory.

> For more on creating API Specs, see [APIs and API Specifications](/docs/general/api-developers/api-specs) and [Creating Specs](/docs/tutorials/spec-writing/creating-api-specs).

To upload an API specification:

1. Click **Choose File**, then select the `yaml` or `json` file from your computer.  
    **Note:** Ensure the `version` and `basePath` declarations in the API Spec match your implementation's details.  
    When your spec finishes uploading, the UI displays the following API details:
    * **API Name** – Fortellis uses the title in the spec for the name of the API.
    * **Description** – Fortellis gets this information from the description in the spec.
    * **Category** – Fortellis get this information from the tags in the spec.
        Users can filter the APIs in Fortellis by the tags. You can use any tags you want, but the API Directory only filters APIs by the pre-existing categories.
    * **Version** – Fortellis uses the version declared in the spec.
    * **Proxy URL** – Fortellis generates the proxy URL with the environment and the namespace that App Buyers use to call your API.  
        **Note:** Fortellis does not include the test environments (Test, Dev) in the URL for production APIs.
1. Click **Save**.

> **Note:** If you receive the error "Basepath already exists, you must update the basepath in your API Spec so it is unique for each API Spec."
> Contact [Fortellis Support](mailto:support@fortellis.io) to request a new namespace, or change the `basePath` to use the same Organization namespace.

### Namespace

Fortellis uses namespaces to ensure `basepath` conflicts do not occur between APIs. Requests are sent to your API at the URL described below, and all method endpoints in the specification are based on this basePath URL:

```bash
$[apiFortellisUrl]/{organizationNamespace}[-dev|test]/{basePath}
```

Keep in mind:

* You do not need to include `{organizationNamespace}[-dev|test]` in your basePath in the spec.
* You do not need to include `/{organizationNamespace}` with your URL when you are entering `{yourTargetURL}`.
* If you have multiple namespaces registered in your Organization, a **Namespace** dropdown input box displays and you must select the correct namespace for the API's implementation from the dropdown.

Fortellis does not restrict how you format the basePath of an API, as long as it points to the intended implementation. We recommend you include the major version of the API Spec in the specified basePath.

### Target URL

The target URL is the public location where Fortellis will proxy requests to your underlying API implementation.

Calls to your API through Fortellis take the following path:

1. For the `/{yourBasePath}/{yourEndpoint}`, API users call `$[apiFortellisUrl]/{yourNamespace}-dev/{spec-basePath}` for the development environment and `$[apiFortellisUrl]/{yourNamespace}/{spec-basePath}` for the production environment.
1. Fortellis routes the call to the URL that you entered for your API plus the `{yourBasePath}/endpoint`, such as `yourURL.com/{yourBasePath}/{yourEndpoint}}`.
1. Your API sends the response back to Fortellis.
1. Fortellis sends the response back to the app.

## API URL and Admin API URL

When API Developers are registering their APIs, they can fill out two URLs for their APIs:

* The API URL
* The Admin API URL

At the API URL, API Developers should do the following:

* Receive all calls at their URL plus the `basePath`
* Receive calls at all the endpoints in the spec
* Use the adapter to send the calls to their backend if they need an adapter

At the Admin API URL, API Developers should do the following:

* Receive connection requests at the `/activate` endpoint
* Store information in the request on their backend
* Receive notifications of deactivations at the `/deactivate` endpoint

### Visibility

To show the API in the API Directory, make it public by clicking **Show in API Directory**.

For more, see [Listing APIs](/docs/tutorials/api-lifecycle/listing-apis).

### Access Control

You can configure your API to work with several types of user authorization.

To configure the authorization method used by your API when registering your API, select the from the options listed on the **Access Control** dropdown menu.

Follow the instructions given in the individual configuration topics for the following authorization options:

* [Fortellis OAuth 2.0](/docs/tutorials/solution-integration/fortellis-oauth-config)
* [External OAuth 2.0](/docs/tutorials/solution-integration/external-oauth-config)
* [Custom OAuth 2.0](/docs/tutorials/solution-integration/custom-oauth-config)
* [API Key and Secret](/docs/tutorials/solution-integration/key-secret-oauth-config)

### Authorization Tips

> * If you select an authorization method other than Fortellis, be sure to provide the correct credentials so Fortellis can verify requests via your authorization service.  
> * While you can update your authorization method at any time, beware of breaking existing customer connections if you do so.  
> * You can configure a custom authorization method at any time whether with any customer, regardless of if they are already active.
> * You cannot automatically accept app activations if you choose to have activation-specific values for your API.  
> * You may break existing activations if you incorrectly configure the activation-specific values for your app users.  
> * You must update your users' API activation-specific values if you change the API to require new activation-specific values.

## Completing the API Registration

After completing the API Registration Form, click **Create API** to be automatically redirected to the API Details page for your new API.
