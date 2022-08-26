---
title: API Spec Introduction
author: Nathaniel Sandberg
last updated: 3/31/2022
---

You can create your own API specs and implement those when you join Fortellis.
You can keep the following in your specs:

* Innovations through the design of the API.
* A customized design that suits your needs.

With the spec, you can create a specialized design that is part of your company's domain with your `basePath` appended to it.

We recommend that
you start with the API spec
when creating an API for the following reasons:

* You waste time and money when you change an API that is already in place.
* You can change the design easily and inexpensively as you discuss it with stakeholders.  
    **Note:** Build API Specs with 1 or 2 of the developers that will eventually use the API.
* You can break a client using the API if you make changes after creating the API.
* You can use the API spec as the source of truth for the API to generate the following:
    * API code  
    * API client code  
    * Documentation  
    * Tests  
    * Mocks  

With the API spec, API Developers can implement the design while App Developers create solutions.

Without the API design, API Developers may do one of the following:

* Build the wrong API.
* Build the API in the wrong way.

API Developers can also consume their API specs and may discover the following:

* Inconveniences
* Missing features
* Missing data
* Missing documentation

You can make well-informed decisions to make the API better when you are designing it.

In your spec, you should mostly focus on the following frontend design decisions:

* The API `basePath`  
    **Note:** In OpenAPI 3, you include the `basePath` in the `server` object that contains the `schema`, `host`, and `basePath`.
* The API parameters:
    * Query parameters
    * Path parameters
    * Body parameters
    * Header parameters
* The status codes

You can use the following steps to design and create an API:

1. Find out how the majority of consumers are going to use the API.
1. Use the REST architectural style.
1. Design a blueprint of the API using an API description language, the OpenAPI Specification.
1. Design the API that the App Developer interacts with.
1. Build a prototype of the API, and collect feedback on the design.
1. Adapt and iterate over the design.
1. Use a generative technique to create the API.

You can promote your API with good documentation in the following ways:

* Your documentation encourages businesses to adopt your API.
* App Developers can produce apps in parallel with you
    if you create the following in advance of releasing the API:
    * API Specs
    * Developer Guides

You might face the following challenges with API documentation:

* The API develops rapidly.
* You must maintain the documentation while updating the API.
* Documentation must speak to the unlimited codebases the API can support.
* The codebases cannot speak to each other, so they need a common basis for communication.

These problems lead to the [OpenAPI Specification (OAS)](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md).

The OpenAPI Specification does the following:

* Defines a standard description language for RESTful APIs.
* Provides a human and computer-readable description of the API and its capabilities.
* Provides documentation generation tools with information to create the following:
    * Clients
    * Servers
    * Testing tools

Fortellis uses the OpenAPI Specification for documentation for the following reasons:

* OAS defines an API's contract.
* OAS allows all the API stakeholders to understand the following:
    * The API's functionality
    * The API's interactions
* OAS is language agnostic.
* OAS does not have any implications on API or code.

You can use OAS to perform the following:

* Convert your swagger documentation into an interactive API console that consumers can use to interact with the API.
* Visualize the API.
* Help internal team members understand the API and agree on its attributes.
* Collaborate with others using tools, such as swagger hub.
* Promote your API with external consumers who can get comfortable with it before implementing the API.
* Create tests for the API.
* Generate API code and SDKs.

## Editors

You can use the following tools to assist you when creating your spec:

* [SwaggerHub](https://swagger.io/tools/swaggerhub/)
* [Swagger Editor](https://editor.swagger.io/)
* [VS Code](https://code.visualstudio.com/)  
    **Note:** Fortellis now provides an extension to Visual Studio Code to lint and visualize your spec
        when you are creating it.

### SwaggerHub

SwaggerHub does the following:

* Provides validation.
* Provides a live proxy that you can use.
* Provides the following:
    * The code editor
    * The validator
    * The graphical interface

### Swagger Editor

Swagger Editor does the following:

* Provides a validator.
* Provides a graphical UI renderer.
* Provides automated client-side tools.
* Provides automated server-side tools.

### VS Code

Visual Studio Code gives you a text editor to update your spec.
In addition, Fortellis provides the [Fortellis Spec Tools](https://marketplace.visualstudio.com/items?itemName=Fortellis.fortellis-spec-tools) extension that does the following:

* Validates your spec.
* Renders the spec to give you a visual.
* Provides immediate feedback when you are creating your spec.

## OpenAPI Specification (OAS)

On Fortellis, you use the OpenAPI Specification (OAS) to design APIs for the following reasons:

* OAS provides specialized language concepts for APIs.
* Developers can use the OAS description to implement and consume the API design much faster.

OAS specs tell developers the following:

* What your API does.
* How to use it.
* How to implement it.
* How it is secured.
* Where its endpoints are.
* What parameters does it use.
* What payloads it consumes.
* What payloads it sends.

OAS does the following:

* Presents a human and machine-readable format.
* Gives a precise presentation of what the API should do.
* Gives a standardized format to any API or app.
* Gives a higher level of abstraction.
* Describes the structure of the response instead of the actual response.
* Gives a single source of truth for an API.
* Applies the root level objects to all operations unless otherwise overridden.

**Note:** To keep your specs relatively small and manageable, we recommend the following:

* Use smaller complete documents with internal references.
* Iterate over a relatively small list of endpoints in the spec.

### YAML Syntax

You do the following when you are creating specs using YAML syntax:

* Use whitespace to structure the document.
* Use key-value pairs and objects.
    * You should use keys in YAML that are scalar strings.
* Use tags that only allow the JSON Schema rule set.
    * Use the JSON schema 4.0 draft.

#### Root Elements

**Note:** Do not indent the root elements.
Your spec must pass the linter to register the API on Fortellis.

##### Swagger 2.0

Place the following elements in the Swagger description with the following indentation:

* `swagger: "2.0"`: Use `swagger` to identify the spec and version that you are using.  
    **Note:** You can now use the `2.0` or `3` version for your specs.  
* `host`: Use the host to specify the hostname or the IP address of the host.
    * On Fortellis, always use `$[apiFortellisUrl]`.
* `basePath`: Append the `basePath` to your API URL.  
    **Note:** Fortellis will call your API at `{yourURL}` + `{yourBasePath}`.
* `schemes`: Use the scheme `https` on Fortellis.
* `tags`: Use tags to categorize your API on Fortellis.  
    **Note:** You must use an object for your tags.  
* `info`: Define the metadata about all the endpoints in the API spec.
    * `title`: Define the title of the API spec.
    * `description`: Describe the API spec.
    * `contact`: Include the following:
        * `name`
        * `email`
        * `url`
    * `license`: Link to the license of the API spec.
        * `name`
        * `url`
    * `version`: Enter the semantic version of the API spec.  

        ```yaml
        info:
            version: "19.0.0"
            title: Fortellis Spec
            description: |
                # Your Product - Your Namespace - Your Spec

                Provide a brief description of your API.

                # What does this API do?
                Say what someone could do with your API here.

                # Intended Audience
                Provide a brief description of the audience who will consume the API and who will benefit from the API.
            contact:
                name: Developer Evangelists
                url: https://fortellis.io/contact-us
                email: support@fortellis.io
        ```

* `paths`: Use the path property to define the resources of the API spec.
* `responses`: Use responses to define reusable responses from the API.
* `parameters`: Use the parameters to define reusable parameters objects.
* `definitions`: Reuse definitions of data structures within your API spec.

##### OpenAPI 3

Place the following elements in the OpenAPI spec with the following indentation:  

* `openapi: "3.0.0"`: Use `openapi` to identify the spec and the version that you are using.  
* `info`: Define the metadata about all the endpoints in the API spec.  
    * `title`: Define the title of the API spec.  
    * `description`: Describe the API spec.  
    * `contact`: Include the following:  
        * `name`
        * `email`
        * `url`
    * `license`: Link to the license of the API spec.  
        * `name`
        * `url`
    * `version`: Enter the semantic version of the API spec.  

        ```yaml
        info:
            version: "19.0.0"
            title: Fortellis Spec
            description: |
                # Your Product - Your Namespace - Your Spec

                Provide a brief description of your API.

                # What does this API do?
                Say what someone could do with your API here.

                # Intended Audience
                Provide a brief description of the audience who will consume the API and who will benefit from the API.
            contact:
                name: Developer Evangelists
                url: https://fortellis.io/contact-us
                email: support@fortellis.io
        ```

* `servers`: User the servers to create an object with all your servers.  
    * `url`: In the `url`, identify the url of the API with the `schema`, `host`, and `basePath`.  
    **Example:** `$[apiFortellisUrl]/yourBusinessDomain/yourProductName/v4/yourResource`
* `paths`: Use the path property to define the resources of the API spec.  
* `tags`: Use tags to categorize your API on Fortellis.  
    **Note:** You must use an object for your tags.
* `components`: Use the components to create reusable components that you can use throughout your spec.
    * `parameters`: Use the parameters to define reusable parameters for the API to receive in requests and return in responses.
    * `responses`: Use `responses` to define reusable responses that the API returns.
    * `schemas`: Use the `schemas` to define reusable input and output data types for an API.  

#### Endpoints

Describe the endpoints that the consumer can call in the paths property.

You can use any of the following methods for the path:

* `GET`
* `POST`
* `PUT`
* `DELETE`

Define the following properties for each method:

* `summary`: Describe the purpose of the resource.
* `description`: Provide a verbose description of the resource.
* `schemas`: Use the `https` scheme in your Fortellis methods.  
    * In Swagger 2.0, you may want to specify your scheme at the root to reuse that throughout your YAML spec.
    * In OpenAPI 3, you must specify the scheme in the `servers` object.
* `parameters`: List the input parameters for that endpoint.
* `responses`: List the responses by their status codes.
* `consumes`: List the media types consumed by the resource.
    * On Fortellis, always use `application/json`.
    * Do not include `consumes` in the OpenAPI 3.
* `produces`: List the media types produced by the resource.
    * On Fortellis, always use `application/json`.
    * Do not include `produces` in OpenAPI 3.
* `operationId`: Give the resource a unique name.

You must use the following properties in your responses:

* `description`: Describe the response.
* `schema`: Describe the HTTP body of the response with one of the following:
    * Primitive type
    * Array
    * Object
* `headers`: Send the response headers back to the client.
* `examples`: Show examples of the responses you send back to the client from the API service.

#### Reusable Elements

Include all reusable elements in the components in OpenAPI 3.
Put the reusable elements in that section in Swagger 2.
You can use the following elements throughout your API description to make your APIs more consistent and to write them more quickly:

* Parameters
    * Declare reusable parameters in the `parameters` property.
    * In the Swagger 2.0 spec, use `parameters` as a root element.
    * In the OpenAPI 3 spec, use `parameters` in the components.
    * Use the following properties in your parameters:
        * `name`: Show the name of the parameter.  
        * `type`: Show the data type of the parameter.  
        * `in`: Show where the client puts the parameter.  
            **Note:** You can use any of the following:
            * `path`
            * `query`
            * `formData`
            * `header`
            * `body`
        * `schema`: Give the HTTP body schema of the request.
        * `required`: Show whether the parameter is optional.
        * `description`: Give a verbose description of the parameter.
        * `format`: Give additional rules for the parameter values.
        * `collectionFormat`: Describe how to format the serialized array.  
            **Note:** Use one of the following formats for this field:
            * `csv`
            * `ssv`
            * `tsv`
            * `pipes`
            * `multi`
        * `items`: Describe the elements in an array.  
            **Note:** You must set the type to array to use items.  

        ```yaml
        header.Request-Id:
            name: Request-Id
            in: header
            required: true
            type: string
            description: A correlation ID that should be returned back to the caller to indicate the return of the given request
        ```

* Schema
    * Declare reusable schemas in the `definitions` property of the root element of 2.0 specs and in the components of version 3 specs.
    * Use the strategy for [creating schemas](/docs/tutorials/spec-writing/linting-api-specs#definitions) to fill out your definitions section.
* Responses
    * Reuse schemas to declare responses in the `responses` property.
    You can use the same schema definition in all your error responses:

    ```yaml
    responses:
        BadRequest:
            description: 400 - Bad Request
            headers:
                Request-Id:
                type: string
            schema:
                $ref: "#/definitions/ErrorResponse"
        Unauthorized:
            description: 401 - Unauthorized
            headers:
                Request-Id:
                type: string
            schema:
                $ref: "#/definitions/ErrorResponse"
        Forbidden:
            description: 403 - Forbidden
            headers:
                Request-Id:
                type: string
            schema:
                $ref: "#/definitions/ErrorResponse"
        NotFound:
            description: 404 - Not Found
            headers:
                Request-Id:
                type: string
            schema:
                $ref: "#/definitions/ErrorResponse"
        ServerError:
            description: 500 - Internal Server Error
            headers:
                Request-Id:
                type: string
            schema:
                $ref: "#/definitions/ErrorResponse"
    ```

##### References

Reference other areas of the document with the following:

* Use a hashtag to reference the document.  
* Reference the `components` section first in OpenAPI 3.
* Reference the section by name:  
    * `definitions`
    * `parameters`
    * `responses`
* Use the name of the item that you want to reference.  

```yaml
$ref: "#/definitions/reference"
```

### Communicate the API to Your Stakeholders

Fortellis only accepts OAS formatted APIs due to the following:

* You can easily share the API specs when using OAS.
* You may have to share the API spec often and need a standard format.
* You may need to communicate API specs to developers that don't use the same technologies or tools.
    * The API Developer needs to share the spec with client-side developers.

#### Documentation Pros and Cons

|Type of Documentation|Pro|Con|
|--|--|--|
|Prose Documentation|Humans can read it.|It may not be precise enough.|
|Code as Documentation|It's more precise.|It may be too long, too complicated, and unpublishable due to property rights.|
|OAS|You create human- and machine-readable API documentation. You can automatically generate the documentation using the OAS instead of creating HTML pages for each API.|You must learn OAS and have specialized knowledge to create the document.|

When you write APIs in OAS, the documentation has the following properties:

* It contains only relevant information.
* The information is available in a structured and compact form.
* You do not need to format or style the information.  
    **Note:** You do need to follow syntactic rules when writing the documentation in OAS.
* Machines can process these documents.
* Machines can transform these documents into HTML pages.
* Developers can also create interactive documentation that use the spec to show resource structures.

Create easy-to-read and complete API documentation to entice developers to use your API service. For more information, see [Creating API Developer Guides](/docs/tutorials/api-lifecycle/creating-developer-guides).

## Examples

To make your experience better, use a template spec to get started:

* Fortellis Specs share many identical fields, such as schemes.
* You can go straight to creating your path and the responses that you would like to create.

You can find example specs in the following locations:

* [Fortellis GitHub](https://github.com/Fortellis/example-spec)
* [Creating Specs](/docs/tutorials/spec-writing/creating-api-specs)
