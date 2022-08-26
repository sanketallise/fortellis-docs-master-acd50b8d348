---
title: Admin API Spec
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can get information on the endpoints that Fortellis sends information to in the Admin API specification.
You may use the [Swagger Spec Editor](https://editor.swagger.io) to more easily consume the spec.
The spec gives you the design to implement to gather information on the users requesting access to your API and accept connection requests to the API.

## Provider Admin API Spec

```yaml
swagger: '2.0'

info:
  version: "v2"
  title: Provider Admin API
  description: Fortellis uses the Admin API spec to send connection requests to the provider.

basePath: /

paths:
  /activate:
    post:
      summary: activate account
      description: activate account
      operationId: activateAccount
      tags:
        - activate account
      parameters:
        - $ref: '#/parameters/Authorization'
        - $ref: '#/parameters/Accept'
        - $ref: '#/parameters/Accept-Charset'
        - $ref: '#/parameters/Prefer'
        - $ref: '#/parameters/Request-Id'
        - $ref: '#/parameters/activateRequest'
      produces:
        - application/json
      responses:
        200:
          description: OK
          headers:
            Content-Type:
              type: string
            Request-Id:
              type: string
          schema:
            $ref: '#/definitions/ActivateResponse'
        400:
          description: Bad Request
          schema:
            $ref: '#/definitions/ErrorResponse'
        403:
          description: Forbidden
          schema:
            $ref: '#/definitions/ErrorResponse'

  /deactivate/{connectionId}:
    post:
      summary: deactivate an account
      description: deactivate an account
      operationId: deactivateAccount
      tags:
        - deactivate account
      parameters:
        - $ref: '#/parameters/Authorization'
        - $ref: '#/parameters/Accept'
        - $ref: '#/parameters/Accept-Charset'
        - $ref: '#/parameters/Prefer'
        - $ref: '#/parameters/Request-Id'
        - $ref: '#/parameters/connectionId'
      produces:
        - application/json
      responses:
        200:
          description: OK
          headers:
            Content-Type:
              type: string
            Request-Id:
              type: string
          schema:
            $ref: '#/definitions/ActivateResponse'
        400:
          description: Bad Request
          schema:
            $ref: '#/definitions/ErrorResponse'
        403:
          description: Forbidden
          schema:
            $ref: '#/definitions/ErrorResponse'

parameters:
  Authorization:
    name: Authorization
    in: header
    required: true
    type: string
    description: Fortellis Authorization Bearer Token. This token will be able to be verified with the Fortellis Identity Provider

  Accept:
    name: Accept
    in: header
    required: true
    type: string
    enum:
      - application/json
    description: Which content types, expressed as MIME types, the api is able to understand

  Prefer:
    name: Prefer
    in: header
    required: true
    type: string
    enum:
      - return=representation
      - return=minimal

  Accept-Charset:
    name: Accept-Charset
    in: header
    required: true
    type: string
    enum:
      - utf-8

  Request-Id:
    name: Request-Id
    in: header
    required: true
    type: string
    format: guid

  activateRequest:
    name: createAccountRequest
    in: body
    description: The details to create an account
    schema:
      $ref: '#/definitions/ActivateRequest'

  connectionId:
    required: true
    name: connectionId
    in: path
    description: The id for the specific connection activation/deactivation being requested.
    type: string


definitions:
  # Entities
  ActivateRequest:
    properties:
      organizationInfo:
        type: object
        properties:
          id:
            type: string
            format: guid
            description: The id of the organization the activation request is being made for
          name:
            type: string
            description: The name of the organization the activation request is being made for
          address:
            type: string
            description: The address of the organization the activation request is being made for
          phoneNumber:
            type: string
            description: The phoneNumber of the organization the activation request is being made for
        required:
          - id
          - name
          - address
          - phoneNumber
      appInfo:
        type: object
        properties:
          id:
            type: string
            format: guid
          name:
            type: string
            description: The name of the app
          developer:
            type: string
            description: The organization that developed the app
          contactEmail:
            type: string
          appOrgId:
            type: string
            format: guid
            description: The organization id of the app
        required:
          - id
          - name
          - developer
          - contactEmail
          - appOrgId
      entityInfo:
        type: object
        properties:
          id:
            type: string
            format: guid
            description: The id of the organization the activation request is being made for
          name:
            type: string
            description: The name of the organization the activation request is being made for
          address:
            type: string
            description: The address of the organization the activation request is being made for
          phoneNumber:
            type: string
            description: The phoneNumber of the organization the activation request is being made for
        required:
          - id
          - name
          - address
          - phoneNumber
      solutionInfo:
        type: object
        properties:
          id:
            type: string
            format: guid
          name:
            type: string
            description: The name of the app
          developer:
            type: string
            description: The organization that developed the app
          contactEmail:
            type: string
          appOrgId:
            type: string
            format: guid
            description: The organization id of the app
        required:
          - id
          - name
          - developer
          - contactEmail
          - appOrgId
      userInfo:
        type: object
        properties:
          fortellisId:
            type: string
            format: email
        required:
          - fortellisId
      apiInfo:
        type: object
        properties:
          id:
            type: string
          name:
            type: string
          implementationName:
            type: string
        required:
          - id
          - name
          - implementationName
      subscriptionId:
        type: string
        format: guid
        description: The id for the specific app and organization combination
      connectionId:
        type: string
        format: guid
        description: The id for the specific connection activation being requested. This value will be used in the callback to Fortellis.
    required:
      - organizationInfo
      - appInfo
      - userInfo
      - apiInfo
      - subscriptionId
      - connectionId
    example:
      organizationInfo:
        id: 2529a384-aee3-4b9a-af35-ed08e77dee15
        name: Super Car Dealership
        address: 1234 Some St. Columbus, OH 43235
        countryCode: US
        phoneNumber: (614) 555-5555
      appInfo:
        id: f83eaff0-3f88-4ebd-9fc8-25f051632968
        name: Awesome Application
        developer: Awesome App Developer
        contactEmail: contact@example.com
        appOrgId: 2059d5f5-bccb-40c8-937e-f217311feabd
      entityInfo:
        id: 2529a384-aee3-4b9a-af35-ed08e77dee15
        name: Super Car Dealership
        address: 1234 Some St. Columbus, OH 43235
        countryCode: US
        phoneNumber: (614) 555-5555
      solutionInfo:
        id: f83eaff0-3f88-4ebd-9fc8-25f051632968
        name: Awesome Application
        developer: Awesome App Developer
        contactEmail: contact@example.com
        appOrgId: 2059d5f5-bccb-40c8-937e-f217311feabd
      userInfo:
        fortellisId: user@example.com
      apiInfo:
        id: v1-appointments
        name: Appointments
        implementationName: "Fortellis Spec 8"
      subscriptionId: 2529a384-acc3-4b6a-a835-ed09e77dee15
      connectionId: 2529a384-add3-4b6a-a935-ed09e45def12

  ActivateResponse:
    properties:
      links:
        type: array
        items:
          $ref: '#/definitions/LinkDescriptionObject'
    required:
     - links

  # Common
  ErrorResponse:
    properties:
      code:
        type: integer
        format: int32
      message:
        type: string
    required:
      - code
      - message

  LinkDescriptionObject:
    required:
      - href
      - rel
    properties:
      href:
        type: string
        description: The target URI
      rel:
        type: string
        description: The link relation type
      method:
        type: string
        description: The HTTP verb that MUST be used to make a request to the target of the link
      title:
        type: string
        description: The title property provides a title for the link and is a helpful documentation tool to facilitate understanding by the end clients
```
