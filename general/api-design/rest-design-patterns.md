---
title: Introduction to REST
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can create usable APIs that App Developers can more easily adopt using these guidelines.
Your APIs can comply with REST design guidelines and Fortellis standards using this guidance.
With these guidelines, you can increase the usability and popularity of your APIs with App Developers.

## Responsibilities of APIs

You can do the following with your API:

* Gather data.
    * Gather data from different data sources including the following:
        * Databases
        * Legacy systems
        * Enterprise buses
    * Hide the backend systems that you are using.
* Structure and format data.
    * Make the data easy to consume.
* Deliver data.
    * Use appropriate delivery protocols to deliver the data securely with high performance.
* Secure and protect.
    * Use the API to secure and protect your backend systems and data so it's not stolen or compromised.

### Create Consumable APIs

For consumers to want them, APIs also need to have the following characteristics:

* They must focus on the consumer:
    * Target API consumers.
    * Suite the API consumers' needs.
* Consumers should see the API as the following:
    * Simple
        * Be easy to use and learn.
            * Use relevant standards and industry conventions that the consumers know.
        * Have a low barrier of entry for new API consumers.
    * Clean
        * Have a short number of parameters.
        * Use understandable names.
        * Follow a naming scheme.
    * Clear
        * Operate on only one object.
        * Do only one thing.
    * Approachable
        * Your API should be the following:
            * Explanatory
            * Intuitive
            * Predictable
            * Explorable
            * Discoverable
            * Well-documented
        * You should design your API with the following to make it approachable:
            * The URI needs to be predictable.
            * The parameters need to be self-explanatory.
            * The data objects need to be easy to understand.
    * Your API should do the following to be forgiving and forward compatible for API consumers:
        * Deliver error messages that the consumer can understand.
        * Hint for fixes to mistakes when the consumer makes mistakes.
        * Handle unexpected input in a graceful way.
        * Deliver useful results even when the parameters are slightly off.

### Protect Your Backend Systems for Your APIs

To secure your API, you can do the following:

* Ensure only authorized and authorized users can access your API.
* Ensure the API does not leak internal information.
* Ensure the API is compliant with the following:
    * Best practices
    * Security regulations

### Ensure Your API's Performance

You can ensure your API's performance by checking that the API does the following:

* Performs
* Scales
* Has high availability

### Reuse Components from Your Spec

You can build the API from reusable components to do the following:

* Make all APIs in a portfolio behave consistently.
* Reuse components across multiple APIs.
* Make it easier for consumers to use multiple APIs.

### Make Your API Specs Backward Compatible When Possible

Do the following to ensure your API is backward compatible:

* Support old clients.
* Create new versions for APIs that not backward compatible.  
    **Note:** See [API Versioning](/docs/tutorials/api-lifecycle/api-versioning) for more information on versioning.
* Do not change or remove a published API.

## Architectural Patterns

You can reuse several architectural patterns to solve common architectural challenges with APIs:

* Stateless server pattern
* Facade pattern
* Proxy pattern

## Stateless Server Pattern

The client can assume that the server does not retain any information about the state of the session.

The stateless server pattern provides the following:

* Scalability
* Availability
* Performance

### Facade Pattern

The API developer exposes a facade which hides the business logic working in the backend systems of the API.

You need to do two things to expose a facade:

* Design the best interface for your API consumers.
* Mediate between your consumers and your backend systems.

### Proxy Pattern

App Developers access APIs through Fortellis, which acts as a proxy to prevent the resource from being exposed directly.

## Architectural Styles

You can build the API faster and more consistently with a predefined architecture.

### REST Style

You can use the REST architectural style to follow constraints and make RESTful APIs that are optimal for HTTP requests.

#### HTTP

The World Wide Web Consortium and the Internet Engineering Task Force have standardized the Hypertext Transfer Protocol (HTTP).

#### REST Concepts

REST is based on HTTP concepts.

**Resource:** A unique resource that you identify by its URI

**API:** A root resource that contains other sub resources

**Representation:** The form of concrete data that you retrieve from a resource

**Uniform Resource Interface:** HTTP methods that you use with REST APIs to perform operations on resources

#### REST Constraints

* Use HTTP as much as possible.
* Design resources (nouns), not methods or operations (verbs).
* Use a uniform interface defined by HTTP methods.
* Use stateless communication between the client and server.
* Use loose coupling and independent requests.
* Use HTTP return codes.
* Use media-types.

##### Application state

* You use application state to track the interactions in a single instance of a client application.
* You can make sure that the application can scale easily and dynamically.
* The application must manage the state and pass the information along to the server with every request.

##### Anti-Pattern

You may not want to use token attributes to store application state
because you cannot see state changes stored in tokens.

#### Advantages of REST

REST APIs have several desirable properties:

* They are scalable.
* They tolerate faults.
* They improve the reliability and the availability of the complete system.
* Solutions built with REST APIs are more performant.
* You can handle multiple content-types with the negotiation between the client and the server.
* You can also get simpler and lighter weight APIs with REST.

#### HATEOAS Style

You can use REST and the additional constraints of Hypermedia As The Engine Of Application State (HATEOAS) to create self-documenting APIs:

* You return information on all the other endpoints with each API call to make the APIs self documenting.
* From the root resource, the client can discover all the other endpoints within the API.
* The client always navigates to the needed resource by following links.
* You can obtain all the information to make the next call from the response to the previous call with the HATEOAS links.
* The links are updated whenever the API is updated.
* You must link all resources to each other through hyperlinks in their responses.
* You must provide the semantics of the API responses by the media-types.

You can use [Roy Fielding's definition](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm) of HATEOAS to make your APIs more discoverable. The links define the following:

* Other methods that the endpoint supports.  
    * DELETE
    * GET
    * POST
* Possible next steps for using that endpoint.  

#### HATEOAS Example 1

1. A client makes a POST request, creating a new user:

   ```json
   POST $[apiFortellisUrl]/crm/v1/customer/users
   {
      "givenName": "James",
      "surname": "Greenwood",
      ...
   }
   ```

1. The API creates a new user from the input and returns the following links to the client in the response:

   ```json
   HTTP/1.1 201 CREATED
   Content-Type: application/json
   {
      ...
      "links": {
         "self": {
            "href": "$[apiFortellisUrl]/crm/v1/customer/users/ALT-JFWXHGUV7VI"
         },
         "delete": {
            "href": "$[apiFortellisUrl]/crm/v1/customer/users/ALT-JFWXHGUV7VI",
            "method": "DELETE"
         },
         "replace": {
            "href": "$[apiFortellisUrl]/crm/v1/customer/users/ALT-JFWXHGUV7VI",
            "method": "PUT"
         },
         "edit": {
            "href": "$[apiFortellisUrl]/crm/v1/customer/users/ALT-JFWXHGUV7VI",
            "method": "PATCH"
         }
      }
   }
   ```

1. A Fortellis API consumer can store these links in their database for later use.  
   **OR**  
    An admin can delete one of the users.

#### HATEOAS Example 2

1. The API consumer makes a GET request to the same fixed URI `/users`.

   ```text
   GET $[apiFortellisUrl]/crm/v1/customer/users
   ```

1. The API returns all of the users in the system with respective `self` links in the following response:

   ```json
   HTTP/1.1 200 OK
   Content-Type: application/json
   {
      "totalItems": "166",
      "totalPages": "83",
      "items": [
         {
            "givenName": "James",
            "surname": "Greenwood",
            ...
            "links": {
               "self": {
                  "href": "$[apiFortellisUrl]/crm/v1/customer/users/1"
               }
            }
         },
         {
            "givenName": "David",
            "surname": "Brown",
            ...
            "links": {
               "self": {
                  "href": "$[apiFortellisUrl]/crm/v1/customer/users/2"
               }
            }
         },
         ...
      ]
   }
   ```

1. The client retrieves the URI of the link relation type `delete` from the database.
1. The client deletes the user with the delete method on the user's URI.

   ```text
   GET $[apiFortellisUrl]/crm/v1/customer/users/ALT-JFWXHGUV7VI
   ```

1. The API returns an empty body with a 204 status code.

   ```json
   HTTP/1.1 204 OK
   Content-Type: application/json

   ```

## Spec Design

You can turn a good API into a great API with a great API frontend design.

### Resources

In REST, we recommend you do the following:

* Use the methods defined by HTTP for your resources.
* Design the representation of your resources.
* Identify resources by a Uniform Resource Identifier (URI).
* Use the following categories of resources:
    * **Collection resources:** These list other resources.
        * You usually have several of the same type of resources in a backend system.
            * Use a plural noun for the collection of the backend resource.
            * Use the same resource type for all sub resources that are part of a collection.
            * Use modifiers to perform operations on the collection:
                * Filter resources with specific properties.
                * Sort the resources.
                * Group similar resources.
                * Search for resources with specific properties.
            * Use the collection to create a new resource when you send a POST request:
                * Send the values for the resource as form parameters.
                * Send a 201 response code back.
                * Send the location of the new resource in the response back.
    * **Instance resources:** These represent a single business object.
        * Design the instance resource in the following way:
            * Get a collection of customers (resources) from `https://fortellis.io/customers`.
            * Get an instance of the resource by appending the customer ID to the URL: `https://fortellis.io/customers/4567`.
        * Remember the following when creating instance resources in the API:
            * Do not leak any information by exposing the key that you use in your backend system.
            * If you can easily guess keys for an instance and you need to protect the data, translate between the following:
                * A random instance resource used in the frontend.
                * The key on the backend.
    * **Controller resources:** These represent long running processes.
        * For batch functions and other such functions that run for long periods and need to run asynchronously, model the resource as a controller resource.
            * Controller resources return a response of `202 Accepted`.
            * You can use the status resource to track the status of the process.
                * Include the current status fields and the estimated execution time of the process.
                * Include a recommendation for polling in the status resource.
            * Return a `330 See Other` status with a URL in the location header when finished.
            * Return a `200 OK` response on failure with the failure response in the body.
            * Return a `200 OK` with an updated process resource when processing.
            * For an asynchronous delete, use a status code of `200 OK` for completion.

#### Hierarchies

We recommend you do the following in your REST APIs when you are creating hierarchies in REST APIs.

* Build resources in a hierarchy.
* Create resources as subordinates of other resources.
* Form a tree with the resource model.
* Express the ordering of the resources in the URL.

##### Root Resource

The root resource is at the top of the tree.

##### Sub Resource

* Create sub resources that are subordinate to other resources.
* Append the URI fragment to the URI of the root resource.

#### Resource Granularity

We recommend you figure out the following when creating the spec:

* The size of the resource.
* The number of fields in the resource.

Consider these perspectives when finding the answers to these questions:

* For the API Developer, you should optimize the reusability of the resource.
* For the App Developer, you should optimize the following:
    * Simplicity
    * Understandability
    * Usability
    * Performance

#### Resource Relations

Take an example of a customer and the messages that the customer receives, 787 and 895.

##### Resource Ordering

You can return the list of messages as a sub resource of the customer and return the actual message as a sub resource of messages.

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123
```

```json
{
    "name": "Barry"
}
```

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123/messages
```

```json
{
    "messages": [787, 895]
}
```

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123/messages/787
```

```json
{
    "from": "Barry",
    "to": "Denice",
    "body": "Thanks...",
    "time": "4:56 PM",
    "date": "4/8/2020"
}
```

You can use this structure to express hierarchical data.
You would probably find it difficult to express links between the messages of different customers.

##### IDs and Separate Root Resources

You can use the ID to correspond to a message of the respective customer.

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123
```

```json
{
    "name": "Barry",
    "messages": [787, 895]
}
```

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/messages/787
```

```json
{
    "from": "Barry",
    "to": "Denice",
    "body": "Thanks...",
    "time": "4:56 PM",
    "date": "4/8/2020"
}
```

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/messages/895
```

```json
{
    "from": "Denice",
    "to": "Barry",
    "body": "Yup...",
    "time": "5:05 PM",
    "date": "4/8/2020"
}
```

You may use this option if one of the following applies:

* You need to save bandwidth.
* The app and the API are tightly coupled.

##### Links and Separate Root Resources

You can also use HATEOAS to include the full link between the customer resource and the message resource.

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123
```

```json
{
    "name": "Barry",
    "messages": [
        {
            "href": "$[apiFortellisUrl]/customers/customer-rewards/v1/messages/787",
            "rel": "messages",
            "method": "GET",
            "media-type": "application/json"

        },
        {
            "href": "$[apiFortellisUrl]/customers/customer-rewards/v1/messages/895",
            "rel": "messages",
            "method": "GET",
            "media-type": "application/json"

        }
    ]
}
```

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/messages/787
```

```json
{
    "from": "Barry",
    "to": "Denice",
    "body": "Thanks...",
    "time": "4:56 PM",
    "date": "4/8/2020"
}
```

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/messages/895
```

```json
{
    "from": "Denice",
    "to": "Barry",
    "body": "Yup...",
    "time": "5:05 PM",
    "date": "4/8/2020"
}
```

The consumers do not need to know the following:

* Anything specific about the resources.
* Interpretations of the message ID.
* Construction of a URL for retrieving the message.

Use this option
if the App Developer and the API Developer should be loosely coupled and independent.
App Developers can follow the links to retrieve the messages.

##### Embedded Resources

You can merge all resources into a single resource with no separate resource for the messages.

```curl
GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123
```

```json
{
    "name": "Barry",
    "messages": [
        {
            "from": "Barry",
            "to": "Denice",
            "body": "Thanks...",
            "time": "4:56 PM",
            "date": "4/8/2020"
        },
        {
            "from": "Denice",
            "to": "Barry",
            "body": "Yup...",
            "time": "5:05 PM",
            "date": "4/8/2020"
        }
    ]
}
```

Remember the following about this method:

* You can only use this to express hierarchical relationships.
* You cannot model arbitrary graphs.
* You can get a lot of performance out of this.

##### Combinations

You will mostly likely combine these approaches to fit specific use cases for your App Developers.

#### Resource Links

To provide resource information, you can include the following when using links:

* The URL
* The method
* The meta-description
* The link target

You should include the following in links according to [RFC 5988](https://datatracker.ietf.org/doc/html/rfc5988):

* A target URL pointing to the target resource.
* A relation type providing the semantics or meaning of the relation.
* Optionally, a context URI identifying a frame or context in which this link is used.
* Optionally, one or several target attributes.

##### Example

```json
{
    "href": "GET $[apiFortellisUrl]/customers/customer-rewards/v1/customers/123",
    "rel": "customers",
    "method": "GET",
    "media-type":"application/json",
    "parameters":{
        "active": true
    }
}
```

#### Prohibited in Resource Design

* Do not use redundant data.
* Do not expose internal data.
* Do not expose composite resources.

### Uniform Resource Identifier

You can define the URI of the API in one of two ways:

* You can define it as a name, a Uniform Resource Name (URN).
* You can define it as a locator, a Uniform Resource Locator (URL).

#### URI Conventions

Use the following conventions to make the resources easier to parse:

* Use `/` to separate the hierarchies of the resources in the path.
* Use `-` to structure long resource names.
* Use `?` to separate the path from the first query parameter.
* Use `&` to separate the query parameters from each other.
* Use only lower case characters in the URI.
* Do not put file extensions in the URI.
* Use plural names for the resources in the URI.
* Do not have a slash, `/`, as the last character.
* Do not use blanks in the URI.

#### basePath Model

Fortellis will append the `basePath` to the URL: `{yourURL}/basePath`.
In OpenAPI 3, you include the `schema`, `host`, and `basePath` in the `servers` object.
Use the endpoint after your domain name for the `basePath`.

**Model:** `/business-domain/business-subdomain/your-product/version/resource?query`

##### URL Structure

Below you can find information on the URL components:

* Their order
* Example components
* A component diagram

**Note:** Please note the full URLs represent possible values and may not function.
Only use the major version of the API spec.

```text
'domain('/'sub-domain)/'3rd-party product name'/v'major-version'/'resource('/'resource)*'?'query
```

```text
/sales/deal-creation/best-dealer/v1/marked-down?name=test  
\____/\____________/\__________/\_/\__________/\________/  
|  |         |             |     |      |     |    |  
|  | sub-business domain   |  version   |     |  query  
|business domain           |         resource |  
|                 3rd-party product name      |  
\_____________________________________________/  
                        |  
                    base path  
```

#### Good URIs

We recommend the following for good URIs:

* Do not change the URI of a published API.
* Limit the nesting depth of the path to 2-3 levels.
* If your URI gets close to 2000 characters, redesign the API to use `POST` instead of `GET` to send information in the request body.
* If your the URL you receive is too long, return a status code `414 Request-URI Too Long`.
* Use a plural noun in the URL of a collection resource.

### Rules for Sending Representations

You can serialize resources to create a representation for the client:

* Serialize the representation in a JSON format.
* Include the rules for the serialization in the HTTP header.
    * Use the `Content-Type` header to specify `application/json`.
    * Use the `Accept` header to specify `application/json`.
* Use the `GET` method to send representations in the HTTP body of the response from the API.
* Only use JSON on the Fortellis Platform.  

### REST Status Codes

REST APIs return common HTTP status codes with a human-readable response.

#### Introduction to REST Status Codes

* Informational status codes begin with 1 and contain non-critical information.
* Success status codes begin with 2 and show that the request processed successfully.
* Redirect status codes begin with 3 and tell the client where it can go to find the resource.
* Client error status codes begin with 4 and tell the client that it is responsible for the error.
* Server error status codes begin with 5 and tell the client that the server is responsible for the error.

##### Redirect Status Codes

* You can handle redirection on the server side, making the redirection transparent to the client.
* You can give the client the new URL to make the request at.
    * Use `301 Moved Permanently` in the `Location` header to redirect permanently to the address.
    * Use `302 Moved Temporarily` in the `Location` header to redirect temporarily to the address.
    * Use `303 See Other` in the `Location` header to redirect to the address, changing the HTTP method to `GET`.
    * Use `307 Temporary Redirect` in the `Location` header to temporarily redirect to the address.

#### Error Codes

You can use status codes to indicate what the error was and who is responsible for fixing it.

You can include the following in the error message:

* Instructions on the following:
    * Getting more information
    * Fixing the error
* Locations with more documentation about the error.
* Time when the following occur:
    * The server is fixed.
    * The client can retry the request.

##### Error Message Guidelines

Try to do the following when creating your error messages:

* Include the error message in the response body with how to fix the error.
* Make the error message consistent across all your APIs.
* Include a link to more information on the error.

### Easy to Use APIs

The API consumer should be able to guess the following:

* URIs
* Parameter names
* Data fields

You can use the following to create easy-to-use APIs:

* Consistency
* Guidelines
* Conventions
* Standards

#### Consistent Names

Try to do the following to make your names consistent:

* Use camelCase JSON fields.
* Use lowercase fields for query parameters.
* Use Upper-Kebab case for standardized header parameters.

### Cross-Origin Resource Sharing

Browsers implement Cross-Origin Resource Sharing (CORS) to prevent cross-site scripting and cross-site request forgery.
The browser must send and receive the following responses to access an API:

1. The browser performs the pre-flight request with the `OPTIONS` method.
    * The pre-flight request describes the intended API call in the `Origin` request headers.
1. The API reads the `OPTIONS` method.
1. The API reads the `Origin` request header.
1. The API sends the `Access-Control-Allow-Origin` header in the response if it decides the API call is legitimate.
1. The browser checks the `Access-Control-Allow-Origin` headers.

### Forgiving APIs

Try to do the following to create forgiving APIs:

* Implement APIs with default values that are used when users do not supply or know the parameters for the API.
* Document the default values your APIs use when users do not supply a value.

### Discoverable APIs

You can use the `OPTIONS` method on resources so that endpoints are discoverable and self-documenting.

You can use two `OPTIONS` on an API endpoint:

* **Basic:** Return a status code of `204 No Content` with the HTTP header field `Allow` with a comma separated list of HTTP methods.
* **Advanced:** Return an API description in JSON.

## Pagination Controls for Query Operations

Pagination allows for reasonable response payload sizes when performing queries against collections.
You can include the following query parameters in methods that query collections:

### `page`

* This is a non-negative and non-zero integer
    that indicates which page of the query results should be returned.
* It must be optional.
* It must have a default value of 1.
* If a client provides an invalid value, such as a negative number,
    the server must respond with `400 Bad Request`.
* If the page value exceeds the total number of possible pages given the number of query results,
    the resource server must respond with a `200 OK` status code and an empty result set.

### `pageSize`

* This is a non-negative and non-zero integer of the maximum number of results in the response.
* It must be optional.
* It must have a default value.
* If a client provides an invalid value, such as a negative number,
    then the response must be a `400 Bad Request` status code.
* An API Developer may choose to limit the value to a reasonable maximum.  
    **Note:** This limits the ability to use pagination controls outside of their intended purpose.

## Read Resource Operation Response

When returning a resource, we recommend the API respond with the following schema:

* A resource identifier in the form `{resourceName}Id`.  
* An object `links` with `LinkDescriptionObject` properties describing actions given the response.  
    **Note:** Include the `self` relation in all resources.

For example, the following represents a query response for a single resource `Dog`:

```json
HTTP/1.1 200 OK
{
   "dogId": "1",
   "name": "Fido",
   "breed": "Labrador Retriever",
   "color": "Yellow",
   "links": {
      "self": {
         "href": "$[apiFortellisUrl]/v1/dogs/1",
         "method": "GET",
         "title": "Fido the dog"
      }
   }
}
```

## Query Collection Operations

When returning a collection of resources, the API can respond with the following schema:

* An array of resource representation `items` from the queried collection.
* An object `links` with `LinkDescriptionObject` properties describing actions given the response, such as `first`, `last`, `next`, and `prev` relations.
* The number of resources `totalItems` that match the query.
* The total number of pages `totalPages` that can be returned.

For example, you can see the response for a collection of `Dog` resources below:

```json
HTTP/1.1 200 OK
{
   "totalItems": 12,
   "totalPages": 4,
   "items": [
      {
         "dogId": 1,
         "name": "Fido",
         "breed": "Labrador Retriever",
         "color": "Yellow",
         "links": {
            "self": {
               "href": "$[apiFortellisUrl]/v1/dogs/1",
            }
         }
      },
      {
         "dogId": 2,
         "name": "Bowser",
         "breed": "Great Dane",
         "color": "Brown",
         "links": {
            "self": {
               "href": "$[apiFortellisUrl]/v1/dogs/2",
            }
         }
      },
      {
         "dogId": 3,
         "name": "Skip",
         "breed": "Corgi",
         "color": "Sable",
         "links": {
            "self": {
               "href": "$[apiFortellisUrl]/v1/dogs/3",
            }
         }
      }
   ],
   "links": {
      "first": {
         "href": "$[apiFortellisUrl]/v1/dogs&page=1;pageSize=3",
            "method": "GET",
            "title": "first page"
      },
      "last": {
         "href": "$[apiFortellisUrl]/v1/dogs&page=4;pageSize=3",
         "method": "GET",
         "title": "last page"
      },
      "next": {
         "href": "$[apiFortellisUrl]/v1/dogs&page=2;pageSize=3",
         "method": "GET",
         "title": "next page"
      },
      "prev": {
         "href": "$[apiFortellisUrl]/v1/dogs&page=1;pageSize=3",
         "method": "GET",
         "title": "previous page"
      },
   }
}
```

## Delete Resource Operations

When responding to a request to delete a single resource,
the API can return a response of `204` and an empty body.

## Error Responses

To make the API easier for users to understand,
you can use standard error response schema for your RESTful operations:

| Property | Type   | Required? | Description |
|----------|--------|-----------|-------------|
| code     | string | no        | An optional error code further identifying the error|
| message  | string | yes       | A description of the error |
| link     | string | no        | An optional URL to more information about the error |

The following example `ResourceNotFoundError` returns a `400 Bad Request` HTTP status code:

```json
{
  "code": "ResourceNotFoundError",
  "message": "Unable to find given resource ID",
  "link": "https://example.com/errors/ResourceNotFound"
}
```

You can do the following to provide descriptive messages to help users recover from the error:

* Identify any custom error codes in your documentation and make sure to explain them in detail.
* Include human readable messages that summarize the following:
    * Context
    * Cause
    * Solution if possible  
* Create a unique `Request-Id` in the spec that the app sends and the API returns with the response.  
   **Note:** You must provide a unique `Request-Id` for all requests sent through Fortellis.
        Include the `Request-Id` as a required parameter in all specs at all endpoints.
* Include the `https` status code in the body as well as the header.
* Send the text of the status code so your API consumers have it.
* Send the title of the error to inform the consumer what caused the error.
* Include the following keys in the error responses:
    * Detail
    * Type
    * Title
    * Status
    * Instance
* Provide links to help pages in your responses if you can.
* Provide additional details on how to fix the problem if you have a good understanding of what probably happened.
