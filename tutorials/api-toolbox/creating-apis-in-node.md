---
title: Creating APIs in Node.js
author: Nathaniel Sandberg
last updated: 1/28/2021
---

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create a simple API in Node.
* Verify tokens sent from Fortellis.

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

You can find a completed project with the example on GitHub: [Get Car Inventory API](https://github.com/Fortellis/getCarInventory).

## Creating Node Project

**Prerequisites:** For this tutorial, you should have Node.js and npm already installed.
Go to [Node.js](https://nodejs.org/en/download/) to find the latest download.

1. Select or create the spec that you are going to implement.  
    You can download the [Example Spec](https://github.com/Fortellis/example-spec/blob/master/testing2.yaml) that has one API endpoint.  
    **Note:** You want to create relatively discrete APIs that Apps can consume with little effort.
1. Create a new folder where we are going to put the application for development locally.  
    **Note:** For this tutorial, we are going to use the folder `getCarInventory`.
1. Open a command-line interface in the folder you just created.
1. Create an npm project.  
    In the project directory on the command line, type the following to initialize the project.

    ```curl
    npm init
    ```

1. Fill out the information that npm prompts for.  
    **Note:** To use the default values, press **Enter** at each one of the prompts.
    * package name
    * version
    * description
    * entry point: (index.js)
    * test command
    * git repository
    * keywords
    * author
    * license: (ISC)
1. Accept the contents of the file.  
    Type the following on the command line,
    and then press **Enter**.

    ```curl
    yes
    ```

## Creating the Express App

1. Install express.  
    On the command line, type **npm install express**.
1. Create an `index.js` file.  
    On the command line, type the following:

    ```curl
    touch index.js
    ```

1. Require the express app in your `index.js` file.  
    At the top of the `index.js` file, type the following:

    ```curl
    const express = require("express");
    ```

1. Create the express app and the listener for the express app.  
    Under the express constant, type the following:

    ```javascript
    const app = express();

    app.listen(3000, "127.0.0.1");
    ```

1. Create the log to show that the app is running on the command line.  
    At the bottom of the `index.js` file, type the following:

    ```curl
    console.log("Node server running on port 3000");
    ```

1. Create the `/health` endpoint to check that the server is running.  
    Between the **const app = express();** and **app.listen(3000, "127.0.0.1");**, copy and paste the following code in the `index.js` file:

    ```javascript
    app.get("/health", (req, res) => {res.json({ status: "Up" });});
    ```

1. To start the server from the command line, type the following:

    ```curl
    node index.js
    ```

You can now do one of the following to check that the endpoint is working:

* Run a get request in your favorite API testing solution at the `localhost:3000/health` endpoint.
* Go to `localhost:3000/health` and see the response in your browser.

## Creating the Inventory Endpoint and Verifying the Token

> You may not need to verify the Fortellis authorization token
> if you choose one of the following options
> when you register your app:
>
> * External OAuth 2.0.
> * Custom OAuth 2.0.
> * Basic authorization.
> * API Key and Secret.
>
> Verify your token with the correct method
> if you choose authorization other than Fortelis OAuth 2.0.

1. Install the okta dependency.  
    On the command line, type the following:

    ```curl
    npm install @okta/jwt-verifier
    ```

1. Import the const for the jwt verifier, and create the constant for the URL for the Fortellis tokens.
    Under the constant `const app`, type the following to get the jwtVerifier in the `index.js` file:

    ```javascript
    const JwtVerifier = require("@okta/jwt-verifier");
    ```

1. Create the constant to reference to verify the token.  
    Under the `JwtVerifier` constant, type the following:

    ```javascript
    const jwtVerifier = new JwtVerifier({
        issuer: `$[authServerUrl]`,
        assertClaims: {
            aud: "api_providers",
            sub: "{yourClientID}"
        },
    });
    ```

1. Use the `verifyToken` function to check that the token is valid.  
    At the bottom of the `index.js` file, type the following to check that the token is valid and that the `Subscription-Id` matches your active apps:  
    **Note:** You would want a more robust solution for matching the `Subscription-Id` in your API.

    ```javascript
    //Verify the token in this request.
    function verifyToken(req, res, next) {
        //Get the authorization header in the request.
        const bearerHeader = req.headers["authorization"];
        //Check if the request has the authorization header.
        if (bearerHeader&&req.headers["subscription-id"]==="7e465be1-4fef-4840-8b84-a95ab4c74f0c") {
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

1. Create the dependency requirement to read from files in the `index.js` file.  
    Below the constant for the `app` at the top of the `index.js` file, type the following to require fs:  

    ```curl
    const fs = require("fs");
    ```

    **Note:** You get this dependency from Node.js.

1. Create the `inventory.JSON` file in the same folder as your `index.js` file.  
    Enter the following information in the `inventory.JSON` file:

    ```JSON
    {
        "inventory": [
            {
                "dealerGroupName": "CDK Dealer Group",
                "dealerships": [
                    {
                        "city": "Chicago",
                        "state": "Illinois",
                        "name": "Chicago Chevy",
                        "inventory": {
                            "cars": [
                                {
                                    "make": "Chevy",
                                    "model": "Nova",
                                    "color": "green",
                                    "year": "2020",
                                    "price": 3000,
                                    "financingAvailable": true
                                },
                                {
                                    "make": "Chevy",
                                    "model": "Camaro Z28",
                                    "color": "Black",
                                    "year": "1979",
                                    "price": 1500,
                                    "financingAvailable": true
                                }
                            ],
                            "trucks": [
                                {
                                    "make": "Chevy",
                                    "model": "Silverado",
                                    "color": "black",
                                    "year": "2018",
                                    "price": 30000,
                                    "financingAvailable": true
                                },
                                {
                                    "make": "Chevy",
                                    "model": "Canyon",
                                    "color": "yellow",
                                    "year": "2020",
                                    "price": 100000,
                                    "financingAvailable": false
                                }
                            ]
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Create the endpoint for the inventory.  
    In the `index.js` file, place the following code beneath the `/health` endpoint:

    ```javascript
    //Create the endpoint to use the get method and include the function to verify the token.
    app.get("/inventory", [verifyToken], (req, res) => {
        //Include the jwtVerifier and the call to verify the token along with the information on the audience of api_providers.
        jwtVerifier.verifyAccessToken(req.token, "api_providers").then(jwt => {
            //Read from the inventory.JSON file.
            fs.readFile("./inventory.JSON", "utf-8", function(err, data) {
                //Throw an error if you encounter any.
                if (err) throw err;
                //Parse the data in the inventory.
                let inventory = JSON.parse(data);
                console.log(inventory.inventory[0]);
                //Create the HATEOAS links.
                const HATEOASLinks = {
                    links: [
                    {
                        href: "localhost:3000",
                        rel: "self",
                        method: "GET",
                        title: "Get Inventory",
                    },
                    {
                        href: "localhost:3000",
                        rel: "POST",
                        method: "POST",
                        title: "Post inventory",
                    },
                    ],
                };
                // Push the HATEOAS links to the inventory that you return from the API.
                inventory = { inventory, ...HATEOASLinks};
                //Set the content type for the response.
                res.set("Content-Type", "application/json");
                //Send the response back to the client.
                res.send(inventory);
            });
        });
    });
    ```

## Testing the Endpoint

You can now call the inventory endpoint with a token to get the data from the JSON file.

1. To stop the Node.js server, click on the command line and press **Ctrl**+**C**.
1. To start the server again, type the following on the command line:

    ```curl
    node index.js
    ```

1. Use cURL or another API testing tool to test your endpoint.  
    **Note:** Remember to send your request with a new token.
    See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) for more information on acquiring one.

### Example cURL Request

```JSON
curl --verbose -X GET  -H 'Subscription-Id:7e465be1-4fef-4840-8b84-a95ab4c74f0c' -H 'Accept: application/json' -H 'Content-Type: application/json' -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiI1ajdsMGNmb1ZtUHpIcVc2elJIakduQXhxR3lBemxNdSIsImp0aSI6IkFULmcyWDl3Qnp3SF9aeEJuUVJXOXhXcDZsQk9sVW1hRURHZTdDdlM3dWxjclEiLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IjVqN2wwY2ZvVm1QekhxVzZ6UkhqR25BeHFHeUF6bE11IiwiaWF0IjoxNTk5NjY0MzE3LCJleHAiOjE1OTk2Njc5MTcsImF1ZCI6ImFwaV9wcm92aWRlcnMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.sMyx6-ISqWLZXp96mwiHrSdPkE9NTSZGIWRpO9mHL0f_RO3f4sS1VR4Pt3Q-oV9h7ElxRPvA0ivQcn2Q0bXX8N1bQSrfQWdelxqKToYfeindp8ZfOFLgVxejpHmRMFpHC8AZv-3jBSl_RbHRjIHM70WMDrbYBR_GDQ4kDTD9iq3FD7-2yeV-gui5eMbGshlNvBUJIsCjT0MrCsnBT3eM0gq171x7s7t_STMq4G2TeHnJ_dY-6s1mQBV4vh5Iz3IkFzv8z4Wb2zWBH10NA1XYCkuXCrOuobhjJXxiHQoW-aPytXF6rivn3N553IqpUmJ_5VCq_ktDoZJiFwyWFJMLVQ" http://localhost:3000/inventory
```

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create a simple API.
* Verify tokens sent from Fortellis.

You can download the project from GitHub at [getCarInventory](https://github.com/Fortellis/getCarInventory).

If you download the example repository, do the following:

1. Download the GitHub project in the directory of your choice.  
    On the command line, type the following:  

    ```curl
    git clone git@github.com:Fortellis/getCarInventory.git
    ```

1. Change directories to the downloaded project.  
    On the command line, type the following:  

    ```curl
    cd getCarInventory
    ```

1. Install the dependencies.  
    On the command line, type the following:  

    ```curl
    npm install
    ```

1. Start the server.  
    On the command line, type the following:  

    ```curl
    node index.js
    ```

1. Request a token.  
    On another command line, type the following to request a token:  

    ```curl
    curl -X POST --url $[authServerUrl]/v1/token \
    --header 'accept: application/json' \
    --header 'authorization: Basic Base64Encoded{yourClientID:yourClientSecret}' \
    --header 'Cache-Control: no-cache' \
    --data 'grant_type=client_credentials&scope=anonymous'
    ```

1. Make requests to your endpoint with a token.  
    On another command line, type the following to send the request and press enter:  
    **Note:** You must use the correct `Subscription-Id` and a recently created token or you receive a forbidden error.  

    ```curl
    curl --verbose -X GET  -H 'Subscription-Id:7e465be1-4fef-4840-8b84-a95ab4c74f0c' -H 'Accept: application/json' -H 'Content-Type: application/json' -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiI1ajdsMGNmb1ZtUHpIcVc2elJIakduQXhxR3lBemxNdSIsImp0aSI6IkFULmcyWDl3Qnp3SF9aeEJuUVJXOXhXcDZsQk9sVW1hRURHZTdDdlM3dWxjclEiLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IjVqN2wwY2ZvVm1QekhxVzZ6UkhqR25BeHFHeUF6bE11IiwiaWF0IjoxNTk5NjY0MzE3LCJleHAiOjE1OTk2Njc5MTcsImF1ZCI6ImFwaV9wcm92aWRlcnMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.sMyx6-ISqWLZXp96mwiHrSdPkE9NTSZGIWRpO9mHL0f_RO3f4sS1VR4Pt3Q-oV9h7ElxRPvA0ivQcn2Q0bXX8N1bQSrfQWdelxqKToYfeindp8ZfOFLgVxejpHmRMFpHC8AZv-3jBSl_RbHRjIHM70WMDrbYBR_GDQ4kDTD9iq3FD7-2yeV-gui5eMbGshlNvBUJIsCjT0MrCsnBT3eM0gq171x7s7t_STMq4G2TeHnJ_dY-6s1mQBV4vh5Iz3IkFzv8z4Wb2zWBH10NA1XYCkuXCrOuobhjJXxiHQoW-aPytXF6rivn3N553IqpUmJ_5VCq_ktDoZJiFwyWFJMLVQ" http://localhost:3000/inventory
    ```

You receive a response back from the API with the contents of the `inventory.JSON` file.
