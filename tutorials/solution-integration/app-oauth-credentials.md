---
title: Getting Your App Credentials
author: Kelly Rich
last updated: 4/1/2022
---

Fortellis generates a set of *OAuth credentials* for every app you register. The credentials consist of an *API Key* and an *API Secret*, which are also formally known in OAuth terms as the *client ID* and *secret*.

## The App Credentials

After registering an app, Fortellis generates a set of credentials for the app that you can view on Details page of the app.

The app credentials are comprised of the following values:

* **API Key** – You identify the app to Fortellis with the API key.
    Within the URLs and API calls, you may see this referred to as the "client_id".
* **API Secret** – You use this value with the API Key to generate a Base64-encoded value that send to the Fortellis token server retrieve your OAuth Bearer token.

### Getting the API Key and API Secret

1. Sign in to the [Developer Network Platform]($[devNetworkUrl]) and enable the Organization in which the app is registered:  
    1. Hover over your name in the upper-right corner and click **Change**.
    1. Click the Organization where the app is registered, then click **OK**.
1. In the App section, choose **View All** and find the app whose credentials you want to retrieve.
1. Click on app listing to open the app's Details Page.  
    The app's OAuth credentials are listed on its Details Page.
1. Copy and save the app's **API Key** and **API Secret**.

> **Warning!** While your app's API Key may get shared with others (so they can verify your authorization token), it is imperative that you keep your app's API Secret stored in a secure location. Do not share your API Secret with other users.

![The API Credentials for an app]($[docsUrl]/static/images/api-credentials.jpg)

## Using the Credentials

App Developers must provide the following information in the HTTP headers of the API requests they make through their app:

* A Bearer token to authorize a call to a Fortellis API
* The `Subscription-Id` to identify the user's active app

In addition to the authorization token and Subscription ID, App Developers must also supply any required request body fields that are indicated for the API endpoint.

App Developers must also approve their users in Marketplace before sending requests to APIs.
