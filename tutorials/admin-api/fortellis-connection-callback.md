---
title: Fortellis Connection Callback Spec
author: Nathaniel Sandberg
last updated: 6/14/2021
---

You can accept your user requests using the Connection Callback Spec endpoints.
You can send the `connectionId` to the callback endpoint,
and Fortellis will forward requests from that `connectionId` to your API.
Use the [Swagger Spec Editor](https://editor.swagger.io) to more easily consume the spec.
With the Connection Callback, you can approve Subscribers and start to monetize your development work on your APIs.

## Fortellis Connection Callback Spec

```yaml
swagger: "2.0"

info:
    version: "v1"
    title: Connection Callback API
    description: The Connection Callback API allows providers to accept a connection to begin receiving Fortellis API traffic.

host: subscriptions.fortellis.io
basePath: /
schemes:
    - https

securityDefinitions:
    Authorization:
        type: oauth2
        flow: accessCode
        authorizationUrl: $[authServerUrl]/v1/authorize
        tokenUrl: $[authServerUrl]/v1/token
        scopes:
            openid: default scope

paths:
    /connections/{connectionId}/callback:
        post:
            summary: Allows a provider to accept a connection
            description: Fortellis calls the `/activate` endpoint to indicate to the API Developer that a Subscriber has requested a connection. The API Developer can call the `/connections/:id/callback` to accept or reject the request.
            operationId: callbackConnection
            tags:
                - connections
            security:
                - Authorization:
                      - openid
            parameters:
                - $ref: "#/parameters/Authorization"
                - $ref: "#/parameters/Accept"
                - $ref: "#/parameters/Accept-Charset"
                - $ref: "#/parameters/Prefer"
                - $ref: "#/parameters/Request-Id"
                - $ref: "#/parameters/path.connectionId"
                - $ref: "#/parameters/body.callbackConnectionPayload"
            produces:
                - application/json
            responses:
                200:
                    description: OK
                400:
                    description: Bad Request
                    schema:
                        $ref: "#/definitions/ErrorResponse"
                403:
                    description: Forbidden
                    schema:
                        $ref: "#/definitions/ErrorResponse"
                404:
                    description: Not Found
                    schema:
                        $ref: "#/definitions/ErrorResponse"

parameters:
    Authorization:
        name: Authorization
        in: header
        required: true
        type: string

    Accept:
        name: Accept
        in: header
        required: true
        type: string
        enum:
            - application/json

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

    body.callbackConnectionPayload:
        name: callbackConnectionPayload
        in: body
        required: true
        description: |
            You provide Fortellis with your acceptance, rejection, error message,
            and any of your internal authorization if you chose to use your
            External authorization. If you accept, Fortellis forwards calls
            to your API. You may reject the connection at any time, even after
            a previous acceptance, if you want to stop receiving API calls from
            that Subscriber.
        schema:
            $ref: "#/definitions/CallbackConnectionPayload"

    path.connectionId:
        name: connectionId
        in: path
        required: true
        type: string
        format: guid
        description: A unique id for this connection

definitions:
    enableConnectionStatus:
        type: string
        description: |
            The API Developers indicate that they have accepted or rejects the request:
            * Rejected: The API Developer has not accepted the requested connection
            and Fortellis should not send the API any traffic.
            * Accepted: The API Developer has accepted the requested connection and
            is ready to receive Fortellis API traffic
        enum:
            - accepted
            - rejected
    addCustomParameters:
        type: object
        description: |
            If you chose External Auth or subscription specific parameters
            when you registered the API, you can authorize users with your
            `client_id`, `client_secret`, or any additional subscription
            specific parameters you.
        additionalProperties:
            type: string
    CallbackConnectionPayload:
        properties:
            status:
                $ref: "#/definitions/enableConnectionStatus"
            auth:
                $ref: "#/definitions/addCustomParameters"
            error:
                type: string
                description: If the requested connection is rejected, the provider can use this field to say why.
        required:
            - status

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
```
