---
title: Testing the Admin API
author: Nathaniel Sandberg
last updated: 4/6/2022
---

You have two options for testing your Admin API implementation:

* You can test the Admin API implementation using the UI
* You can test it using the Postman collection available in the UI

**Note:** If you did not elect to implement the Admin API, you do not receive this collection in the download.

## Testing the Admin API in the UI

1. Sign in to your [Developer Network Account]($[devNetworkUrl]).
1. Hover over your name.
1. Click **Change** to change organizations.
1. Select the organization from the list.
1. Click **OK**.
1. Click the API.
1. In the **Credentials** section, click **Test Activation**.

**Note:** You do not have the option to test your admin API implementation
if you chose to automatically or manually approve app subscriptions.

## Testing the Admin API with Postman

1. In the **Fortellis Sample Application**, click **Admin API Testing** > **Generate Infrastructure Bearer Token**.
1. Click **Send**.  

You must validate the information in this token against the `$[authServerUrl]` URL.

The **Activate Subscription** call does the following:

* Goes to the Admin API URL with the `/activate` endpoint appended to it.  
    **Note:** You entered this URL when you registered your API.
* Sends mock data so you can test your Admin API.
* Send the token with your **Client ID** in the subject.

## Remember the Following

* Your collection automatically populates the token in the environment variables.
* You can continue to use your test collection after the API is implemented and approved.
* Your collection automatically populates with your `Subscription-Id`.
* You must enable and populate any required path parameters.
* Apps should include a unique `Request-Id` with every call in production, but you can test your API without the `Request-Id`.
* You can continue to test your API at any time on the platform.
* You must register your API with Fortellis before you can test the API through the platform.
* If you have a payment plan for the API, calls for the test subscription continue to go through the platform.
