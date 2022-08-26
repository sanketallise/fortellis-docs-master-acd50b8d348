---
title: When to Use Event Relay
author: Nathaniel Sandberg
last updated: 9/7/2021
---

## Asynchronous API Developers

API Developers may choose to create asynchronous APIs for the following reasons:

* To complement their synchronous APIs and mirror the functionality.
* To lower the following on backend systems:
    * Network bandwidth
    * Latency
    * Traffic
* To remove delays caused by synchronous APIs.
* To increase performance with parallel programming.
* To update the client apps with a long-running backend process when the backend process has finished.

## App Developers

You may need to use asynchronous API producers to get information in real time.
For example, you may need to receive real-time information in the following situations:

* Stock prices.
* Train times.
* Updates to inventory.
* Acceptance of concurrent requests.
* Limitations on the threads you can use or have available.

App Developers usually design the app to respond quickly to an API response. However, APIs may have increased latency due to the following:

* The app hosting stack.
* The security components.
* The relative geographic location of the caller and the backend.
* The network infrastructure.
* The current load.
* The size of the request payload.
* The processing queue length.
* The time for the backend to process the request.

You should use asynchronous APIs to develop your apps for the following reasons:

* Remove delays caused by synchronous APIs.
* Increase performance with parallel programming.
* Avoid wait times for the server.
* Keep multiple connections open to continue multiple requests.
* Provide better performance in the app.
