---
title: Implementing the Endpoint
author: Nathaniel Sandberg
last updated: 1/13/2021
---

You can receive requests and monetize your API with Fortellis.
You must implement the endpoint on your URL and register the API to receive requests through Fortellis.
Fortellis proxies the request and helps secure your API.

## Implementing the Endpoint

You implement the `endpoint` that you create in the spec.
Fortellis routes API requests to your URL plus the `endpoint`
when app users call the API at `$[apiFortellisUrl]/{yourNamespace}/{yourEndpoint}`.

> For more information on creating specs, see the [APIs and API Specifications](/docs/general/api-developers/api-specs) and the [Creating Specs](/docs/tutorials/spec-writing/creating-api-specs) sections.

1. Create the spec that you would like to implement.  
1. Set up the API to receive calls according to the spec at your URL + the endpoints for that API.  
    Take the following example:
    * Your URL is `http://example.com`.
    * You create a spec with an `/endpoint`.
    * You would enter `http://example.com` for the API when registering.
    * Fortellis will append the `/endpoint` and route calls sent to `$[apiFortellisUrl]/{yourNamespace}/{yourEndpoint}` to `{yourURL}/{yourEndpoint}` for calls made through your app.  
    **Note:** Fortellis checks the API for a **200** response,
    but does not perform any other checks on this endpoint.  

    ```http
    http://example.com/{yourEndpoint}
    ```

1. After activation, receive calls routed through Fortellis from the app to your API.  
    **Note:** If you have not implemented the Admin API or have not chosen to manually accept requests,
    Fortellis automatically connects all requests to your endpoint.
