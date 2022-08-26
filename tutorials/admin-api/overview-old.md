---
title: Admin API Overview
author: Nathaniel Sandberg
last updated: 12/11/2019
---

With the [Provider Admin API Specification](/docs/tutorials/admin-api/admin-api-specs), you can perform the following tasks:

* Screen users
* Create a UI for subscription review

The [Provider Admin API Specification](/docs/tutorials/admin-api/admin-api-specs) contains the following:

* The endpoint that Fortellis sends the activation request to
* The activation request payload that Fortellis sends to you  
    **Note**: The activation request includes all the information on the subscriber and the subscriber's organization. You should store this information for future reference and associate it with that subscriber.
* The activation response that you send to Fortellis
* The endpoint and the path parameter that Fortellis sends the deactivation request to
* The deactivation request payload that Fortellis sends to you
* The deactivation response payload that you send to Fortellis

With the [Fortellis Connections Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback), you can perform the following:

* Activate subscription requests  
    **Note:** Fortellis does not complete the connection until you activate the request.
* Create a UI for activating subscription request

The [Fortellis Connections Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback) contains the following:

* The authorization server that you must get a token from to send the response
* The connection response you send to Fortellis to activate the connection
* The endpoint and path parameter you send the response back to
* The header parameters you send in your response back

With these two APIs, you can perform the following:

* Systematically fulfill requests
* Provide a great experience for your users
* Decrease the time to accept connections
* Provide the correct information
* Simplify the process for exposing your APIs to users
* Ensure only authorized subscribers can use your APIs
* Vet subscribers to secure your APIs

To see steps in the process, see [Implementing the BasePath](/docs/tutorials/admin-api/creating-the-base-path).

You can copy the information from the spec and paste it into the [Swagger Editor](https://editor.swagger.io) to see what JSON payloads you can expect to receive and send with the Admin API and Connection Callback API. See our full [example Admin APIs](https://github.com/Fortellis/admin-api-example) on GitHub.
