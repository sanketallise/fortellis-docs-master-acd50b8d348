---
title: Configuring Custom Authorization
author: Nathaniel Sandberg
last updated: 3/30/2022
---

If you select **Custom OAuth 2.0** when registering your API, you indicate that will use your own authorization server.

To configure your authorization server when registering an API, do the following for all API subscription-specific values:

1. Click **Subscription-specific** for each parameter to provide subscription-specific values.  
    **Note:** You must include a **Display Name** for all **Subscription-specific** values.  
1. When users request to access your API,
    configure subscription-specific values using one of the following:  
    * Use the [Connection Callback API](/docs/tutorials/admin-api/fortellis-connection-callback) when you activate connections to your API with one of the following:  
        * The [Admin API](/docs/tutorials/admin-api/admin-api-overview)  
        * [Manual activation](/docs/tutorials/configuration/updating-apis/#subscription-activation)  
    * Configure the API subscription-specific values when you are [Managing App Activations](/docs/tutorials/api-lifecycle/updating-apis/#managing-app-activations)

## Requesting a Token

1. Select the method Fortellis needs to use to validate the token:
    * **POST**
    * **PUT**
1. Enter the the URL of your authorization server in the **Auth URL** input box.
1. Enter the **Content Type** header the authorization server expects with the validation request.

## Request Headers

1. To add header values for your custom authorization, click **Add Header**.
1. Check **Subscription-specific** to make the header specific to each API subscriber.
1. Enter the following for each of the header parameters:  
    * **Name**  
    * **Value**  
    * **Display Name**  
    **Note:** You do not have to enter **Subscription-specific** values because that information gets sent when you receive an API subscription request from Fortellis.

### Request Payload

1. Enter the **Name** and **Value** pairs to send in the request.
1. To provide a **Subscription-specific** value, check the box.
1. Follow the instructions to send the subscription-specific values to Fortellis when a user subscribes to your API.
1. To add additional parameters, click **Add Parameters**.

## Token Response

Under **Token Response**, enter the name of the token field in **Attribute Name** and the **Expiration** time, in seconds.

Fortellis uses the **Attribute Name** to find the token in your custom authorization response. If you enter `deep.deeper.token` with the custom token shown below, Fortellis parses the JSON to get the token.

```json
{
    "token_type": "Bearer",
    "expires_in": 3600,
    "deep": {
        "deeper": {
            "token": "eyJhbGciOiJSUzI1NiIsInR5cCI6Ikp...GcvoRrGR4hkqs2UvIXuEK_hidK52VS5Oak04yCTKw"
        }
    },
    "scope": "anonymous"
}
```

Currently, Fortellis parses only objects (no arrays).

## Token Prefix

Enter the **Token Prefix** value for this field.

## Additional authorization Parameters Included on API Requests

1. Click **Add Parameters** to add any additional parameters in the Fortellis request to your API server.
1. Check **Subscription-specific** to provide a subscription-specific value.
1. Enter the following for the additional parameters:  
    * **Name**
    * **Value**
    * **Type**  
        * **Header**
        * **Query**
    * **Display Name**
