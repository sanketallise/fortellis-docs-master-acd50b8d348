---
title: API Data Schemas
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can follow these additional Fortellis guidelines to track your requests and subscriptions across the platform.

With the Fortellis spec guidelines, you can onboard App Developers familiar with other Fortellis conventions onto your API more quickly.

## Resource Identifiers

The Fortellis APIs conform to the following conventions:

* API specs follow [RFC 4122](https://tools.ietf.org/html/rfc4122) standards for universally unique identifiers.
* Resource identifiers use ASCII characters.
* Query parameters and resource identifiers must use the URI-percent-encoding [RFC 3986](http://tools.ietf.org/html/rfc3986) standard for syntax except URI unreserved.

## Use of Standard HTTP Headers

Adhere to the following guidance on HTTP headers:

* [RFC 7231](https://tools.ietf.org/html/rfc7231)
* [RFC 7232](https://tools.ietf.org/html/rfc7232)
* [IANA Header Registry](https://www.iana.org/assignments/message-headers/message-headers.xhtml)

Use the following HTTP request headers consistently when authoring specs.

### Request Headers

Method requests must include these outlined HTTP request headers.

#### Authorization

Include the `Authorization` header in the API spec. The `Authorization` header specifies the credentials of the client.

**Note:** Use Fortellis Authorization.
Fortellis uses your selected token to contact your API,
but accepts Fortellis OAuth 2.0 tokens from the App Developers to authorize requests.

```text
Authorization: Bearer <credentials>
```

#### Request-Id

Include a 16-byte `Request-Id` header in all calls in the API spec to trace requests across system boundaries. The `Request-Id` header specifies the globally unique identifier for the request.

```text
Request-Id: <16 byte unique_identifier>
```

#### Subscription-Id

Include the `Subscription-Id` header in every request to correlate an Organization with an App. For the connection to work, the Subscription ID must be *active* for the Organization.

```text
Subscription-Id: <identifier>
```

### Response Headers

Your method responses should include the following HTTP response header.

#### Request-Id

Include the `Request-Id` from the request in the response. The `Request-Id` header specifies the unique ID of the request, and the caller can use the `Request-Id` to trace the request on their end.

```text
Request-Id: <unique-identifier>
```
