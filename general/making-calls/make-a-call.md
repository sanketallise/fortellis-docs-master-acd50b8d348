---
title: Making an API Call
author: Kelly Rich
last updated: 7/13/2022
---

Making your first API call in Fortellis is **1**, **2**, **3** steps away.

This exercise shows you how to call the **CDK Drive Customer API** in a few minutes using the credentials you get when you register an app.

## 1. Sign in to your Fortellis Developer Network account

For directions on how to create an account and sign in to the Fortellis Developer Network, see [Getting Started](/docs/general/overview/getting-started).

## 2. Get your OAuth credentials

In this step you create a Fortellis app, select an API, and get the credentials assigned to your app.

To begin, sign in to the Developer Network, then:

1. On the Developer Network, hover over your name and choose **Developer Account**.  
    The Developer Account page displays and shows the APIs and Apps registered to your current [organization](/docs/general/overview/organizations):  

    ![The Developer Account page]($[docsUrl]/static/images/developer-menu.jpg)  

1. If you don't have any Apps in your organization, register a new app:
    1. Click **New App** to display the **Register a New App** page.
    1. In the **App Details** section of the page, enter the **App Name** and **App Description**.  
        For now leave the **Website** and **Callback URL** fields blank, you can complete them at a later date.  

        ![The new app form]($[docsUrl]/static/images/register-new-app.png)  

    1. In the **API Integrations** section:  
        * Select **Categories ->> All Categories**
        * Select **CDK Global - Fortellis Development**
        * Check **Customers (CDK Global - Fortellis Development)**
    1. In the **Selected APIs** section, agree to the listed **Pricing** and **Terms** for the selected APIs.
    1. Click **Register** to begin the provisioning of your app.  

        ![Selecting an API]($[docsUrl]/static/images/select-api.png)  

        Depending on a variety of factors, this process may take a minute. When complete, Fortellis creates an entry for the app on your Developer Account page and you can open the app to get your credentials.
1. With at least one app listed in the Apps section of your Developer account, click the app to open it, then scroll down to the **Credentials** portion of the App's configuration.
1. Copy and paste the values of the **API Key** and **API Secret** into a text editor, they're needed in the next step.  

    ![The API Credentials for an app]($[docsUrl]/static/images/api-credentials.jpg)

## 3. Make the call

Every call to a Fortellis API requires you *authorize* the request with an OAuth access token and the type of app you create dictates the type of OAuth flow you use for the authorization.

Often, apps make use of the OAuth *Client Credentials Flow* to generate a "bearer" token that is passed in the calls made by the app. For more on authorization and generating Bearer tokens, see [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/).

With an active Subscription ID and a valid Bearer token value, you can make a call to a Fortellis API using a call structure similar to the one shown in the cURL snippet below:

``` bash
curl -X GET "$[apiFortellisUrl]-test/cdkdrive/crm/v1/customers" \
  -H "Authorization: Bearer {bearerTokenValue}" \
  -H "Subscription-Id: activeSubscriptionId"
```

*Voi La!* If everything goes as planned, the server responds with a JSON string that contains the full list of customers entered into the Test environment.

Congratulations on your first step as a Fortellis developer.
