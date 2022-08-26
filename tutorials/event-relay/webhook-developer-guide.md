---
title: Webhook Developer Guide
author: Nathaniel Sandberg
last updated: 3/18/2022
---

To provide our App Developers with a more familiar flow, we created this guide to get started developing apps with asynchronous APIs.
You must use slightly different strategies
when you are developing for asynchronous APIs
because you can do the following with REST API specs:

* Create a stub from the spec.
* Use the spec to get examples.
* Create mocks using the spec.

However, with an [AsyncAPI](https://www.asyncapi.com) spec, you have fewer options for creating stubs.
To help you create mock events, use the simulator for asynchronous APIs at the end of this tutorial that you can use to send events locally to your app while you develop.
With an understanding of the proper headers, you can create an app that securely receives events from asynchronous APIs.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

1. Verify the token and all the required headers.  
1. Verify that the event matches the schema in the asynchronous API spec.  
    **Note:** You can create a schema to validate the event with the example in the asynchronous API spec.  
    > Store the event for processing after you have verified it.
1. Send the correct response back.  

You can use the simulator at the end of this guide and the event payload from the spec to develop your asynchronous API consumer.

### Things to Remember in This Tutorial

You should remember the following:

* Use the following to start the server:  

    ```curl
    node index.js
    ```

* Use **Ctrl**+**C** to stop the server.
* Install the following before starting the tutorial:
    * Node
    * NPM

## Verifying the Headers in the Event

You receive the following headers in the event when you receive an event from the Fortellis Event Relay:

* `Data-Owner-Id`: `{yourDataOwnerId}`
* `X-Request-Id`: `{UUID}`
* `Authorization`: `Bearer {yourToken}`
* `Content-Type`: `application/json`
* `Source-Owner-Id`: `{informationOwner}`
* `Origin-Request-Id`: `{orginRequestId}`
* `deliveredTimestamp`: `{timeStampOfDeliveryToFortellis}`

### Headers

Verify the event has all required headers.

#### Origin-Request-Id

Fortellis uses the `Origin-Request-Id` to track the request across systems and sends a corresponding `eventId` to the API Developer.
You can reference the `Origin-Request-Id` for troubleshooting and tracking individual events.
If the source doesn't include a `Origin-Request-Id`, Fortellis may automatically generate it.

#### Data-Owner-Id

Events use the organization ID to identify the dealer who owns the data.
Asynchronous API Developers must identify the dealer they are publishing the event for with the `Data-Owner-Id`.
Use this to deliver the data to the correct subscriber.
Not all events to your app webhook go to all Subscribers.  

#### Fortellis Token

You receive a Fortellis token that Fortellis created with your API key and secret.
Check for the following in the token match your **API Key**:

* The subject, `sub`.
* The client ID, `cid`.

Only you and Fortellis can create Bearer tokens using these keys.
With them, you can verify that the token is from Fortellis and the event is for you.
You should also verify the following in the token:

* The token is not expired.  
* The token was issued before the current time.  
* The token has the correct issuer.  
* The token has the correct audience `api_providers`.  
* The token has a secret key encrypted with the JSON Web Key Set.  

#### Content Type

For the `Content-Type`, Fortellis sends `application/json` to your webhook endpoint.

#### X-Request-Id

Fortellis sends this header to track the event from the event producer through Fortellis to the event consumer.
Use the same `X-Request-Id` when returning the response to Fortellis.

#### deliveredTimestamp

Use this to determine when the asynchronous API published the event.

## Verifying the Event

You get the payload of the event from the body.
You should verify that the event is in the correct format.
Use the example event from the asynchronous API to create a schema to validate the event with.

## Sending Responses

Include the `X-Request-Id` in all responses for Fortellis to correlate the response to the event.
You can respond to the Event Relay with the following status codes to show how you processed the event.

### 202

Use the `202` response to indicate you have received the event for processing.
Do not include a response body if you send this response.

### 400

Use the `400` response to indicate that the event was malformed or missing information, such as a header.

Example Value

```json
{
  "code": 400,
  "message": "Bad Request"
}
```

### 401

Use the `401` code to indicate that the event was unauthorized. You usually see this error when the token has expired.

Example Value

```json
{
  "code": 401,
  "message": "Unauthorized"
}
```

### 403

Use the `403` code to indicate that the action is forbidden. You usually see for one of the following reasons:

* The token is not valid.
* The event did not include a token.

Example Value

```json
{
  "code": 403,
  "message": "Forbidden"
}
```

### 404

Use the `404` code to indicate that you couldn't find the resource.
You may not have included the `/event` endpoint at the end of your webhook
if you see this in production.

Example Value

```json
{
  "code": 404,
  "message": "Not Found"
}
```

### 500

Use the `500` code to indicate that the server had an internal error of some sort.
You may need to update your service to have better error handling or address an exception.

```json
{
  "code": 500,
  "message": "Internal Server Error"
}
```

### 503

Use this status code to indicate that the server is unavailable.
You may need to start or restart your server if you receive this error.

Example Value

```json
{
  "code": 503,
  "message": "Service Unavailable"
}
```

## Next Steps

In this tutorial, you should have learned how to do the following:

1. Verify the token and all the required headers.  
1. Verify that the event matches the schema in the asynchronous API spec.  
    **Note:** You can create a schema to validate the event with the example in the asynchronous API spec.  
    > Store the event for processing after you have verified it.
1. Send the correct response back.  

You can now download an asynchronous API simulator and use the asynchronous API spec to send example data to develop your app.

### Preparing the Code

1. Download the project from <https://github.com/Fortellis/Simulation-Async-API>.  
1. Change directories to Simulation-Async-API.  
    On the command line, type the following:  

    ```bash
    cd Simulation-Async-API
    ```

1. [Registering-apps](/docs/tutorials/app-lifecycle/registering-apps) your app on Fortellis.
1. Get your **API Key** and **API Secret**.
1. [Base 64 encode](https://www.base64encode.org) your `APIKey:APISecret`.
1. In the `index.js` file, replace the following:  
    * `base64Encoded{yourAPI:yourSecret}` with the new value.  
    * `{yourLocalHostOrDevelopmentServer}` with your local development or your development server URL with the `/event` endpoint.  
    * `{yourDataOwnerID}` with the organization ID subscribed to your app.
        **Note:** You can mock this if you choose in development.
1. Install the dependencies.  
    On the command line, type the following:  

    ```bash
    npm install
    ```

### Sending Mock Requests

1. Start the server with **node index.js**.  
1. Replace the contents of the `payload.json` file with the contents that you see in the example of the asynchronous API spec.  

You can now start developing your asynchronous API consumer locally with mock requests.
Go to the next tutorial, [Integrating Your App and an Asynchronous API Producer](/docs/tutorials/event-relay/tutorial-integrate-an-async-api-integration-with-an-app), to see how to start integrating your app with an asynchronous API in Node.
