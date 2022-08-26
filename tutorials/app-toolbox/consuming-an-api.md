---
title: Consuming APIs within Your App
author: Nathaniel Sandberg
last updated: 10/25/2021
---

To get started on development, we are going to create an app that works with an API.
With this tutorial, you can start working on your app and creating great user experiences.

> For this tutorial, we use express to proxy our requests from the react app.
> If you do not use an express app,
> make sure to prevent CORS errors.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create an Express server in node.
* Create a React app that uses the Express server.
* Install the Bootstrap dependency.
* Create a button in react.
* Create the call and proxy off the express server.
* Get your basic authorization token.
* route your request to the API.
* Display the information in the UI of your React app.

You can following the instructions at the end of the tutorial to download the repository from GitHub: <https://github.com/Fortellis/Consuming-APIs-within-Your-App>.

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

## Creating the Express Server to Use as a Proxy

1. In the directory of your choice, create the directory for the app that you are going to create.  
    On the command line, type **mkdir {Your Project Name}**.
1. Change directories to the one you just created.  
    On the command line, type **cd {Your Projects Name}**.
1. Initialize the project to create the `package.json` file and the `node_modules` folder.  
    On the command line, type **npm init**.  
    **Note:** To accept the default values, press enter on each of the fields, or you can also customize these for your needs.
1. Add the express dependency.  
    On the command line, type **npm install express**.
1. Create the `index.js` file.  
    On the command line, type **touch index.js**.
1. Enter the following text in the `index.js` file:

    ```javascript
    const express = require('express');
    const path = require('path');

    const app = express();

    // Serve the static files from the React app
    app.use(express.static(path.join(__dirname, 'client/build')));

    // Handles any requests that don't match the ones above
    app.get('/', (req,res) => {
        res.sendFile(path.join(__dirname+'/client/build/index.html'));
    });

    const port = process.env.PORT || 5000;
    app.listen(port);

    console.log('App is listening on port ' + port);
    ```

1. Add the start script to the `package.json` file.

    ```json
    {
        "name": "react-express-example",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
            "test": "Testing",
            "start": "node index.js"
        },
        "keywords": [],
        "author": "",
        "license": "MIT",
        "dependencies": {
            "express": "^4.16.3"
        }
    }
    ```

You can now type **npm run start** on the command line to start the server and see `App is listening on port 5000`.
If you need to stop the server at any point,
press **Ctrl**+**C**.

## Creating the React App in the Express Directory

1. Install the react app globally if you do not already have it installed.  
    On the command line, type **npm install -g create-react-app**.
1. Create the react app.  
    On the command line, type **npx create-react-app client**.  
    **Note:** You create the react app in the client folder in the project folder using this command.
1. Add the proxy line to the `/client/package.json` file.  
    Under the scripts in the `/client/package.json` file, add `"proxy": "http://localhost:5000"` and include a comma.
1. Install the concurrently dependency so that you can run both the react app and the express app at the same time.  
    On the command line, type **npm install concurrently**.
1. Add the extra scripts that we are going to need to get both the apps running at the same time.  
    In the `package.json` file in the root directory, enter a comma after the existing scripts and then add the following scripts:

    ```json
    "client-install": "npm install --prefix client",
    "server": "node index.js",
    "client": "npm start --prefix client",
    "dev": "concurrently \"npm run server\" \"npm run client\""
    ```

1. Start the express and react server.  
    On the command line, type **npm run dev**.  
    **Note:** You can now do the following:
    * Open the react app in the browser at `localhost:3000`.
    * See the react app running on port 3000 on the command line.
    * See the express server running on port 5000 on the command line.
    * Update the `App.js` file.
    * See your changes to `App.js` hot load in the react dev environment.

## Installing the Bootstrap Dependency

1. Change directories to the client directory.  
    On the command line, type **cd client**.
1. Install the bootstrap dependency.  
    On the command line, type **npm install react-bootstrap bootstrap**.
1. Add the import of the button from React Bootstrap at the top of the `App.js` file after the other constants.  

    ```javascript
    import Button from 'react-bootstrap/Button';

    import 'bootstrap/dist/css/bootstrap.min.css';
    ```

## Creating the Button

1. Add the button to the `App.js` file after the `<a>` tag.  

    ```javascript
    <Button variant="primary">
        Call the API.
    </Button>
    ```

1. Install axios in the client directory.  
    On the command line, type **npm install axios**.
1. Add the constant for axios
    after the import statements in `App.js`.  

    ```javascript
    const axios = require('axios');
    ```

1. Create the function in the `function App` in the `App.js` file to call the express app that forwards the call to the API.  
    > For this tutorial, we are going to use the [Vehicle Specifications API](https://apidocs.fortellis.io/specs/2a82adcd-570f-41de-bc4c-9f5364bcc583),
    > but you can repurpose these instructions for any API
    > that you would like to use.

    ```javascript
    const makeAPIRequest = (e) => {
        e.preventDefault();
        const requestParameters = {
            yourParameters: "{yourName}"
        };

        axios
            .get('test', requestParameters)
            .then(response => {
                console.log(response.data);
            })
            .catch(err => {
                console.log(err);
            })
    }
    ```

1. Add the function to your button in the `App.js` file.  
    In the `<Button>`, type **onClick={makeAPIRequest}**.

    ```javascript
    <Button variant="primary" onClick={makeAPIRequest}>
        Call the API.
    </Button>
    ```

1. Add the request endpoint to the `index.js` file in the root directory.  
    After the `app.get` that serves the root path, add the following code to send the request to the API.  

    ```javascript
    app.get("/test", (req, res) => {
        console.log("You have received a request.");
        res.send("We have received your request.");
    });
    ```

1. Change directories to your root directory.  
    On the command line, type the following:

    ```curl
    cd..  
    ```

    **Note:** You can now do the following:
    1. Run the app with the following command:

        ```curl
        npm run dev
        ```

    1. Open the console in the developer tools.
    1. Click the button.
    1. View the following:
        * See the request "You have received a request." on the command line.
        * See the response "We have received your request." in the console.

## Creating the Call and Proxy Off the Express Server

1. Install the body-parser dependency in the root directory.  
    On the command line, type the following:

    ```curl
    npm install body-parser
    ```

1. Under the other constants in the `index.js` file, enter the following code to use the `body-parser` dependency.

    ```javascript
    const bodyParser = require('body-parser');
    app.use(bodyParser());
    ```

1. Change the line `You have received a request.` to the following:  

    ```javascript
    console.log("You have received a request: " + JSON.stringify(req.headers));
    ```

1. In the `App.js` file, update the `const requestParameters` variable to include all the variables that you will need to use your API.  
    You need the following parameters and any required by that particular API:  
    * `SubscriptionId`  
        * Use the `test` Subscription ID for our example.  
    * `RequestId`  
        * Use any UUID that you like.  
    * `Authorization`  
    **Note:** You must add the API to the app in your developer account before you can access the test data.  

## Getting Your Basic Authorization Token

1. Sign in to [developer.fortellis.io]($[devNetworkUrl]).  
1. Do the following to select the organization that the app belongs to:  
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.  
    1. Click the organization that the app belongs to.
    1. Click **OK**.  
1. Do the following to find the **Credentials** for your app:
    1. Hover over your name in the upper-right corner.  
    1. Click **Developer Account**.  
    1. To view all the apps, click **View All**.  
    1. Click on the app that you are working with.  
    1. Save your `API Key` and `API Secret`.  
1. If you haven't added the API to the app already,
    follow [Updating the App](/docs/tutorials/app-lifecycle/registering-apps/#updating-the-app) to add it.  
    **Note:** In this tutorial, we use the Vehicle Specification API.
1. [Base64 encode](https://www.base64encode.org) `{your API Key}:{your API Secret}`.  
1. Enter your `Basic` authorization in the `requestParameters` below.  
    **Note:** We use `test` for the `Subscription-Id` and your basic authorization in this tutorial to get test data back from the simulator.

    ```javascript
    const requestParameters = {
        headers: {
            Authorization: "Basic {Your Basic Authorization}",
            SubscriptionId: "test",
            RequestId: "4a0eaa11-6bee-4da9-8b76-12daf5e7d82e"
        }
    };
    ```

## Routing the Request to the API

1. Add the following code to the `GET` request in the `index.js` file.

    ```javascript
    app.get("/test", (req, res) => {
        console.log("You have received a request: " + JSON.stringify(req.headers));
        console.log(req.headers.authorization);
        console.log(req.headers.subscriptionid);
        console.log(req.headers.requestid);
        res.send("We have received your request.");
    });
    ```

1. Install the axios dependency in the Express app.  
    On the command line, type the following:  

    ```curl
    npm install axios
    ```

1. In the `index.js` file, add the constant for the axios dependency beneath the last constant at the top of the file.  

    ```javascript
    const axios = require('axios');
    ```

1. Send the request to the simulator in the `index.js` file in the root directory.  
    In the `index.js` file, delete `res.send("We have received your request.");` and replace it with the following:

    ```javascript
    const headersForAPI = {
        headers:
            {
                Authorization: req.headers.authorization,
                "Subscription-Id": req.headers.subscriptionid,
                "Request-Id": req.headers.requestid
            }
    };

    axios
        .get("$[apiFortellisUrl]/vehicles/reference/v4/vehicle-specifications/vins/{vin}",
        headersForAPI)
        .then(response => {
            console.log(response.data);
            res.send(response.data);
        })
        .catch(err => {
            console.log(err)
        })
    ```

> To see the response on the command line and in the console in your browser,
> start the server with **npm run dev** on the command line,
> and press the button.

## Displaying the Information in the UI

1. Add the code to use state in the functional component.  
    Add the `import { useState } from "react";` import before the constant that requires axios:

    ```javascript
    import { useState } from "react";

    const axios = require("axios");
    ```

1. Add the state to the `App.js` file.  
    Add the following lines to create a state for the function and the replacement state in the `App` function:

    ```javascript
    const [responseFromAPI, setResponseFromAPI] = useState("");
    ```

1. Set the state to the response data under where you log the data in the console.  

    ```javascript
    setResponseFromAPI(response.data.vehicleSpecification);
    ```

1. Add the code to show the JSON content in a stringified format below the button.

    ```javascript
    <p>
        {JSON.stringify(responseFromAPI)}
    </p>
    ```

You can now do the following:

* Start the app
* Click the button to see the data that the API returns

You need to take these additional steps to create a working app:

* Get real Bearer tokens that Fortellis will accept in production
* Get a real `Subscription-Id` to use in the production environment

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create an Express server in node.
* Create a React app that uses the Express server.
* Install the Bootstrap dependency.
* Create a button in react.
* Create the call and proxy off the express server.
* Get your basic authorization token.
* Route your request to the API.
* Display the information in the UI of your React app.

### Starting from the GitHub Project

#### Downloading from GitHub and Installing Dependencies

1. Download the repository from GitHub.  
    On the command line in the directory of your choice, type the following to clone the repository:

    ```curl
    git clone git@github.com:Fortellis/Consuming-APIs-within-Your-App.git
    ```

1. Change directories to the project directory.  
    On the command line, type the following to access the project directory.  

    ```curl
    Consuming-APIs-within-Your-App
    ```

1. Install the dependencies.  
    On the command line, type the following:

    ```curl
    npm install
    ```

1. Change directories to the client folder.  
    On the command line, type the following:

    ```curl
    cd client
    ```

1. Install the dependencies in the client folder.  
    On the command line, type the following:

    ```curl
    npm install
    ```

1. Change directory from the `client` folder to the root folder.  
    On the command line, type the following:  

    ```curl
    cd ..
    ```

#### Registering the App through Marketplace

1. Sign in to [Marketplace]($[marketplaceUrl]).
1. Click **Sell on Marketplace**.
1. Click **Create an App**.
1. Follow the instructions on [Submitting Apps to Marketplace](/docs/tutorials/solution-integration/launch/#publishing-you-app-to-marketplace) to complete the registration process using the [Vehicle Specifications API]($[]).
1. Get your App credentials, as described in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth).
1. To use basic authorization, [base 64 encode](https://www.base64decode.org) your {APIKey:APISecret}.  
1. Replace the value for the authorization header in the `client/src/App.js` with your value.  
1. Start the project.  
    On the command line, type the following:  

    ```curl
    npm run dev
    ```

You need to use a Fortellis Bearer token to make calls to the API when it's hosted in one of the Fortellis environments (Test, Dev, or Production).
