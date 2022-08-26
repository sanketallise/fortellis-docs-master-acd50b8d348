---
title: Key Features
author: Nathaniel Sandberg, Daniel New 
last updated: 4/27/2022
---

## Event-Driven Publish and Subscribe Messaging

Publishers submit messages via a REST API to Event Relay. Subscribers register a webhook where Event Relay can deliver those messages via HTTP.

## Reliable, In-Order Delivery

Event Relay reliably delivers messages to subscribers and automatically retries delivery if a subscriber Application is unavailable. Messages arrive in the order they are sent.

**Note:** Event Relay cannot guarantee the messages are in order
    if the event does not include the `partitionKey` in the request.
    If you rely on in order messaging,
    ensure the event include the `partitionKey` in the event headers.

> Remember the following caveats:
>
> * If the message does fail with a `partitionKey` for that webhook,
> the webhook receives no other messages
> until a retry of the failed message succeeds.
> * The webhook may continue to receive events from events with a different `partitionKey` in the header.
> * If the Asynchronous API Developer does not include the `partitionKey` in the request,
> the Event Relay cannot guarantee the messages arrive in order.
> Ensure the Asynchronous API Developer includes the `partitionKey` in the event to get the messages in order.
> To see if the Asynchronous API Developer includes the `partitionKey` in the header, check the spec for the header in the message.

### Example

```yaml
...
header:
    type: object
    properties:
        partitionKey:
            description: The Asyncrhonous API Developers sends the `partitionKey` in the header to ensure in order delivery of messages through the Event Relay
            type: string
...
```

## Secured End-to-End

Event Relay secures access for publishers and subscribers using the OAuth2 protocol. Data exchanged between publishers and subscribers is controlled with the consent of the data owner.

## Standards Driven

Publish your event producer using the industry standard [AsyncAPI](https://www.asyncapi.com) specification language.

## Distributed Specs

Fortellis distributes asynchronous API specs through the [API Directory]($[apiReferenceUrl]).
App Developers can use the spec to create apps that consume your asynchronous API. Asynchronous API Developers can also use the tags in the spec, and users can filter by those categories in the [API Directory]($[apiReferenceUrl]).

**Note:** You can only use approved Fortellis tags at this time:

* **CDK Partner Program**
* **Communications**
* **Credit**
* **CRM**
* **Customers**
* **Data**
* **Documents**
* **Insurance**
* **Media**
* **Parts Management**
* **Payments**
* **Transportation**
* **Vehicles**

You must request any additional categories that you want in the API Directory from [support@fortellis.io](mailto:support@fortellis.io).
