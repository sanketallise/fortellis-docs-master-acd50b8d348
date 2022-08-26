---
title: Key Concepts
author: Nathaniel Sandberg, Daniel New 
last updated: 4/27/2022
---

![Event Relay Logical Architecture]($[docsUrl]/static/images/event-relay/fortellis-event-relay-logical-arch.jpg)

*Event Relay* is an asynchronous message broker that provides a reliable, scalable, and secure mechanism for building event-driven systems.

## Event-Driven Communication

*Event-driven communication*, or event-driven architecture, is a pattern where *Publishers* and *Subscribers* communicate asynchronously through the exchange of *messages*.

## Publishers

*Publishers* are a service that generates messages which it submits to a *message broker* for delivery.

## Message Broker

A message broker is a shared service does the following:

* Handles the receipt of messages from publishers.
* Queues those messages in a *message topic*.
* Delivers those messages to Subscribers.

Brokers do the following:

* Guarantees the order in which messages are delivered.
* Reliably deliver the message to a subscriber.
* Secures who has access to publish and subscribe to the messages.

## Message Topic

**Note:** Asynchronous API Developers must send a `partitionKey` in the asynchronous API header for the Event Relay to send the events in order.
If the Asynchronous API Developer does not provide a `partitionKey` in the header,
Fortellis does not preserve the order of the messages to guarantee in order delivery.
Fortellis sends each message with a randomly generated `partitionKey` and does not preserve their order.

A *message topic* is the ordered sequence of messages received from a publisher. Each Publisher is allocated a topic where messages can be reliably stored while awaiting delivery to Subscribers.

## Subscribers

Subscribers register to receive messages from event publishers they choose as an integration between the two services. This integration describes where messages should be delivered by the broker. Event Relay delivers messages via *webhooks* registered by the Subscriber.

## Webhooks

Webhooks are a web service endpoint that can receive messages by an HTTP request.
