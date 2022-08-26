---
title: Accessing Subscription Details Programmatically
author: Nathaniel Sandberg
last updated: 6/14/2022
---

You can get a `subscriptionId` list via an API for all dealers subscribed to an app.
You can get this information from the **Subscription Report** in your [**Developer Account**]($[devNetworkUrl]) account.
You can also use the following endpoints to get this information:  

* $[subscriptionCallbackUrl]/v1/solution/subscriptions

    |Parameter|Format|Type|Location|Value|Required|  
    |--|--|--|--|--|--|  
    |`Authorization`|String|Bearer|Header|Use [Getting a Token from the Token Server](/docs/tutorials/solution-integration/client-credentials-flow#getting-a-token-from-the-token-server) for information on getting your token.|Yes|
    |`entityId`|String|UUID|Query|Use the `entityId` to filter by the `subscriptionId` `entityId` or `organizationId`|No|
    |`orgId`|String|UUID|Query|Use the `orgId` to filter by the `subscriptionId` `entityId` or `organizationId`|No|
    > Fortellis determines that the `subscriptionId` is your `subscriptionId` based on the token with your app API Key and API Secret.
    * You can see an example request below:  

        ```curl
        curl -X GET \
        $[subscriptionCallbackUrl]/v1/solution/subscriptions \
        -H 'Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiJXZEhZQURpNlk3dUpTR1I4M0RGUjhxUXBXYU9kUzBQMCIsImp0aSI6IkFULi1GVkx4VmtfMmoyOU9JUHN1UUFiLWNDQlIxTmlFZHh0cDVRVzZUTXhPSHMiLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IldkSFlBRGk2WTd1SlNHUjgzREZSOHFRcFdhT2RTMFAwIiwiaWF0IjoxNjU0NzI2NDE2LCJleHAiOjE2NTQ3MzAwMTYsImF1ZCI6ImFwaV9wcm92aWRlcnMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.KFR7IdA4Dxcd9VAJkmxBALwZThij2b2HjF5KGlGrXgfOWHZrrs_V8vq2TKjOqyGIkqFxr8wmRaaewgHt8bHCTFe06cHV4vprun4v6AahEQIfHdhrGMkVkNOVVVYjd-GBl7rlA2ZPYnQNFWocI7WF-oNYfD6oUP-_Syl3NYYq6AHyk4XazmSrijg5wK1DJ-j6fvv1sXpWaGauYdh0Jm6yF2NNRoJNQ4eBi1OdpJHzUI22I8XwMiotsmuofgd-O4UUiVNfL087SliDuKrJAZOlWId-F0oKTauyN2X6PWnn8lP5ifVb4wim5VPdL_QXd4apwbGXjY0auBFzo1a46T-HuQ' \
        ```

    * You can get the following payload from this endpoint:  

        ```json
        {
            "subscriptions": [
                {
                "subscriptionId": "7f55b3f0-e38c-4491-8573-72022a847251",
                "orgId": "adasdqe-wad-sdq-eqwewq",
                "orgName": "CHICAGO KEY ACCTS TEAM",
                "status": "active",
                "activationDate": "2021-12-10",
                "deactivationDate": ""
                },
                {
                "subscriptionId": "1105edc1-1b27-4f17-a2ce-2df4f207d068",
                "orgId": "fb4582ed-7147-4407-b3f6-1d805296cb9f",
                "orgName": "CHICAGO BMW TEAM",
                "status": "active",
                "activationDate": "2021-12-10",
                "deactivationDate": ""
                },
                {
                "subscriptionId": "51f30c8f-d977-49aa-ab52-0ff81b612afd",
                "orgId": "sdasd-12ads-12adsdas-dsad",
                "orgName": "CHICAGO MAJOR MRKT TEAM",
                "status": "active",
                "activationDate": "2021-12-10",
                "deactivationDate": ""
                },
                {
                "subscriptionId": "e51f0268-ce51-4f93-b433-88dcc07bf8b3",
                "orgId": "wqeqwe-122ada-asdad-dasds",
                "orgName": "CDK CHICAGO REGION",
                "status": "active",
                "activationDate": "2021-12-10",
                "deactivationDate": ""
                }
            ]
        }
        ```

* $[subscriptionCallbackUrl]/subscriptions/{subscriptionId}/context  

    |Parameter|Format|Type|Location|Value|Required|  
    |--|--|--|--|--|--|  
    |`Authorization`|String|Bearer|Header|Use [Getting a Token from the Token Server](/docs/tutorials/solution-integration/client-credentials-flow#getting-a-token-from-the-token-server) for information on getting your token.|Yes|  
    |`subscriptionId`|String|UUID|Path|Get the `subscriptionId` from the previous call for the `subscriptionId1` that you were interested in.|Yes|  
    * You get the following response when your send the endpoint a `subscriptionId` subscribed to your app:  

        ```json
        {
            "organizationInfo": {
                "id": "c8c2bbc3-d8be-42c7-a298-6b480bd820d1",
                "name": "Car Dealership",
                "address": "8601 Ranch Road 2222 Austin TX 78730 US",
                "countryCode": "US",
                "phoneNumber": "9402310425"
            },
            "appInfo": {
                "id": "6f415b3e-6acc-4946-813d-e53b0c89cb06",
                "name": "repair-order-app-v2",
                "developer": "CDK Global",
                "contactEmail": "support@fortellis.io"
            },
            "entityInfo": {
                "id": "c8c2bbc3-d8be-42c7-a298-6b480bd820d1",
                "name": "Car Dealership",
                "address": "8601 Ranch Road 2222 Austin TX 78730 US",
                "countryCode": "US",
                "phoneNumber": "9402310425"
            },
            "solutionInfo": {
                "id": "6f415b3e-6acc-4946-813d-e53b0c89cb06",
                "name": "repair-order-app-v2",
                "developer": "CDK Global",
                "contactEmail": "support@fortellis.io"
            }
        }
        ```

You get the following information from the payload:  

* Information on your `subscriptionId` organization.
    * `id`: You can use this to identify the organization within Fortellis.  
    * `name`: You can use this to find the name of the organization provided to Fortellis
        when the organization registered, usually the name of the dealership or dealer group.  
    * `address`: You can use this to find the name of the organization provided to Fortellis
        when the organization registered, usually the address of the dealership or dealer group.  
    * `countryCode`: You can use this to find the country code of the organization.  
        **Note:** Fortellis only supports the US and Canada for country codes at this time.  
    * `phoneNumber`: You can use this to call the organization.
        that subscribed to the app.  
* Information on your app.  
    * `id`: You can use this field to find the Fortellis `id` of your app.  
    * `name`: You can use this field to see the name of the organization in lower kebab-case.  
        **Example:** repair-order-app-v2
    * `developer`: You can use this field to see the name of your organization
        that developed the app.  
    * `contactEmail`: You can use this field to see the contact email of your organization.  
        **Note:** Follow the instructions in [Editing Your Contact Information](/docs/general/overview/managing-organizations#editing-your-contact-information) for information on how to do this.  

**Note:** Fortellis gives this information to API Developers that call the context endpoint to provide them with information about the app and subscription context at runtime.
See [Validating a Subscription-Id Without an Admin API](/docs/tutorials/admin-api/no-admin-api) for more information.  

With this endpoint, you can programmatically retrieve your subscriptions and use the `subscriptionId` to access subscriber data through Fortellis APIs.
