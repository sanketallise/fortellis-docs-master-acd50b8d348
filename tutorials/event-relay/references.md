---
title: Asynchronous API Reference Architecture
author: Nathaniel Sandberg, Daniel New
last updated: 10/7/2021
---

## Frequently Asked Questions

### What Is the Difference Between a Synchronous API and an Asynchronous API?

**Synchronous APIs:** In synchronous APIs, the caller sends the request and then waits for the response to that request.  
**Asynchronous APIs:** In asynchronous APIs, the server has updated information and sends the payload to the app in response to a change on the server's end.  

### Are REST APIs Synchronous or Asynchronous?  

Asynchronous APIs generally follow the same conventions as their synchronous counterparts for the format of the payload. APIs can send messages synchronously or asynchronously and still conform to REST architecture.

### What Is Publishing Asynchronous APIs?

When you publish an asynchronous API,
you create an asynchronous API on your server and send events through Fortellis.

### What Is a Webhook?

Asynchronous APIs send events to Fortellis
when the asynchronous API system updates.
Fortellis forwards the events to webhooks.
You must make the endpoint publicly available to receive asynchronous API calls through Fortellis.

### How Do I Protect My Webhook Endpoint?

Verify Fortellis tokens to authorize requests to your webhook.

### What Is the Difference Between an API and a Webhook?

Asynchronous APIs send the data payload. Webhooks receive the payload and process the data.

### How Can I Create a Webhook?

Use the programming language of your choice. Currently, you can create an endpoint locally to receive asynchronous API calls using the following tutorials:  

* [Integrating Your App and an Asynchronous API Producer in Node.js](/docs/tutorials/event-relay/tutorial-integrate-an-async-api-integration-with-an-app).
* [Integrating Your App and an Asynchronous API Producer in Python](/docs/tutorials/event-relay/tutorial-integrate-an-async-api-integration-with-an-app-python).

You must then deploy the endpoint and ensure the URL is public to receive asynchronous API calls through Fortellis.
We have more tutorials for other programming languages in progress.

### How Does a Webhook Work?

A webhook receives updates from the asynchronous API.
Instead of the app polling the API for updated information, the API notifies the webhook of any updates.

### What Are Good Use Cases for Webhooks?

See [Use Cases](/docs/tutorials/event-relay/overview/#use-cases) for a list of use cases for webhooks.

## Event Relay Limits and Quotas

Event Relay sets quotas on resources and behaviors for integrations with publishers and subscribers.  The following quotas apply for subscribers:

| Resource/Function                                       | Quota/Limit                             |
|---------------------------------------------------------|-----------------------------------------|
| Undelivered message retention period                    | Unlimited Retries                       |
| Retry policy for failed message delivery to webhooks    | Retry delivery until webhook returns success|
| Retry frequency for failed message delivery to webhooks | Retry based on an initial pause of one second and a back-off multiplier of 2|
|Retry Max Delay|5 minutes|

## Event Relay API References

[Data Plane API Spec (OpenAPI)](https://github.com/Fortellis/Event-Relay-Specs/blob/master/async-api-data-plane-proxy-v1.yaml)
