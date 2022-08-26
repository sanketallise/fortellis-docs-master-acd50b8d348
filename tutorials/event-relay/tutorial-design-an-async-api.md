---
title: Designing an Asynchronous API
author: Nathaniel Sandberg
last updated: 1/20/2022
---

The project [AsyncAPI](https://www.asyncapi.com) seeks to create a specification for asynchronous API producers to standardize software development for Asynchronous API Developers and provide organizations and individuals with an effective way to document their asynchronous API producers.

The AsyncAPI spec is the following:

* Open-source.
* Focused on the current state of Event-Driven Architectures.

AsyncAPI project works to make Event-Driven Architectures as simple as RESTful APIs with the following:

* Documentation
* Code generation
* Discovery
* Event Management

Asynchronous API producers require specs first that allow the following roles to define the interface:

* Developers
* Architects
* Product Managers

Fortellis posts your asynchronous API spec in the [API Directory]($[apiReferenceUrl]).
App Developers use the asynchronous API spec to do the following:

* Understand the payload sent to apps.
* Create apps that consume that payload.
* Use the examples in the spec to create the user interface.
* Understand how to authenticate an event from Event Relay.

You can use the AsyncAPI spec to do the following:

* Automate your documentation or code.
* Formalize your documentation or code.
* Establish solid standards for your events.
* Improve the governance of your asynchronous APIs.

In REST APIs, an app requests some information, and the server sends the response back.

Asynchronous API producers are different in the following ways:

* The API sends the response to the app when the event happens, not when the app calls it.
* The API may not need much, if any, information from the app.
    * In some cases, the asynchronous API producers can fire and forget,
        and the app resolves any conflicts later.  
        **Note:** You can only use the option to fire and forget
        if you do not include the `partitionKey` in your asynchronous API header.

Other than structural differences, you should know the following about the AsyncAPI spec:

* The AsyncAPI spec is compatible with OpenAPI schemas.
* The message payload of an asynchronous API producer can be any value, not just an AsyncAPI or OpenAPI schema.
* The server object is almost identical to its OpenAPI counterpart with two exceptions:
    * The `scheme` is named `protocol`.
    * AsyncAPI introduces a new property called `protocolVersion`.
* OpenAPI path parameters and AsyncAPI spec channel parameters differ because AsyncAPI doesn't have a query or a cookie.

You should use the AsyncAPI spec
because you are going to have multiple teams working across multiple distributed systems.
Those teams need to see the changes
that each one makes
so that they can collaborate with each other and adjust to any changes
that other teams make.

Use one of the following to create your AsyncAPI spec:

* `YAML`
* `JSON`

You can use the following to help you create your AsyncAPI spec:

* [AsyncAPI Preview](https://marketplace.visualstudio.com/items?itemName=ivangsa.asyncapi-preview)
* [AsyncAPI Editor](http://editor.asyncapi.org)

Use [AsyncAPI](https://www.asyncapi.com) to guide you through making your AsyncAPI spec.  

## Fortellis Specific Rules for AsyncAPI Specs

You must use the AsyncAPI spec and Fortellis specific conventions in the following sections
when you submit a spec.

### Servers

* Name your servers in the format {"dev"|"uat"|"prod"}-{"http"}, for example `prod-http`.
* Use the following format for your URL: `$[eventRelay]`.
* Use `http` for the `protocol`.
* Use the following Fortellis `securityScheme`.

    ```yaml
    fortellisOAuth2:
      type: oauth2
      description: OAuth2 authorization using Fortellis
      flows:
        clientCredentials:
          tokenUrl: $[authServerUrl]/v1/token
          scopes: {}
    ```

### Channels

* You must define one or more channels in your AsyncAPI spec.
* You can only use the subscribe property.  
    **Note:** Subscribers cannot publish to your asynchronous API producers through Fortellis at this time.

### Channel Items

* You must use `http` for the bindings of the channel.
* You must use the `application/json` for the `contentType`.

### Components

* You must use a security component in the specification with the following:

    * A `type` of `oauth2`.
    * A `clientCredentials` flow.
    * A `tokenUrl` of `$[authServerUrl]/v1/token`.  
    **Note:** You should use a `securityScheme` like the following:

    ```yaml
    fortellisOAuth2:
      type: oauth2
      description: OAuth2 authorization using Fortellis
      flows:
        clientCredentials:
          tokenUrl: $[authServerUrl]/v1/token
          scopes: {}
    ```

### Messages

You may use the following in your headers:  

* `X-Request-Id`: Use this header to track the event across systems boundaries from the asynchronous API to the event relay to the asynchronous API consumer.  
* `Authorization`: Use this to authenticate the event with Fortellis and tell Fortellis which channel the event corresponds to.  
* `Data-Owner-Id`: Use this header to identify the organization that owns the data that you can send events to.  
    **Note:** Use the `organizationId` of the organization subscribed to the app for the `Data-Owner-Id` to ensure the organization accesses information it has permission to.
* `partitionKey`: Use this header to create a partition.  
    **Note:** Fortellis delivers all events with that `partitionKey` in order and does not continue to send other events to that webhook until the first event receives a success response.  

    ```yaml
    headers:
      type: object
      properties:
        X-Request-Id:
          description: The unique request ID for the event going through Fortellis to track the event across systems
          type: string
        Data-Owner-Id:
          description: The data owner for the event the Asyncrhonousa API Developer is sending
          type: string
        partitionKey:
          description: The unique identifier for the partition for these events that must be delivered in order
          type: string
    ```

## Example AsyncAPI Spec

```yaml
asyncapi: 2.0.0
id: 'https://helloWorldExample.com'
tags:
  - name: Credit
info:
  title: Hellow World Example
  version: '1.0.0'
  description: This is the asynchronous API description so that someone could say hello to the world.
    contact:
    name: Asynchronous API Support
    url: https://fortellis.io/contact-us
    email: support@fortellis.io
servers:
  prod-http:
    url: $[eventRelay]/nathaniel-prod
    protocol: http
    security:
    - fortellisOAuth2: []
channels:
  hello/world:
    subscribe:
      summary: This is how you say hello to the world.
      bindings:
        http:
          type: request
          method: POST
      message:
        $ref: "#/components/messages/HelloWorld"
components:
  messages:
    HelloWorld:
      headers:
        type: object
        properties:
          X-Request-Id:
            description: The unique request ID for the event going through Fortellis to track the event across systems
            type: string
          Data-Owner-Id:
            description: The data owner for the event the Asyncrhonousa API Developer is sending
            type: string
          partitionKey:
            description: The unique identifier for the partition for these events that must be delivered in order
            type: string
      payload:
        type: object
        properties:
          id:
            type: string
            description: This is the id of your hello world.
          number:
            description: The number of times you\'ve said hello.
            type: integer
            minimum: 0
            maximum: 20
          haveYouSaidHello:
            type: boolean
            description: This records whether you have said hello world.
          waysToSayHello:
            type: array
            items:
              type: string
          helloID:
            type: object
            properties:
              language:
                type: string
                description: You said hello in this language.
              id:
                type: string
                description: You said hello in a language with this id.
        contentType: application/json
        example:
          id: 6620800b-7029-4a7a-8e80-cdd852fc01c8
          number: 4
          haveYouSaidHello: true
          waysToSayHello:
          - Hello
          - Hola
          - Hallo
          - Bonjour
          - Olá
          helloID:
            language: English
            id: b2f7494a-5dcf-448c-9585-5301fc0dd231
  securitySchemes:
    fortellisOAuth2:
      type: oauth2
      description: OAuth2 authorization using Fortellis
      flows:
        clientCredentials:
          tokenUrl: $[authServerUrl]/v1/token
          scopes: {}
```

## Creating an AsyncAPI Spec in Fortellis

### What You Will Learn in This Tutorial

In this tutorial, you will learn the following:

* The parts of an AsyncAPI spec on Fortellis.
* The format of the AsyncAPI spec on Fortellis.
* The required fields in the AsyncAPI spec.
* The format for different payloads you can send with your asynchronous API producer.

For this tutorial, we are creating a simple "Hello World!" asynchronous API producer.
You will get the following in this tutorial:

* The Fortellis Asynchronous API Security component.
* The server name you should use.
* The URL that you use for asynchronous API specs.
* The convention for naming your servers.
* The format for components to comply with Fortellis standards.

### Things to Remember During the Tutorial

Please refer to the example above for reference.
We refer to it throughout the tutorial.
Also, ensure you use two spaces for your indents and keep the indents between the fields consistent
if you are copying and pasting.

### Getting Started on the File

1. Start by opening a text editor.  
    In this tutorial, we use **Visual Studio Code**.
1. Use an extension.  
    In this tutorial, we use the **Visual Studio Code** extension [AsyncAPI Preview](https://marketplace.visualstudio.com/items?itemName=ivangsa.asyncapi-preview).
1. Create a new `JSON` or `YAML` file for your AsyncAPI spec.  
    In this tutorial, we name the file `HelloWorld.yaml`.
1. To use the plugin, press **Ctrl**+**Shift**+**P**.  

### Entering the Required Fields

1. Enter the version of the AsyncAPI spec.  
    On Fortellis, you can use any version of the AsyncAPI.
1. Enter the `id` of the spec.  
    Use the URL for the API as the unique identifier for the AsyncAPI spec.  
    **Note:** For this example, we use `https://helloWorldExample.com`.  
        You must have a unique `id` for your spec and enclose the `id` in quotes to upload the spec to Fortellis.
1. Enter the tags below that so API Directory users can easily filter categories to find your API.  

    ```yaml
    tags:
      - name: Credit
    ```

1. Enter the `info` below that.  
    Below `info`, indent with two spaces and create the following three fields followed by a colon on separate lines:  
    * `title`: Use the title to identify the name of the application.  
        You must use a unique title for your AsyncAPI spec.
    * `version`: Use the version field to identify the version of the application API.  
    * `description`: Use the description field to describe the spec and the asynchronous API that you can make using the spec.  
    **Note:** Use quotes around the version so the parser processes it as a string.  
    * `contact`: Use the contact object to include the information that people can use to contact you.
        * `name`: Give the name of the person to contact if your customers need to contact someone.
        * `url`: Give the URL that the customers can use to get more information from your support staff.
        * `email`: Give the email that your customers can use to contact your support staff.

    ```yaml
    info:
      title: Hellow World Example
      version: '1.0.0'
      description: This is the description that you give for the asynchronous API and what it can do.
      contact:
        name: Asynchronous API Support
        url: https://fortellis.io/contact-us
        email: support@fortellis.io
    ```

1. Without any indent, enter the `servers` below that.  
    The `servers` object includes the following:
    * Name your servers in the format {"dev"|"uat"|"prod"}-{"http"}, for example `prod-http`.  
        Include the following in the name.
        * `url`  
        **Note:** You must use the following URL on Fortellis when submitting AsyncAPI specs: `$[eventRelay]`.
        * `protocol`  
        **Note:** You must use the `http` protocol.

        ```yaml
        servers:
          prod-http:
            url: $[eventRelay]
            protocol: http
            security:
            - fortellisOAuth2: []
        ```

### Creating Channels

1. Below `servers` without any indentation, type the following:

    ```yaml
    channels:
    ```

    Producer applications send messages to channels, and consumer applications consume messages from channels, such as the following:
    * `user/signedup`
    * `light/measured`  
1. To create a channel beneath `channels:`, indent with two spaces and enter the channel and a colon.  
    **Note:** For this tutorial, we use the channel `hello/world`.
1. Beneath each channel, indent with two more spaces and type the following to create the `subscribe` object:  

    ```yaml
    subscribe:
    ```

1. Indent with two spaces below `subscribe` and include the following in the subscribe object with a colon after each:
    * `summary`: Use the summary to describe what the channel does.
    * `bindings`: Use the binding to define the protocol and the method used on the channel.
    * `message`: Use the message to define the payload of the channel.  
1. Enter the summary of your channel.
1. For the `bindings`, enter the following:

    ```yaml
    bindings:
      http:
        type: request
        method: POST
    ```

1. Beneath each message, indent with two spaces, type the following and enter the `name` of the message:

    ```yaml
    name:
    ```

    **Note:** Use one word in PascalCase.  
    For this tutorial, we use the name `HelloWorld`.

    ```yaml
    channels:
    hello/world:
      subscribe:
        summary: This is the hello world AsyncAPI spec.
        bindings:
          http:
            type: request
            method: POST
        message:
          name: HelloWorld
    ```

### Creating the Headers

You create headers for each of your messages. You must use the following fields:  

* `X-Request-Id`: Use this to track the event across systems.  
* `Data-Owner-Id`: Use this header to indicate the organization that owns the data the event sends.
    This matches the `organizationId` of the organization subscribed to the app.

You can use the following optional headers:  

* `partitionKey`: Use this header to guarantee the events are delivered in order to the event consumers.  
    **Note:** Fortellis creates a random partition key and sends the events in any order if you do not include the `partitionKey` parameter in your header.
        The Event Relay does not send the `partitionKey` to the apps, but uses it to determine the apps with the Subscriber that has permission to access the event.

To create the header, do the following:  

1. Beneath the name, indent with two spaces and type the following:  

  ```yaml
  headers:
  ```

1. Beneath the payload with two more spaces, type the following:  

  ```yaml
  type: object
  properties:
  ```

  **Note:** You are going to use the object to include each one of the string headers you are going to include in the request.  

1. Below properties, type two spaces and each of the headers.  
    **Note:** You may choose not to use the `partitionKey` if the app does not need events to be delivered in order.

    ```yaml
    X-Request-Id:
    Data-Owner-Id:
    partitionKey:
    ```

1. Include the information for each of the properties.  
   Below each property, complete the `description` and the `type`.

  ```yaml
  headers:
    type: object
    properties:
      X-Request-Id:
        description: The unique request ID for the event going through Fortellis to track the event across systems
        type: string
      Data-Owner-Id:
        description: The data owner for the event the Asyncrhonousa API Developer is sending
        type: string
      partitionKey:
        description: The unique identifier for the partition for these events that must be delivered in order
        type: string
  ```

### Creating the Payload

1. Beneath the headers, type the following:

    ```yaml
    payload:
    ```

    **Note:** You are going to use this for the final payload.
1. Beneath the payload with two more spaces, type the following:

    ```yaml
    type:
    ```

1. Enter one of the following to select the data type:  
    * A string
    * A number
    * An Object
    * An array
    * A boolean
    > For this tutorial, we are going to use an `object`.
1. Use `object` for the type.
1. Beneath the `type`, type the following to create the properties:

    ```yaml
    properties:
    ```

    **Note:** You will define the fields in the payload in the properties.
1. Beneath the `properties`, indent with two spaces and enter the name of the key in the key-value pair of the property.  
    For example, if you are expecting `"id": "someValue"`, you type the following:

    ```yaml
    id:
    ```

1. Beneath the name with two more spaces, enter the description of the JSON value.  
    **Note:** You can use the following reference to see more about data types and how to define the description: [Data Types](https://swagger.io/docs/specification/data-models/data-types). For this tutorial, we use the following payload:

    ```yaml
    payload:
      type: object
      properties:
        id:
          type: string
          description: This is the id of your hello world.
        number:
          description: The number of times you\'ve said hello.
          type: integer
          minimum: 0
          maximum: 20
        haveYouSaidHello:
          type: boolean
          description: This records whether you have said hello world.
        waysToSayHello:
          type: array
          items:
            type: string
        helloID:
            type: object
            properties:
              language:
                type: string
                description: You said hello in this language.
              id:
                type: string
                description: You said hello in a language with this id.
    ```

### Adding Security to Your AsyncAPI Spec

**Note:** You must have Fortellis OAuth security for your AsyncAPI spec.

1. At the bottom without any indent, type the following:

    ```yaml
    components:
    ```

1. Under components, indent two spaces and type the following:

    ```yaml
    securitySchemes:
    ```

1. Under `securitySchemes`, indent with two spaces and use the following Fortellis security scheme:

    ```yaml
    fortellisOAuth2:
      type: oauth2
      description: OAuth2 authorization using Fortellis
      flows:
        clientCredentials:
          tokenUrl: $[authServerUrl]/v1/token
          scopes: {}
    ```

1. Under the `protocol` in the servers, type `security:` with the same indention.
1. Use the security that you created to protect the endpoint under your server.  
    Your server should now have the following:

    ```yaml
    servers:
      prod-http:
        url: $[eventRelay]/new-home-prod
        protocol: http
        security:
        - fortellisOAuth2: []
    ```

### Creating a Relative Reference for Your Message Component

1. After `components`, indent two spaces and type the following:

    ```yaml
    messages:
    ```

1. Below that, indent two spaces and type the name of your message and a colon.  
    For example, if you use `HelloWorld` for the `name` in your channels,
    you would type the following:  

    ```yaml
    HelloWorld:
    ```

1. From your message in the channel, copy everything from the `payload` to the bottom of your message.  
1. Below your new message component, indent two spaces and paste the contents of your message.  
    **Note:** Each item in the message must maintain its indent relative to the other fields.
    You may have to adjust the indent manually.
1. Replace everything in your channel message from the name down with the following:  

    ```yaml
    $ref: "#/components/messages/{yourMessageName}"  
    ```

    For this tutorial, we use the following:  

    ```yaml
    ref: "#/components/messages/HelloWorld"  
    ```

    **Note:** You have created a reference to the message within the components in the document.
1. Add the content type and the schema format that you can use on the platform.  
    In the new `component/messages`, add the following indented to align horizontally with properties:  

    ```yaml
    contentType: application/json
    ```

    **Note:** You can only use the `contentType: application/json`.

### Creating an Example for Your Message

Do not include an example in your asynchronous API spec.

## Downloading the GitHub Project

You can use this example as a starting point for your future projects.
You can find a complete file to download in the [AsyncAPIHelloWorld GitHub Repository](https://github.com/Fortellis/example-spec/blob/master/Async-API-Specs/HelloWorld.yaml).

## Next Steps

This tutorial has shown you how to do the following:

* Make your own AsyncAPI spec from scratch.
* Correctly indent your AsyncAPI specs for machine-readability.
* Correctly enter the required fields in the spec for Fortellis AsyncAPI specs.

You can go on the next tutorial to see how to implement the spec.
