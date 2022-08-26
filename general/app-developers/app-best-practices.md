---
title: Best Practices for Apps
author: Nathaniel Sandberg
last updated: 7/16/2020
---

Fortellis provides these generalized guidelines to help you consume APIs on the platform.
APIs should generally provide you with the following,
but in cases where you have multiple options, choose the APIs that show these characteristics.

## Use JSON over XML

Consume APIs that use JSON for the following reasons:

* JSON is more light-weight than its XML counterpart.
* JSON is language agnostic.
* Your app will parse XML more slowly for the following reasons:
    * XML parsers run into more bugs.
    * XML is harder to parse.

## Use Idempotent APIs

> Idempotent means that the format of the calls does not change, only the values.

Use idempotent APIs for the following reasons:

* The calls and responses are in a consistent format.
* The API calls only change when you are changing the resource.
* You can more quickly program your app with consistent formatting.

### Example

You get the response back from the API.

`GET` `$[apiFortellisUrl]/sales/service/v3/cars/125`

```json
{
  "make": "Ford",
  "model": "F-150",
  "year": 2018,
  "cost": 25000
}
```

To update that record, we send a post with the same payload but updated values for the year and the cost.

`POST` `$[apiFortellisUrl]/sales/service/v3/cars/125`

```json
{
  "make": "Ford",
  "model": "F-150",
  "year": 2020,
  "cost": 35000
}
```

When you update the resource with all the information, you ensure that you do not remove the other information associated with the resource.

| HTTP Method | Idempotent | Safety |
| ----------- | ---------- | ------ |
| Get         | Yes        | Yes    |
| Head        | Yes        | Yes    |
| Put         | Yes        | No     |
| Delete      | Yes        | No     |
| Post        | No         | No     |
| Patch       | No         | No     |

## Use eTags to Avoid Concurrent Requests

You should also use `eTags` if you can find an API that provides them for the following reasons:

* You can avoid overwriting someone's recently updated data.
* You avoid multiple collisions when working within a microservices framework.

## Respond Correctly to HTTP Status Codes

You can receive the following status codes:

* 1XX
    * The server is still processing or switching protocols.
* 2XX
    * The response returned successfully.
* 3XX
    * The resource has moved, and the app should either follow the new URL or change the URL that it is using permanently.
* 4XX
    * The app made a request with an error and must make some changes before updating the request.
* 5XX
    * The server has errored internally.
        * If the user is waiting,
        abort the process and give the user the chance to retry or not.
        * If the app makes the call in the background and no user is waiting,
        stop making the request or use an exponential back-off strategy.

### Typical Responses

To respond to 400 errors for any flow, ask the user to check their input and retry.

* The user may have forgotten a required parameter.
* The user may have included an invalid parameter.
* The user or you may have made some other mistake in formatting the request.

The application may receive a 401 error if the application does not send a token with the request.

Use the following responses for 401 errors:

* To respond to 401 errors for the implicit flow and authorization code flow, redirect the user back to the login page.
* To respond to 401 errors for the client credentials flow, check the application server and the Fortellis UI to make sure that your solution keys match.  
    **Note:** You can rotate the keys at any time for security reasons.

The application may receive a 403 error for the following reasons:

* The user's token has expired.
* The user does not have the correct permissions in the token.
* The token is invalid in some other way.

You can probably resolve these errors by doing one of the following:

* For 403 errors in the implicit flow and the authorization code flow, the token has probably expired and you should redirect the user to the Fortellis login page to get a new token.
* For 403 errors for the client credentials flow, you probably have not requested access to the API spec in the UI.  
    **Note:** To request access to the API spec, follow the steps in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps/#the-apis-tab).

**Note:** A 403 error means you do not have permission to access the resource probably because one of the following:

* The OAuth token was revoked.
* The OAuth token is expired.
* The OAuth token has insufficient permissions.

Perform one of the following for 404 errors:

* Log the error internally to investigate later.
* If the resource is essential to the user's next steps, use a 404 template to show that you couldn't find the item that the user was looking for.

You can also do one of the following to help correct this error if it is caused by user error:

* Ask the user if this is a resource that the user created.
* Ask the user if they made a mistake in the resource that they were requesting.

If you get a 500 error for any credential flow, perform the following:

* Log the error.
* Notify the user that you are looking into the error.
* Look into the error.

### Retry Strategy

Use the following strategy when determining which calls to retry and which calls were successful:

* Retry 5XX error codes.
* Check 4XX error codes.
* Don't retry 3XX or 2XX error codes.

### Exponential Backoff Strategy

You could implement the following exponential back-off strategy:

1. Your app receives a 5XX status code.
1. You retry the request after 1 second.
1. You retry the request again after 2 seconds.
1. You retry the request again after 4 seconds.
1. You continue to retry at exponentially greater intervals until you reach a threshold and stop trying.

## Use Access Tokens Correctly

Fortellis uses your Fortellis issued OAuth token to validate your authorization.
Always use mature and tested libraries, software development kits, and frameworks for secure APIs of Oauth2 and OIDC to take care of most security concerns mentioned below:

* Do not share tokens with other users.
* Do not create a new token for each request.
* Reuse the same token for up to an hour with the following benefit:
    * You can save on the amount of time the app processes the request.
* Do not hardcode anything about the token because the API will know when the token expires, and store the token in the following manner:
    * If you use the client credentials flow, store the token in a database or in the app memory and use the token on each call.
    * If you use the authorization code flow or the implicit flow through Fortellis, maintain the token in a cookie with HTTPOnly and HTTPSecure flags set.  
    * Consider using the experimental cookie flag, SameSite.  
    **Note:** To avoid CSRF attacks, include CSRF tokens when you accept requests from clients.
* You should use the following guidelines when working with API issued tokens:
    * Do not make assumptions about the information in the token because it is only relevant to the issuer of the token.  
        > **Note:** The body of the token provides the following information:  
        * Version (ver): What is the version of JWT
        * JWT ID (jti): What the JWT ID is
        * Issuer (iss): Who issued the token
        * Audience (aud): Who should use the token to verify the user
        * Issued at (iat): When the token can start to be used
        * Expires (exp): When the token expires
        * Client ID (cid): Who the token was issued to
        * Scope (scp): What is the scope of the token
        * Subject (sub): What is the subject of the token
    * Any library you use to validate the token should check for the following in this order of priority:
        1. The token has a private key that you can validate with the public keys.
        1. The token is not expired.
        1. The token is not being used before the time it was set to be issued.
        1. The token has the correct audience.

## Security Considerations

You should observe standard security considerations when using APIs and creating apps offered on Fortellis.

### API Key, Secret, and Authorization Credentials

We recommend the following when authorizing users in your app:

* Avoid displaying your app's API key, API secret, and access token in plain text to the user.
    * You must use your key and secret to get your authorization token.
    * You do not want users getting authorization tokens on your behalf using your key and secret.
    * If you store your app's token in the browser, users can see your token and access Fortellis resources on your behalf.
* Do not store your key, secret, or authorization token in a common location, such as a shared drive, Confluence, SharePoint, or any other shared storage place, for the following reasons:
    * To prevent the end user from copying this information and misusing it
    * To prevent an attacker from misusing the information if an end user's PC is compromised
* If the UI must display the app's authorization, hash the authorization and hide it from the end user.
* Never expose credentials in the URL when making an API request.
* Consider the following when managing risks with sharing credentials, API keys, and API secrets:
    * Take security precautions that reflect the impact that compromised credentials and keys will have to the business.
    * Consider encrypting the keys and credentials if they would pose a higher risk when compromised.
    * Limit the number of users with access to secrets on a need-to-know and least privilege basis.
    * Document the chain of custody for all keys, secrets, and credentials.

### Subscription-Id

Fortellis uses the `Subscription-Id` to identify both the app activation and the API. We recommend the following for the `Subscription-Id`:

* Deploy the `Subscription-Id` in the application that the end user accesses rather than having the end user manually input this information.
    * If your application runs on the user's computer, authorize the user through a remote server with additional parameters when the user launches the application.
* Use some verification of the user within the application through their input to verify they are authorized.
    * Do not use the `Subscription-Id` for validation.
    * Use a user account and other parameters known to the app to authorize the user and store the `Subscription-Id` on the application's end.
