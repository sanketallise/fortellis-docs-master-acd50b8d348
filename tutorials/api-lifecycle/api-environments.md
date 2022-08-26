---
title: API Testing and Production Environments
author: Kelly Rich
last updated: 4/6/2022
---

When you configure a new API, the **Environment** setting designates the host environment for the resulting API service. The environment you select is reflected in the API method endpoints configured in API Gateway. These are the endpoints an app must use to access the API service.

Fortellis supports the following environments:

* **Production** – An API must be live in Production before it can be listed in the API Directory and available to third-party apps.
* **Test** – APIs hosted in the Test environment are available to other Organizations who can test your API with their apps.
* **Dev** – APIs hosted in the Dev environment only process requests from the API's test subscriptions.

The [API Directory]($[apiReferenceUrl]) includes public APIs hosted in the **Test** and **Production** environments, but the directory does not include public APIs hosted in the **Dev** environment.

In some cases, the API Developer provides a test `Subscription-Id` to use during app development. In these cases, you can only call the Test endpoint with the provided test `Subscription-Id`.

**Note:** You must create a new version of an API if you want to change the either the **Environment** or **Namespace** fields. You cannot modify these properties after the API has been created.

## Testing APIs

Use the **Test** and **Dev** environments to test an API. Testing an API on the Fortellis platform:

* Ensures the service is working on the platform
* Ensures API consumers can access the API on the platform
* Lets you test the endpoints while implementing the service and refining the API Spec

## Going Live to Production

Once testing is complete, create a new API listing and set the **Environment** to `Production`. Hosting an API in the Production environment allows you to list the API in the API Directory as well as giving you the ability to share the API third-party developers.

> **Note:** When you move an API to Production, any apps that access the API must be updated so all API requests point to the Production environment.

## Troubleshooting

Be sure to try the following methods when testing an API:

1. Test the API with a Postman collection.
1. Test the API directly from an app or via cURL.

If you cannot get an API to respond, or if you need assistance with any APIs, contact [Fortellis Support](mailto:support@fortellis.io).
