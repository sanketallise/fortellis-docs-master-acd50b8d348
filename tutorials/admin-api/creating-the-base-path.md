---
title: Implementing the Admin API Base Path
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can gather information about the app's activations using the Admin API `basePath`.
With the Admin API, API Developers can perform the following:

* Secure their APIs.  
* Check users requesting to access their APIs.  
* Receive user information to store in a database and check the `Subscription-Id` in the request to ensure the subscriber is authorized.  

With the Admin API, you can prevent users from using your API and reject subscription requests from users.

## Process Overview

For the specs, see the [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs) and the [Fortellis Connection Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback).

1. Add `/activate` and `/deactivate` to the root directory of the URL
    you enter in the **Admin API**
    when you were registering.  
    Remember the following:  
    * Enter your URL in the **Admin API** field
    when you are registering your API.
    * Your endpoint must send a response other than 404.  
    **Note:** It will probably send a 403 response if you check for the Fortellis JWT.
    * Receive the activation request at the `/activate` endpoint and the deactivation request at the `/deactivate` endpoint: `http://example.com/activate` and `http://example.com/deactivate`.
    * Requests to the Admin API include an authorization token
    that can be verified against the public keys at `$[authServerUrl]/v1/keys`.  
    * Requests to the Admin API also include your **Client ID** in the `sub` of the token.
    * Use this URL instead of the URL
    you use to verify apps and subscriptions.  
    Please see [JWT Validation](/docs/tutorials/solution-integration/options-for-api-authorization#fortellis-oauth-2.0) for more information.  
1. Receive an activation request from Fortellis
    when Subscribers choose that API.
    * Example Activation Request URL

    ```text
    http://example.com/activate
    ```

    * Example Activation Payload

    ```json
    {
    "entityInfo": {
        "id": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d",
        "name": "New Organization",
        "address": "6701 Burnet Rd Austin TX 78757 US",
        "countryCode": "US",
        "phoneNumber": "2819234655"
    },
    "solutionInfo": {
        "id": "e911ad1f-a112-4cc5-987a-a866d6718092",
        "name": "Demo-9378162019",
        "developer": "nathan.sandberg@cdk.com",
        "contactEmail": "nathan.sandberg@cdk.com"
    },
    "userInfo": {
        "fortellisId": "test.email@cdk.com"
    },
    "apiInfo": {
        "id": "_service_parts-store_v7",
        "name": "_service_parts-store_v7",
        "implementationName": "Demo10578152019"
    },
    "subscriptionId": "7e465be1-4fef-4840-8b84-a95ab4c74f0c",
    "connectionId": "a3e9a7fa-1717-4314-8381-b9815b48f802"
    }
    ```

1. Store this information in a database
    where you can programmatically retrieve the information later
    when Subscribers use the app and call your API.  
  **Note:** API Developers receive requests with the `Subscription-Id` in the header of every API call.
1. Follow [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) to get a token from Fortellis.
1. Providers use the `connection-Id` and the [Fortellis Connection Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback) to activate the connection.  
    **Note:** Do not respond to requests if you do not wish to activate them.
    Include the `:connectionId` in the path.  
    * Callback URL and body

    ```bash
    curl --location --request POST '$[subscriptionCallbackUrl]/connections/{your connectionId}/callback' \
    --header 'Content-Type: application/json' \
    --data-raw '{"status": "accepted","error": "string"}' \
    --header 'Authorization: Bearer {your token}'
    ```

1. After activation, receive calls routed through Fortellis from the solution to your API.
