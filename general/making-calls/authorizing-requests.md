---
title: Authorizing API Requests
author: Kelly Rich
last updated: 10/05/2021
---

Fortellis uses [OAuth 2.0](https://en.wikipedia.org/wiki/OAuth#OAuth_2.0), relied on by industry leaders, as the authorization protocol for all requests.

OAuth outlines specific authorization flows and you can use the following OAuth flows on Fortellis, as your needs require:

* Client Credentials Flow
* Authorization Code Flow
* Implicit Flow
* Refresh Token Flow

For more details on generating OAuth tokens and on the different types of tokens used in the Fortellis environment, see [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/).

Be aware that in addition to a valid access token, you must also supply a Subscriber ID with an *active* status to make calls to the Fortellis API services.

## The Client Credentials Flow

You authorize each request you make to any of the live Fortellis environments (**Test**, **Dev**, and **Production**) with an OAuth access token that was created with the *client credentials flow*. The client credentials flow makes use of the Fortellis token server to create a token that you provide in the HTTP **Authorization** header of your requests.

To get a valid *Bearer* token, call the Fortellis token server with the *Basic token value* generated using the credentials assigned to your app. The token server mints a fresh, limited-lifespan access token and returns it in a JSON response.

The following cURL command shows how to configure a client credentials request to the token server:

``` bash
curl --request POST \
--url $[authServerUrl] \
--header 'accept: application/json' \
--header 'authorization: Basic <basicToken>' \
--header 'cache-control: no-cache' \
--header 'content-type: application/x-www-form-urlencoded' \
--data 'grant_type=client_credentials&scope=anonymous'
```

To make this call:

* Set the **authorization** header to the generated *Basic token value*
* Set the **grant-type** to `client_credentials`
* Set the **scope** to `anonymous`

When successful, this request returns a JSON response with the access token returned as the value for the **access_token** attribute, and the lifespan of the token, in seconds, returned in **expires_in**.

``` json
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "access_token": "eyJhbGciOiJSUzI1NiIs...LNw644ZB3srGry2rRw",
    "scope": "anonymous"
}
```

Note that Fortellis tokens expire in `3600` seconds, or 1 hour.

### Generating a Basic Token Value

Generate a *Basic OAuth token* value by Base64-encoding your app's credentials (your *API Key* and your *API Secret*) to create a *credential string*, then Base64-encode that string. In detail:

1. Append a colon (":") to your app's API Key value, then append the API Secret value to that string to create your credential string.  
    This example uses variables to show your unique **credential string**:  

    `{apiKey}:{apiSecret}`

1. [Base64 encode](https://www.base64encode.org/) your credential string to generate your unique *Basic token value*.
