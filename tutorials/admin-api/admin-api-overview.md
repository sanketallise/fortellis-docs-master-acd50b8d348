---
title: Admin API Overview
author: Nathaniel Sandberg
last updated: 6/11/2021
---

You can screen users before giving them access to your APIs and get users' information with the Admin API.
You can implement the [Provider Admin API Specification](/docs/tutorials/admin-api/admin-api-specs) to get all the information
that you may need about the user and store that information on your internal system to access it later.
Implementing the [Provider Admin API Specification](/docs/tutorials/admin-api/admin-api-specs), you can secure your API and screen your users to make sure you have their information.

If you implement the Admin API, Fortellis does the following:

* Calls your Admin API to notify you that you have a new app activation request.
* Receives the requests you send to accept the activation request.
* Calls your Admin API to notify you that someone has deactivated an app activation.

**Note:** You will receive the `Subscription-Id` in the header of every request sent to your API.

You can use the Admin API to do the following:

* Get notified when an organization chooses you to be their API Developer.
* Control API traffic to your API.
* Get and save the connection context to use within your API.
* Get notified when an App Buyer deactivates their app through Fortellis.

**Note:** API Subscribers deactivate through Fortellis, and API Developers are responsible for removing the subscription on their end.

## Example Admin APIs

See our full [example Admin APIs](https://github.com/Fortellis/admin-api-example) on GitHub.
All examples include the following:

* The route for the activation request
* The route for the deactivation request
* The code to verify the token

You can find examples in the following languages:

### C# Example

You can see an example of a C# Admin API [here](https://github.com/Fortellis/admin-api-example/tree/master/dotnet).

### Node Example

You can see an example of a Node Admin API [here](https://github.com/Fortellis/admin-api-example/tree/master/node).

### Java Example

You can see an example of a Java Admin API [here](https://github.com/Fortellis/admin-api-example/tree/master/java).

## Implementing the Admin API

An Admin API consists of three parts:

* [Activation Endpoint](#activation-endpoint)
* [Deactivation Endpoint](#deactivation-endpoint)
* [Managing Activation Requests](#managing-activation-requests)

For the spec, click the following: [Provider Admin API Specification](/docs/tutorials/admin-api/admin-api-specs).

### Activation Endpoint

Fortellis calls the activation endpoint
when an organization chooses your API.
The request payload contains the following information:

* User who made the request.
* Organization.
* App.
* API.
* An identifier called `connectionId` that uniquely identifies the request.

**Note:** You will need the `connectionId` to accept the request.
Fortellis will forward requests to you only
after you accept the request using the Fortellis Connection Callback API.

When you are registering on Fortellis, enter the Admin API endpoint in the **Admin API URL**.

#### Implementing the Activation Endpoint

 1. Add HTTP/1.1 POST `/activate` route.  
    **Note:** Do not include `/activate` in your Admin API URL when you register.
 1. Authorize the request.
     * Validate JWT with Fortellis authorization server following the steps in [JWT Validation](/docs/tutorials/solution-integration/admin-api-authorization).
     * Ensure the audience is `api_providers`.
     * Ensure the subject in the token is your **Client ID** as of September 27, 2022.
 1. Perform activation business logic.
 1. Acknowledge with 200 OK.

```javascript
app.post('/activate', (req, res, next) => {
    /*
     * Authorize the request  
     */
    if (!req.headers.authorization  ||
        req.headers.authorization.split(' ')[0] !== 'Bearer') {
            return res.status(403).json({ code: 403, message: 'No Authorization header found!' });
         }
    try {
          const { claims : { audience = '', subject = '' } } = await verifyJWT(req.headers.authorization.split(' ')[1]);
          if(!audience ||
             audience !== "api_providers") {
                 throw { error: 'Invalid audience.' }
            }
            if(!subject || subject !== "{yourClientID}"){
                throws { error: 'Invalid subject' }
            }
      } catch (err) {
        return res.status(401).json({code: 401, message: "Unauthorized"});
    }
    /*
     * Perform activation business logic
     *    ex: Persist organization/solution/api/connection information  
     */

    /*
     * Acknowledge Fortellis with a 200
     */
     res.json({
        links: [
            {
                href: "http://example.com/activate",
                rel: "related",
                method: "POST"
            }
        ]
    })
});
```

### Accepting Activation Requests

With the connection callback endpoint, you can do the following:

* To accept activation requests, send the response with the `connectionId` back to the `$[connectionCallbackEndpoint]/:connectionId/callback` endpoint.
* To reject activation requests, do not respond to the request, and Fortellis will never complete the connection.

You must do the following to accept an activation request:

* Acknowledge the activation request with an HTTP 200 response.
* Accept the connection for Fortellis to forward traffic to your API.

**Note:** Fortellis only forwards traffic to your API after you have accepted the connection.

For the spec, click the following: [Fortellis Connection Callback API Specification](/docs/tutorials/admin-api/fortellis-connection-callback).

#### Accepting and Rejecting Activation Requests

**Note:** You must have the `connectionId` from the activation request made by Fortellis.
The `connectionId` uniquely identifies the organization and solution that chose your API.

 1. Fetch the access token from the Fortellis authorization server following the steps in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/).
 1. Call the Fortellis Connection Callback API using the following:
     * The token
     * The request payload
     * The `connectionId`
     * The `Content-Type`

```javascript
function acceptFortellisConnection(){
    /*
     * 1. Fetch access_token from Fortellis authorization server
     */
    const token = fetchAccessToken();
    const connectionId = getConnectionId();
    /*
     * 2. Call Fortellis Connection Callback API
     */
    http
          .post({
              url: `$[connectionCallbackEndpoint]/connections/${connectionId}/callback`,
              method: 'POST',
              headers: {
                   'Authorization': `Bearer ${token}`,
                   'Content-Type': 'application/json'
              },
              body: {
                  'status': 'accepted'
              }
          })
          .then(res => { console.log('Operation successful.') })
          .catch(err => { console.log('Operation failed. ' + err.message) });
}
```

### Deactivation Endpoint

When you receive a call at the `/deactivate` endpoint, you should know the following:

* Fortellis will no longer send you API calls from this subscription.
* You can perform any clean up within your API to remove that subscription.

#### Implementing the Deactivation Endpoint

 1. Add HTTP/1.1 POST `/deactivate/{connectionId}` route.
 1. Authorize the request.
     * Validate JWT with the Fortellis authorization server following the steps in [JWT Validation](/docs/tutorials/solution-integration/admin-api-authorization).
     * Ensure the audience is `api_providers`.
 1. Perform deactivation business logic.
 1. Acknowledge with 200 OK.

```javascript
app.post('/deactivate/{connectionId}', (req, res, next) => {
    /*
     * Authorize the request
     */
    if (!req.headers.authorization  ||
        req.headers.authorization.split(' ')[0] !== 'Bearer') {
      return res.status(403).json({ code: 403, message: 'No Authorization header found!' });
    }
   try {
        const { claims : { audience = '', subject = '' } } } = await verifyJWT(req.headers.authorization.split(' ')[1]);
        if(!audience ||
           audience !== "api_providers") {
              = throw { error: 'Invalid audience.' }
         }
         if(!subject || subject !== "{yourClientID}"){
            throws { error: 'Invalid subject' }
          }
    } catch (err) {
        return res.status(401).json({code: 401, message: "Unauthorized"});
    }
    const { params: { connectionId } = { } } = req;
    /*
     * Perform deactivation business logic
     *    ex: Necessary step to deactivate the connection identified by the connectionId
     */

    /*
     * Acknowledge Fortellis with a 200
     */
     res.json({
        links: [
            {
                href: "http://example.com/deactivate",
                rel: "related",
                method: "POST"
            }
        ]
    });
});
```

### Managing Activation Requests

You can accept activation requests using the Fortellis Connection Callback API.
Fortellis only forwards traffic for requests you have accepted.
To reject a request, simply ignore it.

**Note:** You can only manage activation requests that you have previously acknowledged with an HTTP 200 response.

For the spec, click the following: [Fortellis Connection Callback API Specification](/docs/tutorials/admin-api/fortellis-connection-callback).

#### Accepting and Rejecting Activation Requests

**Note:** You must have the `connectionId` from the activation request made by Fortellis.
The `connectionId` uniquely identifies the organization and app that chose your API.

 1. Fetch the access token from the Fortellis authorization server following the steps in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/).
 1. Call the Fortellis Connection Callback API using the following:
     * The token
     * The request payload
     * The `connectionId`
     * The `Content-Type`

```javascript
function acceptFortellisConnection(){
    /*
     * 1. Fetch access_token from Fortellis authorization server
     */
    const token = fetchAccessToken();
    const connectionId = getConnectionId();
    /*
     * 2. Call Fortellis Connection Callback API
     */
    await http
          .post({
              url: `$[connectionCallbackEndpoint]/connections/${connectionId}/callback`,
              method: 'POST',
              headers: {
                   'Authorization': `Bearer ${token}`,
                   'Content-Type': 'application/json'
              },
              body: {
                  'status': 'accepted'
              }
          })
          .then(res => { console.log('Operation successful.') })
          .catch(err => { console.log('Operation failed. ' + err.message) });
}
```
