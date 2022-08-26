---
title: Admin API Tutorial in Node.js
author: Nathaniel Sandberg
last updated: 10/26/2021
---

Fortellis has created a schema that API Developers can use to integrate with Fortellis.
API Developers may want to integrate and implement that schema for the following reasons:

* You can use the schema to protect your APIs and vet users.
* Fortellis only sends requests from users that you approve if you implement the Admin API.

You can use the [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs) to receive requests from Fortellis at your endpoint,
and you can use the [Connection Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback) to approve those requests.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create the `/activate` endpoint.  
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Create a GitHub repository for your project.
* Deploy the endpoint to Heroku.

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
* Create an API on Fortellis before you start.  
    **Note:** For more information on registering your API, see [Registering APIs](/docs/tutorials/api-lifecycle/registering-apis).
* View the [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs) for the exact spec and endpoints.

You can find a completed project with the example on GitHub: [admin-api-implementation](https://github.com/Fortellis/admin-api-implementation).

## Workflow

1. Enter your Admin API URL
    when you are updating your API.
1. Implement the Admin API spec to receive requests at that endpoint
    when Fortellis calls your Admin API URL.
1. Use an API or another method to get the connection requests
    that Fortellis sends to your Admin API URL.
1. Send the connectionId to `$[connectionCallbackEndpoint]/connections/:connectionId/callback` endpoint to accept the request.  

You do not have to take any action to refuse the connection.

## Creating a Web Server

### Initializing the NPM Project

1. Create a new folder for your project.  
    For this tutorial, name the project `admin-api-example`.
1. On the command line, change to the directory you just created.  
    On the command line, type the following:

    ```curl
    cd admin-api-example
    ```

1. Create an npm project.  
    In the project directory on the command line, type **npm init**.
1. Fill out the information that npm prompts for.  
    **Note:** To use the default values,
    press `Enter` at each one of the prompts.
    * package name
    * version
    * description
    * entry point: (index.js)
    * test command
    * git repository
    * keywords
    * author
    * license: (ISC)
1. Accept the file contents when you are asked to confirm.  
    On the command line, type the following:

    ```curl
    yes
    ```

1. Finish initializing the project.  
    On the command line, press **Enter**.

### Creating the Express Server

1. Install the Express dependency.  
    On the command line, type the following:

    ```curl
    npm install express
    ```

1. In your text editor, create the `index.js` file in the root directory.
1. Require Express in the `index.js` file.  
    At the top of the file, type the following:

    ```curl
    const express = require('express');
    ```

1. Create the app.  
    At the tope of the file, type the following:

    ```curl
    const app = express();
    ```

1. Include the code to start the server at the bottom of the file.

    ```javascript
    // Start the server on port 3000
    app.listen(3000, '127.0.0.1');
    console.log('Node server running on port 3000');
    ```

1. Between the constants and `app.listen`, create the response for the health check.  
    Type the following to create the health check endpoint:

    ```curl
    app.get('/health', (req, res)=>{res.json({"status":"Up"})})
    ```

1. Run the server to check that it is up.  
    On the command line, type the following to start the server:

    ```curl
    node index.js
    ```

**Note:** You can now go to `localhost:3000/health` in your browser to see the status of the server.

## Implementing the Admin API

Fortellis calls the `/activate` endpoint after a subscriber configures their app to use your API.
Fortellis will include the following:

* `organizationInfo`
* `appInfo`
* `userInfo`
* `apiInfo`
* `connectionId`

The `connectionId` uniquely identifies the request,
and you'll use it to accept the request using Fortellis Connection Callback API,
which we will cover later in this tutorial.
You must also authorize requests to your `/activate` endpoint.
See below for more information.

### Setting the Activate Endpoint to Receive Requests

1. Log the request in the console.  
    Beneath the constants, type the following in the `index.js` file:

    ```javascript
    app.post('/activate', function(req, res){
        console.log(req);
    })
    ```

    **Note:** Consider the following:
    * You will probably want to send this information to your database, but we use a JSON file in this tutorial.
    * You can then get the Subscriber's information from the database when you are verifying your subscriptions.

1. Create the response that Fortellis expects to receive when sending activation requests.  
    Type the following in the `index.js` file within the `app.post` method:

    ```javascript
    res.send('{"links": [{"href": "localhost:3000", "rel": "self", "method": "post", "title": "Activation Request"}]}');
    ```

1. Install the `body-parser` to evaluate incoming requests.  
    On the command line, type the following:

    ```javascript
    npm install body-parser
    ```

1. In the `index.js` file, declare the constant for the body-parser.  
    With the other constants in the `index.js` file, type the following:

    ```javascript
    const bodyParser = require('body-parser');
    ```

1. Set the app to use `JSON`.  
    In the constants, type the following:

    ```javascript
    app.use(bodyParser.json({extended:true}), express.json())
    ```

1. Start the Express server.  
    On the command line, type the following:

    ```javascript
    node index.js
    ```

**Note:** You can now call the endpoint from Postman or any other API testing tool at `http://localhost:3000/activate`:

* Use the Postman collection that you get when you initially submit your API for testing.
* Replace the URL in the activation request with the `http://localhost:3000/activate`.
* Continue to call the endpoint throughout the tutorial to test it before moving on to the next steps.
* Use the payload that you will get when Fortellis sends activation requests to your `activate` endpoint found in the test collection.

#### Request to the Server

```curl
curl -X POST http://localhost:5000/activate -H "accept: application/json" \
-H  "Content-Type: application/json" -d '{"organizationInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"appInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"entityInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"solutionInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"userInfo":{"fortellisId":"user@example.com"},"apiInfo":{"id":"v1-appointments","name":"Appointments","implementationName":"Fortellis Spec 8"},"subscriptionId":"2529a384-acc3-4b6a-a835-ed09e77dee15","connectionId":"2529a384-add3-4b6a-a935-ed09e45def12"}'
```

#### Response from the Server

```json
{
    "links": [
        {
            "href": "string",
            "method": "string",
            "title": "string",
            "rel": "string"
        }
    ]
}
```

### Verifying the Token

You should verify the tokens that Fortellis sends with activation requests.

1. Stop the server if you haven't already.  
    On the command line, type the following:  
    **Ctrl**+**C**  
1. Install the `@okta/jwt-verifier` dependency.  
    On the command line, type the following:  
    **npm install @okta/jwt-verifier**  
    **Note:** You may use another open-source JWT verification library,
    but you will have to modify the code to work with that library.
1. Include the following after `app.use` in the `index.js` file:

    ```javascript
    const JwtVerifier = require ('@okta/jwt-verifier');

    const jwtVerifier = new JwtVerifier({
        issuer: `$[authServerUrl]`,
        assertClaims: {
            aud: "api_providers",
            sub: "{yourClientID}"
        }
    })
    ```

    The constants define the following:
    * The JWT dependency
    * The Fortellis token domain
    * The token verifier
    * The audience in the token
1. Include the function to verify the token at the bottom of the index.js file.

    ```javascript
    //Verify the token in this request.
    function verifyToken(req, res, next) {
        const bearerHeader = req.headers['authorization'];

        if (bearerHeader) {
            const bearer = bearerHeader.split(' ');
            const bearerToken = bearer[1];
            req.token = bearerToken;
            next();
        } else {
            res.sendStatus(403);
            console.log("Forbidden user attempted to access the API.");
        }
    }
    ```

1. Add the `verifyToken` function to the app.post to have it verify the token and send the response back.

    ```javascript
    app.post('/activate',[verifyToken], (req, res)=>{
        //Verify the token.
        jwtVerifier.verifyAccessToken(req.token, "api_providers")
        .then(jwt => {
            res.set('Content-Type', 'text/html');
            console.log(jwt.claims.aud);
            //Return the minified payload that Fortellis expects to receive back.
            res.send('{"links": [{"href": "localhost:3000", "rel": "self", "method": "post", "title": "Activation Request"}]}');
            //console.log(req);
            //Get Just the request body.
            console.log('Got Body', req.body);
            console.log("The subscriptionId is " + req.body.subscriptionId);
            console.log("The connectionId is " + req.body.connectionId);
        })
        .catch(err => {
            res.sendStatus(403);
        })
    })
    ```

1. Save the file.
1. Restart the server.  
    On the command line, type `node index.js`.  

If you send the same request as before,
you should receive a forbidden error.

### Successfully Sending a Request to Your Local Admin Implementation

You can now get a token from Fortellis and send requests to your local Admin API implementation.

1. Get a token from Fortellis.  
    In the Postman collection in the Admin API Testing folder,
    click the **Generate Infrastructure Bearer Token**,
    and then click **Send** to get your token.
1. Copy the `access_token` that you receive in the response body.
1. Call your local endpoint `http://localhost/3000/activate` with the Bearer token in the header.  
    In the `/activate` request in Postman, click **Authorization**,
    and paste your token into **Token**.  
    **Note:** The token expires in one hour.
    You should now be able to successfully send a request.

### Saving the Requests to a JSON File

You should probably save the `subscriptionId` to a database,
but for this tutorial, we'll use a JSON file.

1. At the top of the index.js file with the other constants, import the `fs` module to write information to the `connectionRequests.json` file.  

    ```javascript
    const fs = require('fs');
    ```

1. In the `app.post` after `console.log("The connectionId is " + req.body.connectionId);`, add the function to add the new object to the json file.

    ```javascript
    fs.readFile('./connectionRequests.json', 'utf-8', function(err, data){
        if(err) throw err
        const arrayOfObjects = JSON.parse(data)
        arrayOfObjects.connectionRequests.push(req.body)
        console.log(arrayOfObjects)
        const writer = fs.createWriteStream('./connectionRequests.json');
        writer.write(JSON.stringify(arrayOfObjects))
    })
    ```

1. Create the `connectionRequests.json` file with the following content:

    ```json
    {"connectionRequests":[]}
    ```

**Note:** Stop and restart the local server.
You should now be able to send a request and save it to the file.

### Validating Data to the Activate Endpoint

Please keep the following in mind when validating data:

* You may skip validating the payload if you are just trying to see how the Admin API works.
* In an actual API, you want to have some data validation to ensure you are receiving the JSON that you expect.

1. Install the dependency to validate the request payload.  
    On the command line, type the following:  
    **npm install express-jsonschema**
1. At the top of the file, add the constant to the `index.js` file.  
    In the constants, type the following:

    ```javascript
    const validateSchema = require('express-jsonschema').validate;
    ```

1. Create the schema file for activation requests on a site, such as [JSONschema.net](https://www.jsonschema.net), using the activate payload.  
    **Note:** You can use the payload below to make the schema.

    ```json
    {
        "organizationInfo": {
            "id": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d",
            "name": "Jareds Entity TBD",
            "address": "6701 Burnet Rd Austin TX 78757 US",
            "countryCode": "US",
            "phoneNumber": "2819234655"
        },
        "appInfo": {
            "id": "575d7513-2d43-4f10-a07b-c8e6c3c5ab3a",
            "name": "b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf Testing App",
            "developer": "Jared.Jones@cdk.com",
            "contactEmail": "Jared.Jones@cdk.com",
            "appOrgId": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d"
        },
        "entityInfo": {
            "id": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d",
            "name": "Jareds Entity TBD",
            "address": "6701 Burnet Rd Austin TX 78757 US",
            "countryCode": "US",
            "phoneNumber": "2819234655"
        },
        "solutionInfo": {
            "id": "575d7513-2d43-4f10-a07b-c8e6c3c5ab3a",
            "name": "b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf Testing App",
            "developer": "Jared.Jones@cdk.com",
            "contactEmail": "Jared.Jones@cdk.com",
            "appOrgId": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d"
        },
        "userInfo": {
            "fortellisId": "nathan.sandberg@cdk.com"
        },
        "apiInfo": {
            "id": "api-v2-b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf",
            "name": "api-v2-b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf",
            "implementationName": "Fortellis Spec 8"
        },
        "subscriptionId": "568a62f4-a7c3-468c-973c-004e6cc50a9b",
        "connectionId": "5b8a6242-a1aa-42c5-bec8-4eaa634d5470"
    }
    ```

1. Save the schema file as `schema.json` in the Express root directory.  
1. If necessary, remove the following from the root schema in the `schema.json` file:
    * definitions
    * $schema
    * $id
    * examples
1. At the top of the `index.js` file, add the schema validator.  
    Type the following at the top of the `index.js` file with the other constants:  

    ```javascript
    const schema = require('./schema.json')
    ```

1. Add the function to validate against the schema in the `app.post(/activate` request in square brackets.  
    Type `validateSchema({body:schema})` between the endpoint and the request and response.  

    ```javascript
    `app.post('/activate', [verifyToken, validateSchema({body:schema})],...`
    ```

**Note:** If you change a key or the format of the payload now when sending to the `/activate` endpoint,
then you will receive an error.

## Creating the Endpoint to Delete the Requests

You do not need to take any action to reject a connection.
For this step, we simply remove the request,
and Fortellis will never connect the Subscriber to the API.

### Adding the Delete Endpoint

1. Add post delete endpoint to the `index.js` file below the activate endpoint.  
    Type the following:

    ```javascript
    app.post('/deleteRequest',(req, res)=>{})
    ```

1. Add the constant to get the `subscriptionId` from the payload in the app.post `/delete` request.  
    Type the following to get the `subscriptionId`:

    ```javascript
    const deleteSubscriptionId = req.body.subscriptionId;
    ```

### Writing the Updated JSON to the File

1. Add the read function to the `/deleteRequest` endpoint.

    ```javascript
    fs.readFile('connectionRequests.json', 'utf-8', function(err, data){
        if(err) throw err
    })
    ```

1. Beneath the if statement, get the array of objects with the read file.  
    Type the following to get the data from the `connectionRequests.json` file:

    ```javascript
    const arrayOfObjects = JSON.parse(data);
    ```

1. Beneath that, log information on which one you are deleting to the console.  
    Type the following to see the response in the command line interface:

    ```javascript
    console.log("You have requested to delete " + deleteSubscriptionId + ".");
    ```

1. Beneath that, delete the connection by `subscriptionId` by filtering out that `subscriptionId`.  
    Type the followiing to create the new array to write to the `connectionRequests.json` file:

    ```javascript
    const newConnectionsArray = arrayOfObjects.connectionRequests.filter(connectionRequest => connectionRequest.subscriptionId !== deleteSubscriptionId);
    ```  

1. Beneath that, write this array that doesn't have that `subscriptionId` to the read file.  
    Type the write stream in the function:

    ```javascript
    const writer = fs.createWriteStream('connectionRequests.json')
    ```

1. Beneath that, write the string back to the file without that subscriptionId.  
    Type the following to write the new array to the file:

    ```javascript
    writer.write("{\"connectionRequests\":" + JSON.stringify(newConnectionsArray) + "}");
    ```

1. Beneath that, send the response back to the requester.  
    Type the following:

    ```javascript
    res.send("OK");
    ```

You should now be able to delete the request by sending the full request payload back to the delete request endpoint.

### Testing Delete Requests

1. Save all the files.
1. On the command line, stop and start the server.  
    1. Type following to step the server:  
        **Ctrl**+**C**
    1. Type the following to start the server again:  
        **node index.js**.
1. Create a copy of the **Activate Subscription** call in Postman.
1. Rename the copy to **Delete Request**.
1. Change the endpoint from `activate` to `deleteRequest`.
1. Change the body to an existing request that you have.  
    **Note:** You can get an existing request from the `connectionRequests.json` file.
1. Click **Send**.  
    **Note:** You should see the subscription removed from the `connectionRequests.json` file
    and the request message on the command line.

## Creating an Endpoint to Get Your Requests

* Add the endpoint to get the connections from the `connectionRequests.json`.  
    Below `app.post('/deleteRequest'`, type the following function below the `app.options` for the `connectionRequests` endpoint.

    ```javascript
    //Set the connectionRequests.json endpoint
    app.get('/connectionRequests', (req, res)=>{
        fs.readFile('./connectionRequests.json', 'utf-8', function(err, data){
            if (err) throw err
            const arrayOfObjects = JSON.parse(data)
            console.log("You have requested refreshed information.")
            res.header("Content-Type", "application/json")
            res.send(arrayOfObjects)
        })
    })
    ```

## Deploying to Heroku

Please remember that Heroku lets the server sleep after 30 minutes of inactivity.
To keep the server awake, you may want to implement a strategy for waking up your server, such as a cron job.

### Creating a GitHub Repository

You may have to get a personal access token
if you are using your personal account.
See [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to create your token.
You should enter this token instead of your GitHub password
when you are pushing repositories to GitHub.

1. Sign up for a GitHub account at <https://github.com/>.
1. In the upper-left corner, click the **New** button.
1. Type the **Repository name**.
1. Type a **Description**.  
1. Click **Create Repository**.
1. From the command line, type the following to push the repository to GitHub:  
    1. **git init**  
    1. **git add ***  
    1. **git commit -m "Initial Commit"**  
    1. **git remote add origin {your repository}**  
        **Note:** Get the repository name from GitHub.  
    1. **git push -u origin master**
1. Enter your the following when GitHub prompts use:  
    * Your GitHub username.
    * Your GitHub password or your authentication code in the password field.

### Setting up the Port

1. In the `index.js` file after the `/health` endpoint, add these lines to get the app to work on the port the Heroku server assigns to the app:

    ```javascript
    if(process.env.NODE_ENV === "production"){
        app.get('*', (req,res)=>{
            res.sendFile(path.resolve(__dirname, "client", "build"))
        })
    }
    ```

1. Add the following line after that to let Heroku choose the port for the Express app dynamically:

    ```javascript
    const PORT = process.env.PORT || 3000;
    ```

1. Change the `app.listen('3000', '127.0.0.1')` to `app.listen(PORT);`:

    ```javascript
    app.listen(PORT);
    ```

1. Change `console.log('Node server running on port 3000');` to `console.log('Node server running on port '+PORT);`.  

    ```javascript
    console.log('Node server running on port '+PORT);
    ```

### Setting Up the Package Dependencies

1. In the `package.json` file in the root directory after the last script, add a comma, and on the following line, add the Heroku script to build the application.  

    ```json
    "start": "node index.js"
    ```

1. Add the `.gitignore` file in the root directory.
1. Ignore the `node_modules` in your `.gitignore` file so that these are not tracked on the Heroku server.  
    Type the following in the `.gitignore` file:

    ```curl
    node_modules
    ```

1. Create the `.npmrc` file and type the following to ensure the Heroku app is on the public registry.  
    **registry="<https://registry.npmjs.org>"**
1. Specify the npm version in the `package.json` file under the version.  
    Add the `npm` engines and the `node` engines for the deployment under the version in the `package.json` file in the root directory.  
    **Note:** You can find the `node` and `npm` versions you have locally by typing `node -v` and `npm -v` on the command line.

    ```json
    "engines": {
        "npm": "6.9.0",
        "node": "10.15.1"
    },
    ```

1. Save the file.

### Preparing Git for the Heroku Deployment

1. On the command line in the root directory, type the following to remove the previously created modules from the directory:  
    **rm -rf node_modules**  
    **Note:** Heroku creates the node_modules when you deploy the branch.  
1. To remove the node_modules, type the following:  
    **git add node_modules -u**
1. To track the changes to the `.gitignore` file, type the following:  
    **git add .gitignore**
1. To track the changes to the npm registry, type the following:  
    **git add .npmrc**
1. Add your changes with the following:  
    **git add ***
1. Create the commit message for your commit:  
    **git commit -m "{your commit message}"**
1. Push your changes to the repository:  
    **git push**  

### Creating the App in Heroku

1. Create a [Heroku](https://heroku.com) account if you don't already have one.
1. In the upper-right corner, click **New**.
1. Click **Create new app**.
1. Type the app name.  
    **Note:** You must use a unique app name.
1. In Heroku, choose a region.
1. Click **Create app**.

### Integrating Your Heroku App with GitHub

1. In the middle of the page, click **GitHub**.
1. Click **Connect to GitHub** to connect your account to your GitHub account.
1. At the prompt, click **Authorize heroku** to grant access to your GitHub account.
1. Type the repo name.
1. Click **Search**.
1. Click **Connect**.  
1. At the bottom of the page, click **Deploy Branch**.  
    **Note:** You may have to wait a few moments as Heroku downloads the dependencies.

You can now click **View** at the bottom of the page to see your app.
You should see an internal server error at this point,
but you can now type `/health` after that to see `status: UP`.
You now must install the dependencies to get it working locally again.
On the command line, type `npm install`.

## Adding the Admin API URL to Your API

1. Sign in to the [Developer Network]($[devNetworkUrl]).
1. Do the following to select the organization that the API belongs to:
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
    1. Click the organization that the API belongs to.
    1. Click **OK**.
1. Click **View All** next to **APIs**.
1. Click the API you want to add the **Admin API URL** to.
1. Click **Edit** next to **Subscription Activation** to change the method to **Admin API**.
1. Click **Admin API**, and enter the **Admin API URL**.  
    **Note:** Do not include the `/activate` endpoint.
    Fortellis appends the `/activate` endpoint to the URL that you enter.
1. Click **Save**.

## Approving Connection Requests

You need the `connectionId` from the activation request made by Fortellis.
The `connectionId` uniquely identifies the connection in Fortellis' system.

Store the connection request in a database to do the following:

* Verify subscribers.
* Maintain subscription information.
* Improve performance of your API.

Store the connection request in a database to do the following:

* Verify Subscribers.
* Maintain subscription information.
* Improve performance for your API.

1. Use the `/connectionRequests` endpoint to get your connections.
1. Using your Fortellis Client Key and Client Secret, get your Fortellis token from `$[authServerUrl]`.  
    See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) for more information.
1. Create a new request in Postman.
1. Under **Auth**, enter your `access_token`.
1. Use the following URL to accept the connection: `$[subscriptionCallbackUrl]/connections/{connectionId}/callback`.

    ```bash
    curl --location --request POST '$[subscriptionCallbackUrl]/connections/3fc27153-1131-45db-8ffd-c987087055e8/callback' \
    --header 'Content-Type: application/json' \
    --data-raw '{"status": "accepted","error": "string"}' \
    --header 'Authorization: Bearer {your token}'
    ```

    **Note:** You should receive the following back: `OK`.

1. After you approve connections, send the JSON object for the request that you approved to the `deleteRequest` endpoint to remove approved entries.  

## Deactivation Requests

The Subscriber may request to deactivate the subscription for the following reasons:

* The Subscriber is going out of business.
* The Subscriber no longer uses the solution that works with that API.
* The Subscriber wants to change API Developers and moves to a different API.

API Developers may request to have a Subscriber deactivated through Fortellis by contacting [Fortellis Support](mailto:support@fortellis.io).

### Creating the Deactivate Endpoint

API Developers receive the `connectionId` at the `/deactivate` endpoint and can clean up their backend system when they are ready.

1. Copy the `app.post('/activate'` endpoint and paste a copy below it.
1. Remove the validate schema function from it.
1. Change the `/activate` endpoint to `/deactivate/:connectionId`.
1. Change the title in the response from `Activation Request` to `Deactivation Notification`.
1. Delete the references to `req.body` in the body of the function except in the `fs.readfile` code block.
1. Create a new file `deactivationRequests.json`.
1. Include the following JSON object in the file.  
    Type the following to create the `deactivationRequests` object:

    ```javascript
    {"deactivationRequests":[]}
    ```

### Saving the Deactivation `connectionId`

1. Change the file in `fs.readfile` to `./deactivationRequest.json`.  
1. Above the `fs.readfile`, log the `connectionId` to the console.  
    Type the following to log the `connectionId`:  
    **console.log(req.params.connectionId);**  
1. Change `arrayOfObjects.connectionRequests.push(req.body)` to `arrayOfObjects.deactivationRequests.push(req.params)`.
1. Update the write stream to have the `deactivationRequests.json` file.  
    Type the following to change to the `deactivation.json` file:  
    **const writer = fs.createWriteStream('./deactivationRequsts.json');**
1. Send a deactivation request to your `/deactivationRequest` endpoint using Postman or another API tool.

### Getting the Connections to Deactivate

1. Copy the `app.get('/connectionRequests')` function and create a copy underneath it.
1. In the app.get function, change `'/connectionRequests'` to `'/deactivationRequests'`.
1. In the `fs.readfile` function, change `./connectionRequests` to `./deactivationRequests`.
1. Use Postman or another API testing tool to get the connections that have been deactivated.

You can now receive deactivation requests and remove them from your backend systems.
Use the following curl request to get your deactivation requests.

```curl
curl http://localhost:5000/deactivationRequests -H "accept: application/json" \
-H  "Content-Type: application/json"
```

1. Push your repository to GitHub and redeploy to Heroku to get this functionality.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create the `/activate` endpoint.  
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Create a GitHub repository for your project.
* Deploy the endpoint to Heroku.

Store the information you receive when you get an activation request to find subscriptions and other information you need to process requests.
Remember the following about the Heroku deployment:

* The Heroku server sleeps after 30 minutes.
* You may need to implement a more scalable solution or a production environment.

You can download the project from GitHub at [admin-api-implementation](https://github.com/Fortellis/admin-api-implementation).

If you choose to download the example repository, do the following:

1. In the directory of your choice download the repository from GitHub.  
    On the command line, type the following:  

    ```curl
    git clone git@github.com:Fortellis/admin-api-implementation.git
    ```

1. Install the dependencies.  
    On the command line, type the following:  

    ```curl
    npm install
    ```

1. Follow the instructions in [Deploying to Heroku](#deploying-to-heroku).
    1. Follow the instructions for [Creating a GitHub Repository](#creating-a-github-repository) to create your own GitHub repository.  
    1. Create the engines for Heroku.  
        See step 5 in [Setting Up the Package Dependencies](#setting-up-the-package-dependencies) for more information.  
    1. Follow the instructions in [Preparing Git for the Heroku Deployment](#preparing-git-for-the-heroku-deployment) to let Heroku use the `node_modules` it requires.  
    1. Follow [Creating the App in Heroku](#creating-the-app-in-heroku).  
    1. Follow [Integrating Your Heroku App with GitHub](#integrating-your-heroku-app-with-github) to pr
    1. Follow [Adding the Admin API URL to Your API](#adding-the-admin-api-url-to-your-api) to add your URL to your API on Fortellis.
1. Do one of the following:
    * To approve connection requests, follow the steps in [Approving Connection Requests](#approving-connection-requests).
    * To get deactivation requests, follow the steps in [Getting the Connections to Deactivate](#getting-the-connections-to-deactivate).
