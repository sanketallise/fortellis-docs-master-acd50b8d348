---
title: Spec Publishing Rules
author: Nathaniel Sandberg
last updated: 4/1/2022
---

You can use these guidelines to guide you in correcting any errors in your spec.
The following guidelines outline what the indentation in the spec file needs to be and which fields are required.
With these rules, you can more quickly create a valid spec that you can start making an API for.

You must adhere to the following standards when publishing specs on Fortellis:

* Follow one of the following specifications when creating your spec:  
    * OpenAPI 2.0 Specification.
    * OpenAPI 3 Specification.
* Follow custom Fortellis standards.

Keep the following in mind when working with the Fortellis API standards:

* These standards ensure that both humans and machines can understand your spec without accessing the source code.
* The linter ensures that you receive well-defined errors for the rules that the spec violates.

For more information on version 2 specs, see the [OpenAPI Specification 2.0](https://swagger.io/specification/v2/) for more information.
For more information on version 3 specs, see the [OpenAPI Specification 3.0.0](https://swagger.io/specification/) for more information.

## Fortellis API Rules

Please include the required information when you are creating your API specification URLs.  

### Info Object

The info block has the following uses:

* You can use the `info` block to define general metadata about your spec.
* Fortellis uses the metadata when listing your spec on the platform.

#### Required Fields

| Field Name  | Type     | Description                                                                              |
| ----------- | -------- | ---------------------------------------------------------------------------------------- |
| title       | `string` | The title of your API spec                                                               |
| description | `string` | A short description of the spec details                                                  |
| version     | `string` | The version of the spec using [semantic versioning](https://semver.org/) format          |

#### Example

```yaml
info:
  title: 'My API Specification'
  description: 'Here's where I describe my API'
  version: 1.0.0
```

### Basepath

Remember the following when creating your `basePath`:

* You can use anything for your `basePath` within `{yourNameSpace}`.  
    **Note:** You must use a unique `basePath` within `{yourNameSpace}`.
    If you need to request a new `{nameSpace}` to use that particular `basePath`,
    contact [Fortellis Support](mailto:support@fortellis.io).
* We recommend that you only use the major version in the `basePath` of the spec.
* Do not end the `basePath` with a trailing `/`.  
    **Note:** Fortellis can then append the defined paths correctly.
* Fortellis routes calls to `{yourURL}/` + `{yourNameSpace}` + `{youBasePath}`.
* If you are creating a version 3 spec, your `basePath` is what follows `$[apiFortellisUrl]`.

#### Example

```yaml
# Incorrect
basePath: '/test/'

# Correct
basePath: '/test'
```

### Paths Object

Remember the following about the `paths` object:

* Use the `paths` object to hold the relative paths to a given endpoint and its operations.
* Fortellis appends this path directly to the given spec `basePath`.

Include the following to create a well-formed path:

* The path URL
* Available operations
    * `GET`
    * `POST`
    * `DELETE`  
  **Note:** Include an `operationId` and a `description`.
    Fortellis uses these to display information about the endpoint to the user as part of Fortellis' API documentation.

The spec should define the following:

* All of the parameters it accepts.
* Common responses returned.

#### Example

```yaml
paths:
  /users:
    get:
      operationId: getUsers
      description: "Get the users available"
      parameters:
        #...
      responses:
        #...
```

### Parameter Object

Follow these rules to create the parameter object:

* List each parameter available on that endpoint to the user for an endpoint and an operation.
* Define the parameter in one of the following locations:
    * `path`
    * `query`
    * `header`
* Do not use duplicate parameters even
  if they are in different locations.

#### Path Parameters

Use the following guidelines for path parameters:

* Define path parameters in the path URL.
* Do not duplicate any other path parameters in the path.
* Define path parameters within the parameters object.  
  **Note:** Make these definitions required.
* Use the `in:path` field.

##### Example

```yaml
paths:
  /users/{userId}:
    get:
      description: "Get user by ID"
      operationId: "getUserById"
      parameters:
        - name: userId
          in: path
          required: true
          type: string
          description: 'The given user ID of the requested user'
    # ...
```

#### Header Parameters

Include the required header parameters listed below based on the given path operation:

| Operation      | Required Headers             |
| -------------- | ---------------------------- |
| All Requests   | `Request-Id`                 |
| All Responses  | `Request-Id`                 |

##### Example

```yaml
paths:
  /users:
    get:
      description: 'Get all users'
      operationId: 'getUsers'
      parameters:
        - name: Request-Id
          in: header
          required: true
          type: string
      # ...
      responses:
        200:
          description: Accepted
          headers:
            Content-Language:
              type: string
            Content-Type:
              type: string
            Request-Id:
              type: string
      # ...
```

#### Body Object

Use the following guidelines when creating the body object:

* You can only define the body parameter once for each endpoint.
* You may not use `formData` parameters with the `body` parameter
  because these are mutually exclusive.

##### Example

```yaml
paths:
  /users/{userId}:
    get:
      description: "Get user by ID"
      parameters:
        - name: body
          in: body
          required: true
          schema:
            #...
```

### Response Object

The response object should define the following:

* Define the `responses` with a valid response code.
    * 1XX
    * 2XX
    * 3XX
    * 4XX
    * 5XX
* Create a description for the response.

#### Example

```yaml
# Incorrect
responses:
  '99':
    description: 'OK'

# Correct
responses:
  '200':
    description: 'OK'
  '404':
    description: 'Resource not found.'
```
