---
title: Understanding Asynchronous API Producers for App Developers
author: Nathaniel Sandberg
last updated: 4/27/2022
---

## Benefits

With Asynchronous APIs, you can constantly update your apps with the latest information.
You get the following benefits from asynchronous API producers:

* You do not have to constantly poll an API to get updated data.
* You can subscribe to an asynchronous API to synchronize your app with another system.
* You can subscribe to an asynchronous API to save network bandwidth for your app.
* You can synchronize loosely-coupled systems.
* You can reduce the app logic
    because you don't have to compare the old response to a new response.
* You can save money
    because you don't have to make multiple requests to poll the asynchronous API.
* You can more quickly narrow down a bug or system issue with loosely-coupled systems.
* You can keep your system updated with the most recent changes from the events that you choose.
* You can more easily scale your apps
    because you do not need to keep running threads to poll APIs.
* You receive events with an exponential retry that continues to attempt to deliver the message if the event fails with an error.

Asynchronous APIs deliver the most updated information to you in real time
so you don't have to constantly poll an API to get refreshed data.

## Mocking Asynchronous API Producers

To develop integrations with asynchronous APIs, you may want to create a mockup of the asynchronous API
when you develop the asynchronous API consumer.
Follow the tutorial in [Webhook Developer Guide](/docs/tutorials/event-relay/webhook-developer-guide) and customize the code to create a mock up of the asynchronous API
that you want to consume.
