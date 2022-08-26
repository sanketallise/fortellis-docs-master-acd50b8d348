---
title: Client Credentials Flow for Authorization
author: Kelly Rich
last updated: 3/31/2022
---

The **Client Credentials Flow** uses your app's **API Key** and **API Secret** to retrieve a Bearer token from the Fortellis token server. You then pass the generated token in the **Authorization** header of your API requests so the API Gateway can authorize and route your requests.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Retrieve Bearer tokens using your **API Key** and **API Secret**
* Use the Bearer token and a Subscription ID to call an API

### Things to Remember in This Tutorial

You should remember the following:

* Install [cURL](https://curl.se/download.html) to run the examples in this tutorial.

## Getting a Token from the Token Server

You call the Fortellis token server using a POST request. Format the request as follows:

1. Get your app's credentials as described in [Getting Your App Credentials](/docs/tutorials/solution-integration/getting-app-credentials).
1. Generate your *Basic token value* using the app's credentials:  
    1. Append a colon and your **API Secret** to your **API key** to create a *credential string*.  
    In other words, use your own values in the following snippet to create your unique credential string:  
        `{apiKey}:{apiSecret}`
    1. [Base64 encode](https://www.base64encode.org/) your credential string to generate a unique *Basic token value*.  
1. Retrieve a Bearer Token using your Basic token value in a request to the token server.  
    Format the request as follows:  
    1. Set the endpoint to: `POST $[authServerUrl]/v1/token`
    1. Include the following query parameters:
        * `grant_type=client_credentials&scope=anonymous`
        * `scope=anonymous`
    1. Include the following HTTP headers:
        * `authorization:Basic {yourBasicTokenValue}`
        * `content-type:application/x-www-form-urlencoded`
        * `accept:application/json`
        * `cache-control:no-cache`

**Deprecation Warning:**

> * The following token server has been deprecated: $[oldAuthServerUrl]
> * If you use $[oldAuthServerUrl] to get a token that responds to requests to the `/activate` endpoint, update your setting to the new authorization server, $[authServerUrl].

### Example Request to the Fortellis Token Server

This example uses cURL to make the request:

```bash
curl --request POST \
--url $[authServerUrl]/v1/token \
--header 'accept: application/json' \
--header 'authorization: Basic MG9hY...' \
--header 'cache-control: no-cache' \
--header 'content-type: application/x-www-form-urlencoded' \
--data 'grant_type=client_credentials&scope=anonymous' \
```

This command sends a POST request to the Fortellis token server using Basic authorization. A successful request returns a JSON object containing a Bearer token, as follows:

```json
{
    "token_type": "Bearer",  
    "expires_in": 3600,  
    "access_token":"eyJraWQiOUgz...DN76QwKzoE-uNn323Og",  
    "scope": "anonymous"  
}
```

Note that the Bearer token is valid for one hour, as indicated by the **expires_in** attribute (3600 seconds).

Be aware that when you generate an access token with your app's credentials, Fortellis ties all actions taken with the resulting token to your account.

## Using a Bearer Token in an API Request

If an API uses Fortellis tokens for authorization, supply a valid Bearer token in the **authorization** header of the request and include the App Buyer's Subscription ID in the **Subscription-Id** header.

Of course, you must also supply any other request headers, URL parameters, or a request payload, as the method you are calling requires.

1. Complete the endpoint using the HTTP method (such as POST) with the method's complete URL path.
1. Include the Bearer token in the **authorization** header of the request.
1. Identify the App Buyer to the API Developer and to Fortellis by including the **Subscription-Id** in the header of the request.  
    The **Subscription-Id** is created when the App Buyer activates to your app.  
    You can opt to receive the **Subscription-Id** in one of the following ways:
    * Email
    * API

> Fortellis passes the **Subscription-Id** to the API service when it routes the request. This is so the
> API Developer can validate the user's connection and access their app activation information.

### Example API Request Stub

```bash
curl POST $[apiFortellisUrl]/{basePath}
    -H "authorization: Bearer {yourBearerToken}"
    -H "Subscription-Id: {currentSubscriptionId}"
```

## Next Steps

In this tutorial, you should have learned how to do the following:

* Retrieve tokens using your **API Key** and **API Secret**
* Use a Bearer token and a Subscription ID to call an API
