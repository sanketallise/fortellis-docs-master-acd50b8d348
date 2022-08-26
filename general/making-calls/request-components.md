---
title: API Request and Response Components
author: Kelly Rich
last updated: 10/05/2021
---

The APIs hosted on Fortellis are all RESTful APIs where you use HTTP protocols to make requests to the API services. With REST APIs, you structure requests to service methods using the following components:

* **Endpoint** – Each API method uses an endpoint comprised of an *HTTP method* and a *URL* to the resource being acted upon.
* **HTTP headers** – All methods require you provide a set of HTTP headers with a request. While many are standard, the API Spec defines any special headers used by each method.
* **Request body** – Also known as a "request payload," format the request body in JSON when one is required by the method you're calling.

## The API Spec

An *API Spec* accompanies each published API. The API Spec acts as the contract between the API publisher and the API consumer (known on Fortellis as the *API Developer* and *App Developer*, respectively), and it provides the request and response details of each method supported by the API.

For each method, the API specification defines the following:

* Endpoint
* HTTP request and response headers
* Request and response bodies
* Returned HTTP status codes
* Possible error codes

## HTTP Methods

Apps often use the following HTTP methods when sending requests to APIs.

|Method | Description |
|--|--|
| GET | Requests data from a resource |
| POST | Submits data to a resource |
| DELETE| Deletes a resource |

### Method Endpoints

Endpoints consist of the [HTTP method](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) plus the URL to the resource being acted upon. You can think of the method as the *verb* defining the action you want to take and the URL is the resource you're addressing with your request.

The URL is composed of several elements, including the API *host*, the *Base Path*, and the *resource path*. The resource path itself consists of several elements, including the API version, namespace, and resource name. For more on API URLs, see the section [Uniform Resource Identifier](/docs/general/api-design/restful-api-design/#uniform-resource-identifier).

## HTTP Headers

All REST requests require you to provide a set of *HTTP headers* with the request. While API implementations differ on the headers required to make a call, Fortellis APIs all require you to provide the following set of HTTP headers in each request you make:

| HTTP Header | Description |
| ----------- | ----------- |
| **Subscription-Id** | The Subscription ID assigned to the solution you are developing on the Fortellis platform. Sign in to your Developer Network account to retrieve this value from your app’s Activation Report. |
| **Authorization** | This header provides the authorization needed to make requests to the API.    Call the Fortellis OAuth token server using the client credentials flow to create an OAuth 2.0 access token. Set the value of this header to the word “Bearer” followed by a space and a valid access token. |
| **Request-Id**** | A client-supplied string value that is unique for each request. This value is returned in the response so you can correlate the request to the response.    Note that the Fortellis front end supplies this value to the API service if the value is not supplied by the client. |

Some Fortellis APIs, and methods within an API, require or use additional headers than the ones outlined in the table above. Review the API Spec or *Developer Guide* for details on the headers used by each API method.

For more on the **Authorization** header, see [Authorizing API Requests](/docs/general/making-calls/authorizing-requests).

## Request and Response Bodies

Not all API methods require a request body. For example, many GET methods require just the endpoint and headers. However, when required, format the request body in JSON as outlined in the request schema detailed for each method in the API documentation.

Fortellis APIs return all response payloads formatted in JSON.

## HTTP Status Codes

In general, all Fortellis APIs return standard [HTTP response status codes](https://datatracker.ietf.org/doc/html/rfc7231#section-6). The actual HTTP codes and messages returned for each method are detailed in the associated API Spec.

## API Error Codes

See the API Spec for a list of possible error codes and error messages returned by each method. While many APIs return a common error message structure, error message structures vary. Again, check the API specification to ensure you're aligned on the return structure.

It can be helpful to review the possible errors returned a service. Error messages give a glimpse of how the API works with clues on how to avoid error conditions.
