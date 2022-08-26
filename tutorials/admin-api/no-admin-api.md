---
title: Validating a Subscription-Id Without an Admin API
author: Nathaniel Sandberg
last updated: 4/27/2021
---

While most API Developers implement the Admin API to manage user subscriptions to their API,
some providers may choose to not implement the Admin API.

Fortellis does not recommend you create an API without doing one of the following:

* Implementing the Admin API
    * You can automatically leverage the information that Fortellis sends in connection request to approve and store subscribers.  
    **Or**  
* Using the manual approval that Fortellis provides in the UI  
    **Note:** See [API Connection Management](/docs/tutorials/api-lifecycle/api-connection-management).
    * You must manually manage your subscriptions with this option through the UI.

API Developers should consider the following if they don't leverage the Admin API:

* The service should retrieve the context of the user subscription at runtime from Fortellis.
* The service should cache any needed subscriber information to reduce requests to the subscriber context endpoint.

## Performance

Without an Admin API, your apps should do the following:

* Experience decreased performance resulting from the overhead of doing a subscription look-up for each request made to your API.
* Cache as much subscriber-related information as possible to lower the performance hit related to data retrievals.

## Capturing the Subscription-Id

If you do not use the Admin API, you have to do the following:

* Capture the `Subscription-Id` from the header of the request.
* Use the subscriber context API to get information about the subscription.

### Using the Subscription-Id

1. Call the authorization endpoint to get an access token. See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) for more information.  
  **Note:** Your access token expires in one hour. We recommend that you programmatically renew the token before an hour so that you always have the token to retrieve the subscriber context.
1. Use the `Subscription-Id` to get the context of the subscription with the endpoint below.
1. Use the information from the context endpoint to fulfill any requests.

## Calling the Subscription Context Endpoint

When your application receives a request, you can use the `Subscription-Id` included in the request header and the access token generated in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) to get information about that subscription.

### Request

``` bash
  curl -X GET \
  $[subscriptionCallbackUrl]/subscriptions/<subscriptionId>/context \
  -H 'Authorization: Bearer <access_token>' \
  -H 'Content-Type: application/json'
```

### Response

``` json
{
"entityInfo": {
    "id": "Organization Id",
    "name": "Organization name",
    "address": "Organization address",
    "countryCode": "US",
    "phoneNumber": "Organization phone #"
},
"solutionInfo": {
    "id": "solution ID",
    "name": "solution display name",
    "developer": "solution owner",
    "contactEmail": "solution contact email"
}
```
