---
title: Asynchronous API Overview
author: Nathaniel Sandberg
last updated: 4/27/2022
---

## Executive Summary

With an Event-Driven Architecture, you get many capabilities and benefits
that you don't get with a traditional, synchronous API architecture.
Developers, Managers, and Product Managers can benefit from AsyncAPIs to create apps and services
that do the following:

* Decrease network bandwidth by avoiding polling with synchronous APIs.
* Implement and leverage loosely coupled systems that use events and messaging architecture.
* Synchronize the backend and frontend systems in real time.

With synchronous APIs, the app must poll the API to get updated data.
With the Fortellis asynchronous API service provided by the *Event Relay*, API Developers can create asynchronous API sources
that App Developers can subscribe to through Fortellis.

### Fortellis Event Relay

The Event Relay is the Fortellis system that does the following:

* Requests permission on behalf of Subscribers and apps to access your asynchronous API source.
* Provides the asynchronous API source with an authorization method for the app.
* Routes the asynchronous API source call to webhooks and conceals the source.
* Provides API responses without straining the network with requests and responses.
* Saves your engineers and developers time
    when they are coding
    because they don't have to develop proxies and verification methods.

The Event Relay can give you the flexibility and speed
when you are developing asynchronous APIs and apps.
With Event Relay, you can provide great asynchronous API sources and webhook apps
that avoid the polling problems of APIs.

### Using Fortellis for Your Asynchronous APIs

Fortellis provides the following in the Event Relay to help you more easily create and adopt asynchronous APIs:

* A service layer to not expose asynchronous sources directly and hide source URLs.
* A UI to register asynchronous API topics.
* A linter to check that your AsyncAPI specifications meet the [AsyncAPI](https://www.asyncapi.com) standards.
* A platform where users can subscribe to your asynchronous API source.
* A router that can send asynchronous API calls out to multiple Subscribers at once.
* An exponential backoff strategy that continues to attempt to deliver messages.

### Creating and Updating Sources and Apps

You can find the following in the Event Relay tutorials to help you implement and integrate with Event Relay:

* Overview
    * [Executive Summary](#executive-summary)
    * [Benefits](#benefits)
    * [Use Cases](#use-cases)
* Features
    * [Conceptual Overview of Event-Driven Application and Systems](/docs/tutorials/event-relay/features)
* Getting Started
    * Asynchronous API Developer Tutorials
        * [Creating Your AsyncAPI Spec](/docs/tutorials/event-relay/tutorial-design-an-async-api)
        * [Developing an Asynchronous API in Node.js](/docs/tutorials/event-relay/tutorial-develop-an-async-api)
        * [Developing an Asynchronous API in Python](/docs/tutorials/event-relay/publishing-messages-and-verifying-results-python)
        * [Publishing Messages and Verifying Results in Node.js](/docs/tutorials/event-relay/publishing-messages-and-verifying-results)
        * [Publishing Messages and Verifying Results in Python](/docs/tutorials/event-relay/publishing-messages-and-verifying-results-python)
    * App Developer Tutorials
        * [Understanding Asynchronous API Producers for App Developers](/docs/tutorials/understanding-asyncapis)
        * [Integrating Your App and an Asynchronous API Producer in Node.js](/docs/tutorials/event-relay/tutorial-integrate-an-async-api-integration-with-an-app)
        * [Integrating Your App and an Asynchronous API Producer in Python](/docs/tutorials/event-relay/tutorial-integrate-an-async-api-integration-with-an-app-python)
        * [Building an HTTP Callback to Receive Messages in Node.js](/docs/tutorials/event-relay/tutorial-building-an-http-callback-to-receive-messages)
        * [Building an HTTP Callback to Receive Messages in Python](/docs/tutorials/event-relay/tutorial-building-an-http-callback-to-receive-messages-python)
* References
    * [Reference Architecture](/docs/tutorials/event-relay/reference-architecture)

Go to the Getting Started docs to start creating or consuming asynchronous APIs,
or continue reading the [Benefits](#benefits) and [Use Cases](#use-cases) for more information on what you can do with asynchronous APIs.

Fortellis provides the tooling for the following to stay up-to-date with current trends in the software industry:

* Asynchronous API Developers
* App Developers
* Dealers
* OEMs
* Car Manufactures

As part of the larger CDK Global portfolio, Fortellis can support your asynchronous APIs, apps, APIs, and subscriptions for as long as you need them,
and integrate with your other CDK Global services.

## Benefits

You get the following benefits from asynchronous APIs:

* You prevent polling.
* You synchronize loosely coupled systems.
* You save network bandwidth.
* App Developers reduce application logic.
* Subscribers save money.
* You segregate the following:
    * Resources
    * Components
    * Systems
    * Responsibilities
* You can send updates to multiple Subscribers and systems at the same time.
* You can send and receive responses in real time.
* You can scale apps and asynchronous APIs.

## Use Cases

What kinds of problems can you solve with Fortellis Event Relay?

### Near Real-Time Notifications

You can build workflows that react to events in near real time rather than having to poll constantly and check for changes. The Fortellis Event Relay will push events to a webhook via HTTP that you register for your Fortellis Application.  

### Pub/Sub Broadcasting

You can build an asynchronous API that can broadcast events to multiple subscribers. When your API publishes an event, the Fortellis Event Relay will deliver that event to all registered subscribers.

### Data Replication and Synchronization

Replicate and synchronize data between systems. The Fortellis Event Relay can be used as a reliable event delivery mechanism between two or more systems to ensure that changes are delivered to all Subscribers.  

### Generate Insights from Changes Over Time

By receiving events when a person, place, or thing changes over time, you can capture insights into its behavior that would not be possible from a record representing its current state. You can offer this rich stream of data through the Fortellis Event Relay to developers building analytics.
