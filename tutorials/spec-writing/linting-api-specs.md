---
title: Linting API Specs
author: Nathaniel Sandberg
last updated: 4/11/2022
---

You can create specs quickly and easily on Fortellis.
You can use the linter to check where mistakes are and fix the mistakes before submitting the spec.
With the spec linter, you can ensure that your APIs follow Fortellis conventions, making them more accessible for App Developers.

## Using the Linter

Developers can do the following with the spec linter:

* Check that their spec conforms to Fortellis rules and standards.
* See errors and receive early feedback on their specs.
* Receive the following information about errors in the spec:
    * The line number for errors.
    * The error message.

**Note:** You can use the guidelines and instructions below to write a spec
that passes the linter,
but we recommend
that you start with a spec example from [GitHub](https://github.com/Fortellis/example-spec) and modify it to suit your purposes.

With the API linter, API Developers can create specs that conform to the Swagger 2.0 specification and OpenAPI 3 specification.

## Spec Requirements

Specs must meet the following requirements:

* Follow the OpenAPI 2.0 specification or OpenAPI 3 specification.
* Include an `operationId` for Fortellis to assign a unique ID to that endpoint.
* Conform to current Fortellis standards.

For more information on the spec, please see one of the following:

* [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md) for version 2 specs.
* [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.1.0.md) for version 3 specs.

### File Structure

Format your `yaml` files to make sure they pass the linter for the following reasons:

* The linter looks for required fields at specific indent points.
* The linter may determine that a field is missing if it's not in the correct column.
* The linter may throw misleading errors in poorly formatted files.

Click [here](#example-file-structure) to see an example of the file structure at the bottom of the page.

## Levels

**Note:** Do not indent these to show the linter that they are at the root of the spec.

### Swagger 2.0

Write the following at the top level, or root, of the spec:

* `swagger:`
* `info:`
* `host:`
* `basePath:`
* `schemes:`
* `tags:`
* `paths:`
* `parameters:`
* `definitions:`
* `responses:`

#### Correct

```yaml
swagger: "2.0"
info:
host:
basePath: /service/sessions/v4
schemes:
  - https
tags:
  - name: yourTagName
    description: Your tag description
paths:
parameters:
definitions:
responses:
```

### OpenAPI 3

Write the following at the tope level, or root, of the spec:

* `openapi:`
* `info:`
* `servers:`
* `tags:`
* `paths:`
* `components:`

#### Correct

```yaml
openapi: "3.0.0"
info:
host:
servers:
    - url: $[apiFortellisUrl]/{youBasePath}
tags:
  - name: yourTagName
    description: Your tag description
paths:
components:
```

### Version

Include the version at the top of the file.
You can include either the Swagger version or the OpenAPI version.

```yaml
swagger: "2.0"
```

```yaml
openapi: "3.0.0"
```

### Info

You must include an info section with the following information:

* Version
* Title
* Description
* Contact
    * Name
    * URL
    * Email

Indent each of the first four with one tab, or two spaces, and indent the last three with two tabs, or four spaces.

#### Correct

```yaml
info:
  version: "47.0.0"
  title: Fortellis Sample Application
  description: Provide a really good description.
  contact:
    name: Developer Evangelists
    url: https://fortellis.io/contact-us
    email: support@fortellis.io
```

#### Version

You must include the semantic version of the spec within the `info` section.

> **Note:** Fortellis will take the API version from the version you use in your spec and autopopulate the version in the UI.

#### Title

Consider the following when creating a spec title:

* You must include the title in the spec.
* Users will see the title that you enter when you are registering the spec, not the title from the spec.
* You should keep the title in the spec and the title in the UI consistent.

#### Description

To convert the text in the `.yaml` file to markdown, include a pipe `|`, press enter, and then press tab.

```yaml
description: |
    This is the description.
```

To ensure your spec parses in the [API Directory]($[apiReferenceUrl]), you should include the following headings in markdown to follow the Fortellis standard:

* Product Name
* Description
* Intended Audience

##### Incorrect

```yaml
  description: |
    This is just some information that I think is interesting.
```

##### Correct

```yaml
description: |
    # Your Product - Your Spec

    Provide a brief description of your API.

    # What does this API do?
    Say what someone could do with your API here.

    # Intended Audience
    Provide a brief description of the audience who will consume the API and who will benefit from the API.
```

#### Contact

You must provide your contact information in the `yaml` file.

**Note:** You will see an error telling you
that the info section is missing the contact object
if you leave this section out.

You should include the following information in the contact object:

* `name`
* `url`
* `email`

**Note:** Keep the contact information with one indent and the fields in the contact object with two indents.

### Swagger Hosts

Use three parts to create the Swagger URLs:

* `host`
* `basePath`
* `schemes`

#### Host

All Fortellis APIs route through `$[apiFortellisUrl]`.

```yaml
host: $[apiFortellisUrl]
```

#### basePath

* In the API spec, define a `basePath`.  
    **Note:** We recommend that you conform to the [Fortellis API Design guidelines](/docs/general/api-design/api-spec-introduction).  
* Begin the `basePath` with a slash `/`.  
* Do not end the `basePath` with a slash `/`.  

#### Incorrect

```yaml
basePath: /vehicles/pricing/v2/
```

#### Correct

```yaml
basePath: /vehicles/pricing/v2
```

#### Schemes

You must include the scheme in your spec.

```yaml
schemes:
  - https
```

**Note:** You must use an array to show the schemes that your spec can use. Indicate schemes with the hyphen before the first item in the array.

### OpenAPI Hosts

For OpenAPI 3, use an array of the following in the servers object:

* `host`
* `basePath`
* `schema`

#### Servers

You must include the `servers` in your spec, or your spec fails the linter:

```yaml
servers:
    - url: $[apiReferenceUrl]/{yourBasePath}
```

You include your target URL when you are registering the spec.

### Tags

You must include a tag in your spec to categorize the spec in Fortellis.
API consumers can use these to sort APIs in the [API Directory]($[apiReferenceUrl]).
You can include any unique tag you want,
but users only see tags that Fortellis already has.
To add custom tags to the [API Directory]($[apiReferenceUrl]), please contact [support@fortellis.io](mailto:support@fortellis.io).

```yaml
tags:
  - name: yourTagName
    description: Your tag description
```

### Difference Between Version 2.0 and 3

When you create a spec using version 2, you must have each of the following at the root:

* `definition`
* `responses`
* `parameters`

When you create a spec using version 3, you must put those within the components section.

### Creating Objects

You may have to create objects when creating definitions.
To create an object in `yaml`, perform the following:

1. Beneath the label for the object, type the following indented one more time than the object.

    ```yaml
    type: object
    ```

1. Beneath that, type the following with a space and your description.

    ```yaml
    description:
    ```

1. Beneath that, type the following:

    ```yaml
    properties:
    ```

1. For each of the **properties**, include the following:
    * `type`
    * `title`
    * `description`
    * Any `default`

### Definitions

We outline the following sections in this order because the linter checks for the following in version 2:

1. You have a `definitions` section that the `responses` section references.
1. You have a `responses` section that the `paths` section references.
1. You have a `parameters` section that the `paths` section references.

In version 3, you include these in the components,
and the linter does not enforce their use.

You must do the following when you create the definitions:

* Consider the best information and format for your API consumers.
* Create the API responses based on the workflows that the API consumers need.

#### Creating API Spec Definitions

The linter throws an error for version 2 specs without examples.
The linter does not enforce examples in version 3 specs.

1. Create an example of the JSON payloads that you want to return to the app.  
    **Note:** Check that the JSON you create is valid at the [JSON Formatter and Validator](https://jsonformatter.curiousconcept.com/) site.  
1. Create a schema from the existing payload with a schema generator.

##### Creating the Examples

You can use the [jsonschema.net](https://jsonschema.net) to create a draft of the schema and validate it.
Use draft 4 of the schema validator.

1. Delete from the first curly bracket up to the curly bracket after properties.
1. Delete the last curly bracket.
1. Delete all of the `$id` properties.
1. Format the JSON at [JSON Formatter and Validator](https://jsonformatter.curiousconcept.com/).

##### Adding the Example to the Spec

1. Under the definitions in the [Swagger Editor](http://editor.swagger.io/), type the name of the new definition.
1. Under the name, press tab to indent once and type the following:

    ```yaml
    definitions:
        ExampleDefinition:
            description: Use this to describe the definition.
            type: object
            properties:
    ```

1. Under **properties**, press tab once and paste the schema into the [Swagger Editor](https://editor.swagger.io/) to let Swagger format the definition for you.
1. Copy the original example you created the schema from.
1. Under the new definition, type the following with two indentations:

    ```yaml
    example:
    ```

1. Under example, paste the original example that you created the schema from.
1. Delete the other examples from that schema.

##### Including the Required Fields in the Schema

1. Type **required** with two indents before the example and include any fields that the definition requires.

    ```yaml
    required:
        - message
    ```

1. Type a hyphen followed by the required fields beneath **required:**.

You should have a complete definition that looks like the following:

```yaml
definitions:
  ExampleDefinition:
    type: object
    description: You can make this description anything that you want.
    title: The name you give to the definition
    properties:
      message:
        type: string
        title: The Message Schema
        description: An explanation about the purpose of this instance.
        default: ''
      numberOfParts:
        type: integer
        title: The Numberofparts Schema
        description: An explanation about the purpose of this instance.
        default: 0
      deliveryAvailable:
        type: integer
        title: The Deliveryavailable Schema
        description: An explanation about the purpose of this instance.
        default: 0
      locations:
        type: integer
        title: The Locations Schema
        description: An explanation about the purpose of this instance.
        default: 0
      partsPerLocation:
        type: array
        title: The Partsperlocation Schema
        description: An explanation about the purpose of this instance.
        default: []
        items:
          type: object
          title: The Items Schema
          description: An explanation about the purpose of this instance.
          default: {}
          properties:
            address:
              type: string
              title: The Address Schema
              description: An explanation about the purpose of this instance.
              default: ''
            numberOfParts:
              type: integer
              title: The Numberofparts Schema
              description: An explanation about the purpose of this instance.
              default: 0
    required:
      - message
      - numberOfParts
      - deliveryAvailable
      - locations
      - partsPerLocation
    example:
      message: Success
      numberOfParts: 2005
      deliveryAvailable: 1
      locations: 5
      partsPerLocation:
        - address: Austin
          numberOfParts: 25
        - address: Dallas
          numberOfParts: 52
        - address: San Antonio
          numberOfParts: 198
```

The definition must be a definition object.

We recommend that you include the following in your definitions:

* `description`
* `properties`
* `required`

> You must provide examples in your version 2 specs.
> You do not have to provide an example in your version 3 specs.

* Name definitions using PascalCase.
* Include a description in the definition property.
* Include an example with each definition that conforms to the schema.
* Define the response  `schema` in the `definitions` section.

Once you have formatted the `.yaml` file with the Swagger Editor, you can paste that definition in your local file.

### Responses

In spec version 2, you must include the responses section with all the response patterns in strings.
In spec version 3, you do not have to include a separate responses section with your components,
but we recommend that you do so to make more of your components reusable.
You must also include `content` in the response version 3 specs.

* Use strings for the response codes.  
    **Examples:** "400" and "200"
* Use success responses 2XX or 3XX.
* Use error responses 4XX.
* Return the `Request-Id` with each query.

Use the definitions to create the responses.

#### Version 3 Responses

```yaml
ProductSize:
  description: This sends the product size for a particular product to help you determine if you have the inventory space for that product.
  headers:
    Request-Id:
      type: string
  content:
    application/json:
      schema:
        $ref: "#/components/schemas/ProductSize"
```

#### Creating Responses for Version 2 Specs

```yaml
ProductSize:
  description: This sends the product size for a particular product to help you determine if you have the inventory space for that product.
  headers:
    Request-Id:
      type: string
  schema:
    $ref: "#/definitions/ProductSize"
```

You must have the responses with the following additional indents under `responses` to create proper `.yaml` files:

* Indent the name of the response with one tab, or two spaces.
* Indent the following with two tabs:
    * `description`
    * `headers`
    * `schema`
* Indent the following with three tabs:
    * Names of parameters
    * References for definitions
* Indent the `type` for parameters with four tabs.

You should also use common error responses for each one of your endpoints throughout the document.

```yaml
BadRequest:
  description: "400" - Bad Request
  headers:
    Request-Id:
      type: string
  schema:
    $ref: "#/definitions/ErrorResponse"
```

You can then reference this response in each instance of that error code in every path:

```yaml
responses:
  "400:
    $ref: "#/responses/BadRequest"
```

Use this single reference for every `400` error at every path.

#### Creating Responses for Version 3 Specs

```yaml
ProductSize:
  description: This sends the product size for a particular product to help you determine if you have the inventory space for that product.
  headers:
    Request-Id:
      type: string
  content:
    application/json:
      schema:
        $ref: "#/components/schemas/ProductSize"
```

You must have the responses with the following additional indents under `responses` to create proper `.yaml` files:

* Indent the name of the response with two tabs, or four spaces.
* Indent the following with three tabs:
    * `description`
    * `headers`
    * `content`
* Indent the following with three tabs:
    * headers
    * `application/json` under `content`
* Indent the following with four tabs:  
    * `type` for the headers
    * `schema` for `application/json`
* Indent the `$ref` with five tabs.

You should also use common error responses for each one of your endpoints throughout the document.

```yaml
BadRequest:
  description: "400" - Bad Request
  headers:
    Request-Id:
      type: string
  content:
    application/json:
      schema:
        $ref: "#/components/definitions/ErrorResponse"
```

You can then reference this response in each instance of that error code in every path:

```yaml
responses:
  "400:
    $ref: "#/components/responses/BadRequest"
```

Use this single reference for every `400` error at every path.

### Parameters

In version 2 specs, you must reference all parameters in the parameters section in your spec.
In version 3 specs, we recommend that you still reference the parameters in the component section for the following reasons:

* You reuse many parameters throughout the API spec.
* You can reference one parameter throughout your spec instead of rewriting the same parameter multiple times.

Case the parameters in the following manner:

* Use camelCase in your pathParameters.
* Use Upper-Kebab-Case in your Header-Parameters.
* Use flatcase for any queryparameters.
* Use PascalCase for BodyParameters.

Use the typeofparameter.casedparametername convention to identify the parameters in this section:

* `header.Request-Id`
    * You must include `header.Request-Id` in the parameters.
* `path.pathExampleParameter`
* `query.queryexampleparameter`
* `body.BodyExampleParameter`

You must include the following in the parameters:

* `name:`
* `in:`
* `required:`
* `type:`
* `description:` (optional, but recommended)

Use the same case in the name field that you use when you are providing a label for the parameter or the parameter will not pass the linter.

```yaml
parameters:
  header.Request-Id:
    name: Request-Id
    in: header
    required: true
    type: string
    description: A correlation ID that should be returned to the caller to indicate the return of the given request
```

You must have the parameters with the following additional indents below `parameters`:

* Indent the label for the parameter once.
* Indent the following in the parameter with two tabs:
    * `name:`
    * `required:`
    * `type:`
    * `in:`
    * `description:`

### Paths

Include the following in each of your paths:

* The method
    * The summary
    * The description
    * The operationId
    * The parameters
        * You must include the `Request-Id` parameter.
    * Consumes (`Content-Type`)
        * You do not include `consumes` in version 3 specs.
    * Produces (`Accept`)
        * You do not include `produces` in version 3 specs.
    * Tags
        * Spec parsers use the tags to group endpoints with the same tags together.
    * The responses
        * You must include all responses for all endpoints.

Include the proper indentation and the colon after the name of the field to successfully go through the linter.

You must use the following indentation for the `.yaml` to be in the correct format:

* Indent the endpoint with one tab, two spaces.
* Indent the method with two tabs.
* Indent the following with three tabs:
    * `summary:`
    * `description:`
    * `operationId:`
    * `parameters:`
    * `consumes:`
    * `produces:`  
        > You do not include the `consumes` and `produces` fields in version 3 specs.
    * `tags:`
    * `responses:`
* Indent the following with four tabs and include a hyphen before each:
    * The `$ref:` for the parameters.
    * In version 2 specs, the content it consumes.
        * `application/json`
    * In version 2 specs, the content it produces.
        * `application/json`
    * The tags.
    * The response codes.
        * Use double quotes around each of these. See the example below for formatting information.
* Indent the relative link to the definitions for the responses definitions with five tabs.

You must define the following with relative references to those sections in the `.yaml` file:

* `parameters`
* `responses`
* `definitions`

**Note:** Create sections in the document with references to the information
that you are going to repeat throughout the document.

To create a relative path within the document, perform the following:

1. Use `$ref:` to indicate that you are referring to another location.
1. Use quotes to reference that location.
1. Start the location with a hash.
1. Use a slash to go to the location within the file.  
1. Use the title of the section in the document, either `parameters`, `responses`, or `definitions`.  
    **Note:** Include `components` in version 3 specs, such as `#/components/parameters/{yourComponent}`.
1. Use the title of the `parameters`, `responses`, or `definitions`.  

#### Example Path

```yaml
/all-product-info/{pathProductNumber}:
    get:
        tags:
        - query
        summary: Specific Product
        description: Find a specific product.
        operationId: productInformation
        parameters:
            - $ref: "#/parameters/header.Uid"
            - $ref: "#/parameters/header.Organization"
            - $ref: "#/parameters/path.pathProductNumber"
            - $ref: "#/parameters/header.Request-Id"
        consumes:
            - application/json
        produces:
            - application/json
        responses:
            "200":
                $ref: "#/responses/PathProductNumber"
            "400":
                $ref: "#/responses/BadRequest"
            "401":
                $ref: "#/responses/Unauthorized"
            "403":
                $ref: "#/responses/Forbidden"
            "503":
                $ref: "#/responses/ServiceUnavailable"
```

### Linting Options

Developers can follow these guidelines to create standard Fortellis specs.
Developers can then go to [Developer Network]($[devNetworkUrl]) to upload the spec and pass the linter.
