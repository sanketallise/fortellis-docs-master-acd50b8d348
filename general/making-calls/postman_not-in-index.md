---
title: Testing with Postman
author: Kelly Rich
last updated: 9/30/2021
---

Important! This is a WIP - DO NOT REVIEW yet.

The [Postman](https://www.postman.com/) application lets you run requests in an environment that is customized for testing APIs. Postman supports multiple authorization mechanisms and you can run JavaScript commands before and after requests, enabling you to string requests together. Postman *Collections* are groups of related requests, and some APIs include Postman Collections as a way to jump-start your development process.

## Creating a Postman Collection

This section contains a quick tutorial that creates a Collection and adds a single GET request to the CDK Drive Customer API.

For this exercise, you will use Postman to make a call to the CDKDrive Customer API.

Note: Your final solution will use a more sophisticated authorization flow. See *Authorization on Fortellis* for more information.

## Configuring Postman Building Blocks

Create a Postman collection and request.

Creating a Postman Collection

1. Open Postman.
1. Select the New menu, and then select Collection.
1. In the name field, enter Fortellis Repair Orders API Test.
1. Select the Authorization tab.
1. Set the type to Basic Auth.
1. Enter your API key into the username field.
1. Enter your API secret into the password field.
1. Select Create.

## Creating a Request

1. Select the New menu, and then select Request.
1. In the request name field, enter Create Repair Order.
1. Select the collection you created earlier.
1. Select Save to Fortellis Repair Orders API Test.

## Building Your Request

Note: This procedure uses the request body example from the Repair Orders API Create Repair Order endpoint at Repair Orders API Reference.

1. Open the request you created earlier.
1. Set the method to POST.
1. Enter the request URL: `$[apiFortellisUrl]/cdkdrive/service/v1/repair-orders/`.
1. Select the Headers tab.
1. Add a new key for Subscription-Id with the value `test`.
1. Select the Body tab.
1. Select raw.
1. Paste the following request body into the field.

``` json
{
    "appointmentId": "8888",
    "blockIVRFlag": false,
    "comments": "Use synthetic 5W-20 engine oil",
    "customerHref": "$[apiFortellisUrl]/cdkdrive/crm/v1/customers/800245",
    "vehicleHref": "$[apiFortellisUrl]/cdkdrive/service/vehicles/4G263088",
    "customerContactInfo": "8302852330",
    "dropOff": {
        "dateTime": "2018-08-12T20:13:45",
        "address": "678 Montgomery St",
        "city": "Jersey City",
        "note": "Call before pickup",
        "phone": "8302852330",
        "vanNumber": "8302852330"
    },
    "estimate": {
        "authorized": 200,
        "labor": 80,
        "parts": 40,
        "misc": 30,
        "lube": 30,
        "sublet": 10,
        "tax": 10
    },
    "mileageIn": {
        "value": 87665,
        "units": "MI"
    },
    "pickUp": {
        "dateTime": "2018-08-12T20:19:45",
        "address": "678 Montgomery St",
        "city": "Jersey City",
        "note": "Call before pickup",
        "phone": "8302852330",
        "vanNumber": "8302852330"
    },
    "promiseDateTime": "2018-08-12T20:17:45",
    "serviceAdvisorId": "6164",
    "tagNum": "3088",
    "remarks": "sample remarks",
    "transportType": "DROPOFF",
    "serviceLineItems": [
        {
            "cause": "Braking issue",
            "comebackFlag": false,
            "dispatchCode": "S104",
            "estimatedDuration": 1.5,
            "laborEstimate": 10,
            "laborType": "CEC",
            "lubeEstimate": 10,
            "miscEstimate": 10,
            "partsEstimate": 10,
            "serviceEstimate": 10,
            "status": {
                "code": "I93",
                "description": "PREASSIGNED"
            },
            "serviceRequest": "CUSTOMER STATES THERE IS A CLUNKING SOUND IN FRONT END OVER BUMPS",
            "subletEstimate": 10,
            "taxEstimate": 10,
            "technicianId": "253",
            "laborOperations": [
                {
                    "forceShopChargeFlag": false,
                    "laborType": "CEC",
                    "opCode": "SSTR",
                    "opCodeDesc": "SUSPENSION AND STEERING REPAIR",
                    "soldHours": 0.2,
                    "saleAmount": 7,
                    "saleOverrideFlag": false,
                    "includedParts": [
                        {
                            "fixedSaleFlag": false,
                            "includesCoreFlag": false,
                            "mfrCode": "FR",
                            "partNumber": "P9488",
                            "partNumberDescription": "CLUTCH PLATE",
                            "partQty": 1,
                            "saleAmount": 30
                        },
                        {
                            "fixedSaleFlag": false,
                            "includesCoreFlag": false,
                            "mfrCode": "FR",
                            "partNumber": "7488",
                            "partNumberDescription": "BRAKEPAD",
                            "partQty": 2,
                            "saleAmount": 50
                        }
                    ]
                }
            ]
        }
    ]
}
```

1. Save your request.

## Sending Your Request

In the request you created earlier, select Send. Your request is sent to Fortellis test environment and if you formatted your request properly, it returns a response as defined by the spec.

## Evaluating Your Request

After you send your request, you should see your response body in the lower pane. The response should look like this:

```json
{
    "repairOrderId": "5656",
    "status": {
        "code": "I93",
        "description": "PREASSIGNED"
    },
    "links": {
        "self": {
            "href": "$[apiFortellisUrl]/cdkdrive/service/v1/repair-orders/123"
        }
    },
    "blockIVRFlag": false,
    "comments": "Use synthetic 5W-20 engine oil",
    "customerHref": "$[apiFortellisUrl]/cdkdrive/crm/v1/customers/800245",
    "customerContactInfo": "8302852330",
    "vehicleHref": "$[apiFortellisUrl]/cdkdrive/service/vehicles/4G263088",
    "dispatchMakeCode": "C",
    "estimate": {
        "estimateOverrideFlag": false,
        "authorized": 200,
        "labor": 80,
        "parts": 40,
        "misc": 30,
        "lube": 30,
        "sublet": 10,
        "tax": 10
    },
    "mileageIn": {
        "value": 87665,
        "units": "MI"
    },
    "poNumber": "5334",
    "promiseDateTime": "2018-08-12T20:17:45",
    "remarks": "sample remarks",
    "serviceAdvisorId": "6164",
    "tagNum": "3088",
    "transportType": "WAITER",
    "serviceLineItems": [
        {
            "serviceLineId": "A111",
            "cause": "Braking issue",
            "comebackFlag": false,
            "dispatchCode": "S104",
            "estimatedDuration": 1.5,
            "laborEstimate": 10,
            "laborType": "CEC",
            "lubeEstimate": 10,
            "miscEstimate": 10,
            "partsEstimate": 10,
            "serviceEstimate": 10,
            "status": {
                "code": "I93",
                "description": "PREASSIGNED"
            },
            "serviceRequest": "CUSTOMER STATES THERE IS A CLUNKING SOUND IN FRONT END OVER BUMPS",
            "subletEstimate": 10,
            "taxEstimate": 10,
            "technicianId": "253",
            "laborOperations": [
                {
                    "forceShopChargeFlag": false,
                    "laborType": "CEC",
                    "opCode": "SSTR",
                    "opCodeDesc": "SUSPENSION AND STEERING REPAIR",
                    "soldHours": 0.2,
                    "saleAmount": 7,
                    "saleOverrideFlag": false,
                    "includedParts": [
                        {
                            "partRefId": "1",
                            "fixedSaleFlag": false,
                            "includesCoreFlag": false,
                            "mfrCode": "FR",
                            "partNumber": "P9488",
                            "partNumberDescription": "CLUTCH PLATE",
                            "partQty": 1,
                            "saleAmount": 30
                        },
                        {
                            "partRefId": "2",
                            "fixedSaleFlag": false,
                            "includesCoreFlag": false,
                            "mfrCode": "FR",
                            "partNumber": "7488",
                            "partNumberDescription": "BRAKEPAD",
                            "partQty": 2,
                            "saleAmount": 50
                        }
                    ]
                }
            ]
        }
    ]
}
```

Remember that Fortellis API test endpoints send prefilled sample data as provided in the API Spec. The response will not necessarily correspond to the values from your request.

If you received an error, use the table below for help troubleshooting.

```javascript
Error Possible Cause  Solution
The request could not be satisfied  Incorrect method  Make sure your method is set to POST.
Cannot POST {someURL}  Invalid request URL (endpoint)  Confirm your request URL is correct.
Authorization header required  Misconfiguration of authorization  Make sure your authorization type is set to Basic Auth.
Solution has not requested access to this API. Please select the API from your Developer account.  Invalid API key in the username field or solution is not connected the this API.  Confirm your solution is connected the CDKDrive Customer API and that you have the correct API key from that solution.
Unauthorized  Invalid API secret in the password field  Confirm you have the correct API secret from a solution that is connected to the CDKDrive Customer API.
Subscription-Id header required  Subscription-Id is missing or there is a typo in the key  Confirm you have a header parameter with the key Subscription-Id.
401 Unauthorized  Invalid Subscription ID  Confirm the value for your Subscription ID header parameter is test.
Unexpected token {someCharacter} in JSON at position {someNumber}  Your JSON payload is malformed or has errors  Confirm you copied the code sample correctly. If you have trouble copying from this topic, you can get the same sample request from the API reference page.
```

Note: Because of how the API test endpoints work, you can send a request without a JSON payload in the body and still receive a successful response.

!~!

## Getting Your Test Collection

1. Sign in to [Fortellis]($[devNetworkUrl]).
1. Do the following to select the organization that the API belongs to:
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
    1. Click the organization that the API belongs to.
    1. Click **OK**.
1. Next to **APIs**, click **View All** if available.  
    **Note:** You can now see that the API only has two statuses:  
    * **Provisioning**  
    * **Ready**
1. Click the API.
1. Click the environment where you want to test your API.
1. Click the menu button next to the version.
1. Click **Postman Collection Test**.  
    **Note:** Remember the following about your test collection:  
    * The collection mocks an app that uses your **Client ID** and **Client Secret**.
    * The collection uses these to create tokens for you.
    * Fortellis attributes any actions taken using these tokens to you.

### Importing Your Test Collection into Postman

1. In Postman, click **Import**.  
1. Click **Upload Files**.
1. Click the `{your API name}.postman_collection.json` file.  
    **Note:** You can find this file in the **Downloads** folder by default.  
1. Click **Open**.  
    **Note:** You may want to rename the collection to accommodate multiple test collections.
1. Click **Import**.

You may receive a warning that you have a duplicate collection due to a different environment.
If that happens, click **Import as a Copy** and rename the collection to associate it with the environment and version.

## Testing Your API

1. In Postman, click the **Fortellis Sample Application** > **Fortellis Sample Application** folder.
1. Click **Generate Bearer token**.  
    **Note:** When you import the Postman collection, you should have the following values autopopulated:
    * `client_id`: The collection uses your Client ID assigned in the UI for your username.
    * `client_secret`: The collection uses your Client Secret assigned in the UI for your password.
    * `Subscription-Id`: The collection uses your `Subscription-Id` assigned by Fortellis to make calls through the platform.  
    **Note:** You should use the `Subscription-Id` to recognize the app activations at runtime if you use the Admin API.
1. Click **Send**.  
    **Note:** You get a token in the response. The token populates the authorization in the other calls to the API.

    ```bash
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiIwb2E0cm01eXg0UzhhdU82dzJwNyIsImp0aSI6IkFULkhNRnVSbm5xTW0zRlBkcnBEYTZUYklLZG05NmlPdmdGTGtiZTdRUFZyaW8iLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IjBvYTRybTV5eDRTOGF1TzZ3MnA3IiwiaWF0IjoxNTczMjQxOTcyLCJleHAiOjE1NzMyNDU1NzIsImF1ZCI6ImZvcnRlbGxpcyIsImlzcyI6Imh0dHBzOi8vdmVyaWZ5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyIn0.oqQEU__SB_NJ4X2MxtRiGRo6M-shjGtpqDBSUwAD6O0-EzwU7mrAXegFRDTS9TCCIahN4xAuC3zWACmKtzcuykgn8rkPmFEGd7WY9CyfEL11Q3q2rG--rguTiRWSJDCGqFVpEGdGKxssxDpS2diXtmwGxg8CI7P-RPCwNmZw8wk95-uL1CZHdqnG0TYCvhQin_zK2i26z4nBKBIQv5LZI2yoE9Y7GWPhlRmRAoTcumfG_MeqNn5Td-zHlBvSmhqE6KZcZHkENwpVwF9P2WabGMmkKmENoGRYfiMnk51SrriA9SAI5jL_vjuBz3CYjugOZpv6grttKia70sSuyzgmwQ",
        "scope": "anonymous"
    }
    ```

1. Use the collection to query any of your API endpoints.  

Remember the following about the token:

* You must refresh your token every hour.
* Once your token has expired, you will receive a 403 error from the API.
