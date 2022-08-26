---
title: Configuring External Authorization
author: Nathaniel Sandberg
last updated: 3/30/2022
---

If you select **External OAuth 2.0** when registering your API, do the following:

* Use the client credentials flow.  
* Include the `access_token` in your response so Fortellis can use the correct token to access your API.  
* Include `expires_in` in your response so Fortellis can request a new token when necessary.  
* Include any additional parameters that you need for your authorization.

1. Enter one of the following:  
    * **Send as Basic Auth Header:** Use this option to set the `client_id` and `client_secret` as Basic authorization header on the token request (Recommended).
    * **Send as Request Body:** Use this option to set the `client_id` and `client_secret` with the `grant_type` and `scope` in the request body (Content-Type: application/x-www-form-urlencoded).
1. Enter your authorization server URL that Fortellis should request a token from.  
    **Note:** Include the full URL that you would like Fortellis to use
    when it gets a token.
    For example, you would use `$[authServerUrl]/v1/token` with `/v1/token` at the end to get a Fortellis token.
1. Optionally, enter any scope that Fortellis should pass in the request.
1. Do one of the following:
    * If you want all Fortellis app users to get the token with the same `client_Id` and `client_secret`,
        enter the **Client ID** and **Client Secret** that you want Fortellis to use.  
    * If you prefer to use a specific **Client ID** and **Client Secret** for each user,
        click **Subscription-specific Client ID and Secret**.  
        **Note:** The **Client ID** and **Client Secret** gray.  
        You can do one of the following to activate your connections when you receive the requests:  
        * Implement the Admin API and send the app user's `client_id` and `client_secret` in the [Connection Callback](/docs/tutorials/admin-api/fortellis-connection-callback).
        See [Subscription Activation](/docs/tutorials/configuration/updating-apis/#subscription-activation) for more information.
        * Configure the app's `client_id` and `client_secret` when you are [Managing App Subscriptions](/docs/tutorials/api-lifecycle/updating-apis/#managing-subscriptions).
1. Click **Additional authorization Parameters** to use more parameters.  
    **Note:** You must do the following for subscription-specific values:  
    1. Click **Subscription-specific** for each parameter to provide the subscription-specific values.  
    1. When users request access to your API,
        configure subscription-specific values using one of the following:
        * Use the [Connection Callback API](/docs/tutorials/admin-api/fortellis-connection-callback) when you activate connections to your API with one of the following:
            * The [Admin API](/docs/tutorials/admin-api/admin-api-overview)  
            * [Manual activation](/docs/tutorials/configuration/updating-apis/#subscription-activation)  
        * Configure the subscription-specific values when you are [Managing App Subscriptions](/docs/tutorials/api-lifecycle/updating-apis/#managing-subscriptions).
