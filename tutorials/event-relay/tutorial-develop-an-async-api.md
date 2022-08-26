---
title: Developing an Asynchronous API in Node.js
author: Nathaniel Sandberg
last updated: 1/20/2022
---

$[eventRelayIntroductionBoilerPlate]

## What You Will Learn in This Tutorial

$[eventRelayBoilerPlateWhatYouWillLearn]

## Things to Remember in This Tutorial

You should remember the following:

* Use **node index.js** to start the server.
* Use **Ctrl**+**C** to stop the server.
* Install the following before starting the tutorial:
    * Node
    * NPM

For this tutorial, we also use [https://webhook.site](https://webhook.site) so you don't have to set up a web tunnel,
but you can set up the receiving endpoint however you would like to.

You can follow the instructions at the end to download the completed project and get a working copy from GitHub: <https://github.com/Fortellis/AsyncAPIHelloWorld>.

## Creating the NPM Project

1. From the command line, create a new directory for the project.  
    Type the following:

    ```curl
    mkdir {your directory name}
    ```

1. Change to the directory you just created.  
    Type the following:

    ```curl
    cd {your directory name}
    ```

1. Initialize the npm project.  
    Type the following:

    ```curl
    npm init
    ```

    **Note:** Enter any custom values for the fields for your project,
    or press enter when prompted to select the default.
1. Create the `index.js` file.  
    Type the following:

    ```curl
    touch index.js
    ```

## Creating the Express Server

1. Install the express dependency.  
    Type the following:  

    ```curl
    npm install express
    ```

1. Require express in the `index.js` file.  
    On the first line in `index.js`, type the following:  

    ```javascript
    const express = require('express');
    ```

1. Create the app beneath that.  
    Type the following:  

    ```javascript
    const app = express();
    ```

1. At the bottom of the file, include the code to start the server.  

    ```javascript
    // Start the server on port 5000
    app.listen(5000, '127.0.0.1');
    console.log('Node server running on port 5000');
    ```

## Creating the Payload to Send to the Webhook

1. Create the `payload.json` file.  
    On the command line, type the following:

    ```curl
    touch payload.json
    ```

1. Create the contents of the `payload.json` file.  

    ```json
    {
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
    }
    ```

1. Import fs into the `index.js` file.  
    Beneath the other constants, type the following:  

    ```javascript
    const fs = require('fs');
    ```

1. Enter the following code below the constants in the `index.js` file:

    ```javascript
    fs.readFile('./payload.json', 'utf-8', function(err, data){
        const arrayOfObjects = JSON.parse(data);
        console.log(arrayOfObjects);
    });
    ```

## Sending the File Contents with Express

Now, we send the payload with the express app
when the file changes.

1. Watch the file for changes
    and log the result to the console every time the `payload.json` file changes.  
    Enter the following around the `fs.readFile` function:  

    ```javascript
    fs.watchFile('./payload.json', function(event, filename){
        console.log('event is ' + event);
        if(filename){
            console.log('filename provided: ' + filename);
        //fs.readFile code...
        }else {
            console.log('filename not provided');
        }
    });
    ```

    **Note:** You are now logging everything to the console
    whenever you change the file and save it.
    We are now going to send this to our endpoint.

1. Add request to the constants at the top of the file.  
    Type the following:  

    ```javascript
    const request = require('request');
    ```

1. Add the function to send the request within the `readFile` function beneath the `arrayOfObjects` constant
    and trigger the function.  
    **Note:** Get your webhook URL from <https://webhook.site>.

    ```javascript
    //arrayOfObjects code...
    const sendUpdatedData = () => {
        const serverOptions = {
            uri: '{yourWebhookURL}',
            body: JSON.stringify(arrayOfObjects),
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            }
        };
        request(serverOptions, function (error, response){
            console.log(error);
            return;
        });
    }
    sendUpdatedData();
    //else catch for not providing a file name...
    ```

1. Start the server.  
    Type the following in the console:

    ```curl
    node index.js
    ```

1. Update the payload file with valid JSON data.  
    Replace the existing JSON object with the following:

    ```json
    {
        "id": "1541b98b-ec92-4428-ad96-9300ac0be2b6",
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
            "language": "French",
            "id": "d8521586-066f-4326-9468-5dcba89a324b"
        }
    }
    ```

You can now update the `payload.json`,
and the Node.js server sends the updates to the file to the webhook endpoint.

> Change the `payload.json` file and save it to send the updated information.

View the updates at your <https://webhook.site>.

**Note:** The webhook site may change your URL sometimes.
Double-check the URL at <https://webhook.site>
before you run the Node.js server if you haven't used it recently and replace the UUID in the URL of your `index.js` if needed with the new UUID
that the <https://webhook.site> assigned to you.

## Next Steps

This tutorial should have shown you how to do the following:

* Create an asynchronous API producer that runs locally.
* Create a simple model to update your Subscribers with new information.
* Have the asynchronous API producer send the payload to an endpoint.
* Use a webhook service to test your asynchronous API producer.

Go to the next tutorial to find out how to publish your asynchronous API producers to Fortellis.

You can find the complete project on GitHub: [AsyncAPIHelloWorld](https://github.com/Fortellis/localAsyncAPIProducer).

To use the project from GitHub, do the following:

1. Clone the repository.  
    On the command line in the folder of your choice, type the following:  

    ```curl
    git clone git@github.com:Fortellis/AsyncAPIHelloWorld.git
    ```

1. Change directories to get to the new folder.  
    On the command line, type the following:  

    ```curl
    cd asyncAPIImplementationExample
    ```

1. Install the dependencies for npm.  
    On the command line, type the following:  

    ```curl
    npm install
    ```

1. Update the URL to your webhook.  
    In the `index.js`, replace `{yourWebhookURL}` with the URL you get from <https://webhook.site> to receive requests.
1. Start the server.  
    On the command line, type the following:

    ```curl
    node index.js
    ```

1. Send the event.  
    Change the contents of the `payload.json`.

You can now see the payload at your webhook site.
