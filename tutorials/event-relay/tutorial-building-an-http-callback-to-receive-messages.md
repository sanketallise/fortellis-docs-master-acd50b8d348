---
title: Building an HTTP Callback to Receive Messages in Node.js
author: Nathaniel Sandberg
last updated: 1/20/2022
---

In this tutorial, we expand on the previous tutorial [Integrating Your App and an Asynchronous API Producer in Node.js](/docs/tutorials/event-relay/tutorial-register-an-async-api-integration/) in creating an asynchronous app to deploy your endpoint to a public URL
that Fortellis can send events to.
Use this tutorial to find information on how to create your webhook and register your app on Fortellis to receive events.

> You must associate the `Data-Owner-Id` in the event header with the organization
> that is using the app.
> If you have multiple organizations subscribed to your app,
> you must send the event to the appropriate subscribed organization within the app.
> You can use the tutorial [Accessing Subscription Details Programmatically](/docs/tutorials/app-toolbox/using-the-subscriptions-endpoint) to get your subscriptions and match the `Data-Owner-Id` to an organization subscribed to your app.

## Tutorial

### What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create an endpoint to show the contents of the `./queue.json` file.
* Register the app to Fortellis.
* Create an app that can consume events from Fortellis.
* Verify the token that Fortellis sends with the event.

### Things to Remember in This Tutorial

You should remember the following:

* Use the following to start the server:  

    ```curl
    node index.js
    ```

* Use **Ctrl**+**C** to stop the server.
* Install the following before starting the tutorial:
    * Node
    * NPM

### Creating the Endpoint to Show the Queue

First, we create an endpoint so we can see when the `queue.json` file updates on the server.

* Create another get endpoint to show the contents of the file.  
    Beneath the post endpoint that receives the events, use the following to return the contents of the file:

    ```javascript
    app.get('/hello/world', function(req, res){
        //Read the file and send the contents back for a get request.
        fs.readFile('./queue.json', 'utf8', function(err, data) {
            if (err) throw err;
            console.log(data);
            res.send(data);
        });
    })
    ```

You can now use the get request to get updates and process them when ready.
For this tutorial, we assume that you would automate the process for removing requests
that you have already processed.
To see events sent to you, do the following:

1. Type the following to start the server:  

    ```curl
    node index.js
    ```

1. Go to `http://localhost:3001/hello/world` in your browser.

### Creating Your Public Endpoint

You must create a publicly available endpoint for Fortellis to send events to your API.
You can use any platform you have.
For this tutorial, we use a simple deployment with Glitch.

> Glitch apps fall asleep after five minutes of inactivity.
> Only use this deployment for demonstration purposes.
> For a more robust solution, use a server with more up-time.

### Creating Your Glitch Project

You will now create a public project with an endpoint that Fortellis can post events to.

1. Create the project from the Glitch express example repository.  
    1. Go to the repository at [hello-express](https://glitch.com/edit/#!/hello-express).
    1. Click **Remix to Edit** on the right side of the page to create your own express repository.
1. In the Glitch repository, replace the code above the `listener` constant in the `server.js` file with the code above the `app.listen` in your local `index.js` file.
1. Create the `queue.json` file in your project Glitch directory.
    1. In the Glitch repository, click plus sign on the left side of the page to make a new file.
    1. Enter the name of the file:  
        **queue.json**
    1. Click **Add This File** to add the file.
    1. Copy the content from your local `queue.json` file.
    1. Paste the content from your local `queue.json` to the `queue.json` file in the Glitch repository.
1. Do the following to add the body-parser to the Glitch project:
    1. Click the `package.json` file in the Glitch repository.
    1. Click **Add Package** to add a new package to the project.
    1. Type the following to find the correct package and version:  

        ```curl
        body-parser
        ```

    1. Click the package with the same version that you have in your local directory.
1. At the top of the page, click **Share**, and then copy the **Live Site** in a new browser tab.  
    **Note:** You should see the endpoint reporting that you cannot get from that location.
    Add `/health` to the end of the URL to see your health check endpoint and `{"status": "Up"}`.

You can now do the following:

* See your `queue.json` file
    if you enter the `/hello/world` endpoint at the end of your project URL.  
* Post new events to your `/hello/world/event` endpoint to add events to your `queue.json` file.
    Use the following curl request to post new events:

    ```curl
    curl --location --request POST '{yourProjectURL}/hello/world/event' \
    --header 'Type: application/json' \
    --header 'Content-Type: application/json' \
    --data-raw '{"id": "6620800b-7029-4a7a-8e80-cdd852fc01c8","number": 16,"haveYouSaidHello": true, "waysToSayHello": ["Hello","Hola","Hallo","Bonjour","Ola"],"helloID": {"language":"English","id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"}}'
    ```

* Refresh the browser to see the newly posted events.

Glitch does let the server sleep after five minutes, and it must be awake to receive events.
Only use this for demonstration purposes.
To wake the server up, visit `{yourProjectURL}/health` to check the health check endpoint and wake the server up.

### Registering Your App to Fortellis

You must register your app to receive calls from asynchronous APIs through Fortellis.
You can register for as many synchronous and asynchronous APIs as you want.
You must register the app to get your credentials.
Fortellis sends a token with your credentials to your webhook
when it sends an event to your endpoint.
Verify that the credentials in the token are yours to authorize the request to your endpoint.

#### Sign In and Choose the Organization

1. Sign in to [Fortellis]($[devNetworkUrl]).
1. Hover over your name in the upper-right corner.  
1. Click **Change**.  
    ![Change Organizations]($[docsUrl]/static/images/signInToFortellis.PNG)
1. Click the organization.
1. Click **OK**.  
    ![Click Okay For Organization]($[docsUrl]/static/images/clickOkayForOrganization.PNG)

#### Create the App

1. Click **New App**.  
    ![Change Organizations]($[docsUrl]/static/images/pictureOfApps.PNG)
1. Enter the app details:
    * **App Name**
    * **App Description**
    * **Website**
    * **Callback URL**  
    **Note:** See [Authorization Flows](/docs/tutorials/solution-integration/auth/) to determine if you need a callback URL.  
        ![API Integrations]($[docsUrl]/static/images/registeringApps.PNG)
1. Select the asynchronous API that you want your app to use.
    1. Click **Async API** for the **API Type** to show only the asynchronous APIs.
    1. Click the **Category** of the API if you know it.
    1. Click the **Publisher** of the API if you know it.
    1. Search for the asynchronous API using the search box if you know the name.
    1. Click the asynchronous API that you want to use.  
        ![asynchronous APIs]($[docsUrl]/static/images/asynchronousAPI.PNG)  
    **Note:** Select your asynchronous API
    if you created one to see the webhook receiving calls from your asynchronous API at the end of the tutorial.
1. Click **Webhook** to enter your webhook where you receive events.  
    **Note:** You may use different webhooks for different asynchronous APIs.
1. Enter the URL for your Glitch app with your `/hello/world` endpoint and click **Save**.  
    **Note:** Fortellis appends the `/event` endpoint to the end of the URL and sends the event to that endpoint.
1. Click **Register** at the bottom of the page to complete your registration.  
    ![Solution Register]($[docsUrl]/static/images/solutionRegister.PNG)

You can now click the app to get to the apps page and get your **API Key** and **API Secret**.
You can find your credentials in the **Credentials** section of the page.
Use the [Client Credentials Flow for Authorization](/docs/tutorials/solution-integration/client-credentials-flow) to get a token.

### Verifying the Access Token

Fortellis sends a token with your credentials to your webhook
when it sends an event to your endpoint.
Verify that the credentials in the token are yours to authorize the request to your endpoint.
We are going to create this locally first and then update Glitch.

1. Install the Okta JWT Verifier dependency.  
    On the command line, type the following:  

    ```curl
    npm install @okta/jwt-verifier
    ```

1. Import the const for the jwt verifier, and create the constant for the URL for the Fortellis tokens.  
    With the other constants, add the `jwtVerifier` constant in the `index.js` file:

    ```javascript
    const JwtVerifier = require("@okta/jwt-verifier");
    ```

1. Create the constant to reference to verify the token.  
    Under the `JwtVerifier` constant, type the following:

    ```javascript
    const jwtVerifier = new JwtVerifier({
        issuer: `$[authServerUrl]`,
        clientId: "{yourAPIKey}",
        assertClaims: {
            sub: "{yourAPIKey}",
            aud: "api_providers"
        },
    });
    ```

1. Use the `verifyToken` function to check that the token is valid.  
    Above `app.listen` in the `index.js` file, type the following to check that the token is valid:

    ```javascript
    //Verify the token in this request.
    function verifyToken(req, res, next) {
        //Get the authorization header in the request.
        const bearerHeader = req.headers["authorization"];
        //Check if the request has the authorization header.
        if (bearerHeader) {
            //Break the string array of the Bearer token into an array of substrings.
            const bearer = bearerHeader.split(" ");
            const bearerToken = bearer[1];
            req.token = bearerToken;
            next();
        } else {
            //Send a forbidden error if the request doesn't include the Bearer token.
            res.sendStatus(403);
            console.log("Forbidden user attempted to access the API.");
        }
    }
    ```

1. Add the token verification to the `/hello/world/event` endpoint to verify the token.  
    Add **[verifyToken]** in the `app.post` for the `/hello/world/event` endpoint:  

    ```javascript
    ...
    app.post('/hello/world/event', [verifyToken], function (req, res) {
    ...
    ```

1. Wrap the rest of the function in the verifier function.

    ```javascript
    jwtVerifier.verifyAccessToken(req.token, "api_providers").then(jwt => {
        console.log(jwt.claims.sub);
        ...
    })
    .catch(err => {
        console.log(err);
        res.send('Unauthorized');
    });
    ```

You can now call the endpoint,
but in the request, you must include a token
you created with your **API Key**.

To get a token, use the following:

```curl
curl -X POST --url $[authServerUrl]/v1/token \
--header 'accept: application/json' \
--header 'authorization: Basic {Base64EncodedAPIKey:APISecret}' \
--header 'Cache-Control: no-cache' \
--data 'grant_type=client_credentials&scope=anonymous'
```

To add events to the local queue, use the following with your token.

```curl
curl --location --request POST 'http://localhost:3001/hello/world/event' --header 'Type: application/json' --header 'Content-Type: application/json' --header 'Authorization: Bearer {yourToken}' --data-raw '{
    "id": "6620800b-7029-4a7a-8e80-cdd852fc01c8",
    "number": 16,
    "haveYouSaidHello": true,
    "waysToSayHello": [
    "Hello",
    "Hola",
    "Hallo",
    "Bonjour",
    "Ola"
    ],
    "helloID": {
    "language": "English",
    "id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"
    }
}'
```

You must now get an authorization token with your credentials and send those to your endpoint.
For more information on getting your token, see the [Client Credentials Flow for Authorization](/docs/tutorials/solution-integration/client-credentials-flow).

### Updating Glitch

Follow the same process you previously used to update the Glitch project and use the new dependency.

1. Copy the content of the `index.js` file except for `app.listen` and the `console.log` after that.
1. Delete the `server.js` file in the Glitch project up to the `listener` constant.
1. Paste the copied code above the `listener` constant.
1. Click on the `package.json` file.
1. Click **Add Package** to add a new package to the project.
1. Type the following to find the correct package and version:  

    ```curl
    @okta/jwt-verifier
    ```

1. Click the package with the same version that you have in your local directory.

You must now include your token
when you call your Glitch webhook endpoint.

```curl
curl --location --request POST '{yourProjectURL}/hello/world/event' --header 'Type: application/json' --header 'Content-Type: application/json' --header 'Authorization: Bearer {yourToken}' --data-raw '{
    "id": "6620800b-7029-4a7a-8e80-cdd852fc01c8",
    "number": 16,
    "haveYouSaidHello": true,
    "waysToSayHello": [
    "Hello",
    "Hola",
    "Hallo",
    "Bonjour",
    "Ola"
    ],
    "helloID": {
    "language": "English",
    "id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"
    }
}'
```

### Integrating Your App with Your Asynchronous API Producer

> Currently, a Fortellis Administrator must create the subscription to your app and activate it
> before an asynchronous API sends events to your app webhook.
> Contact [support@fortellis.io](mailto:support@fortellis.io) to ask an administrator to create your subscription and activate it.

If you followed the previous tutorial on [Publishing Messages and Verifying Results](/docs/tutorials/event-relay/publishing-messages-and-verifying-results),
you can now do the following:

1. Start your asynchronous API.
1. Update the `payload.json` file to send a new event to Fortellis.
1. Receive the updates through Fortellis at your webhook in your Glitch project.
1. Refresh the page to see the updated `queue.json` file.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create an endpoint to show the contents of the `./queue.json` file.
* Register the app to Fortellis.
* Create an app that can consume events from Fortellis.
* Verify the token that Fortellis sends with the event.

You can find the completed project on GitHub at [AsyncAPIAppHelloWorld](https://github.com/Fortellis/AsyncAPIAppHelloWorld).

If you download the example repository, do the following:

1. Follow the steps to [Creating Your Glitch Project](#creating-your-glitch-project).
1. Follow the steps to [Registering Your App on Fortellis](#registering-your-app-to-fortellis).
1. Add the dependencies for the `body-parser` and the `@okta/jwt-verifier` to Glitch.
1. Follow the steps in [Creating the Endpoint to Show the Queue](#creating-the-endpoint-to-show-the-queue).
1. Change the `sub` value and the `clientId` value to your **API Key** for your registered app.
