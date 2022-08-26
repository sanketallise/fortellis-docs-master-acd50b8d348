---
title: Admin API Authorization
author: Nathaniel Sandberg
last updated: 3/25/2022
---

If you implement the Admin API, you can protect the endpoint of your Admin API with the use of OAuth tokens.

Fortellis makes calls to the Admin API's `/activate` and `/deactivate` endpoints and Fortellis provides a token in these request that you can verify. The requests contain the following information:

* Audience: `api_providers`
* Issuer: `$[authServerUrl]`
* Subject: `{yourClientID}`

Go to [https://jwt.io](https://jwt.io/libraries) to find libraries
to assist you with validating and verifying tokens.

Remember the following:

* You only receive the token at the `/activate` and `/deactivate` endpoints if you have implemented the Admin API.
* You then follow the steps in [Client Credentials Flow](/docs/tutorials/solution-integration/client-credentials-flow) to get a token to make a call to the `/connections/:connectionId/callback` endpoint.

Always verify the issuer of the token and the audience for the token at each of these endpoints.
You can also verify the subject in the token if your **Client ID** starting September 27, 2022.
