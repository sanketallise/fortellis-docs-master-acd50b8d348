---
title: Integrating Your App and an Asynchronous API Producer in Node.js
author: Nathaniel Sandberg
last updated: 3/24/2022
---

You can receive information from an event with webhooks.
Create a webhook to receive the events and then process those on your backend or display the information in a UI.
With webhooks, you can receive events and keep the information in your app from becoming stale.

## Tutorial

### What You Will Learn

In this tutorial, you should learn how to do the following:

* Set a `health` endpoint for your app.
* Receive a call from an asynchronous API producer.
* Store the call from the asynchronous API producer for processing later.

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

### Creating the NPM Project

1. Create a new directory for the project.  
    Type the following on the command line:

    ```curl
    mkdir {your directory name}
    ```

1. Change to the new directory.  
    Type the following:  

    ```curl
    cd {your directory name}
    ```

1. Initialize the npm project.  
    Type the following on the command line:  

    ```curl
    npm init
    ```

1. Press enter on each prompt to use the default value.  
    **Or**  
    Type your custom value at the prompt.  

### Creating the Health Check Endpoint

1. Install express.  
    Type the following:  

    ```curl
    npm install express
    ```

1. Create the `index.js` file.  
    Type the following:  

    ```curl
    touch index.js
    ```

1. Require Express in the `index.js` file.  
    Type the following at the top of the file:

    ```javascript
    const express = require('express');
    ```

1. Create the app.  
    Type the following under the express constant:

    ```javascript
    const app = express();
    ```

1. Include the code to start the server at the bottom of the file.

    ```javascript
    // Start the server on port 3001
    app.listen(3001, '127.0.0.1');
    console.log('Node server running on port 3001');
    ```

1. Between the constant and app.listen, create the response for the health check.  
    Type the following in the `index.js` file:  

    ```javascript
    app.get('/health', (req, res)=>{res.json({"status":"Up"})});
    ```

1. Run the server to check that it is up.  
    On the command line, type the following:  

    ```curl
    node index.js
    ```

```javascript
const express = require('express');

const app = express();

app.get('/health', (req, res)=>{res.json({"status":"Up"})});

app.listen(3001, '127.0.0.1');
console.log('Node server running on port 3001');
```

You can now go to `localhost:3001/health` in your browser to see the status of the server.
Press **Ctrl**+**C** to stop the server.

### Creating the Endpoint to Receive the Event

1. Log the request in the console.  
    Beneath the constants, type the following in the `index.js` file:

    ```javascript
    app.post('/hello/world/event', function(req, res){
        console.log(req.body);
        res.status(202);
    });
    ```

    > Fortellis appends the events endpoint to the URL that you enter when you are registering your webhook endpoint.

1. Install the body-parser to evaluate incoming requests.  
    On the command line, type the following:

    ```javascript
    npm install body-parser
    ```

1. In the `index.js` file, declare the constant for the body-parser.  
    Type the following with the other constants at the top of the file:

    ```javascript
    const bodyParser = require('body-parser');
    ```

1. In the constants, also include **app.use(bodyParser.json({extended:true}), express.json());**.

    ```javascript
    const express = require('express');

    const app = express();

    const bodyParser = require('body-parser');

    app.use(bodyParser.json({extended:true}), express.json());

    app.post('/hello/world/event', function(req, res){
        console.log(req.body);
        res.status(202);
        res.send('');
    });

    app.get('/health', (req, res)=>{res.json({"status":"Up"})});

    app.listen(3001, '127.0.0.1');
    console.log('Node server running on port 3001');
    ```

1. Type the following on the command line to start the Express server:

    ```curl
    node index.js
    ```

Now when you send a request to the local host at `http://localhost:3001/hello/world/event`,
you get a response with a  `202 Accepted` code and the original call.
The `Accepted` response tells Fortellis
that you received the message,
even if you don't process it immediately.

```bash
curl --location --request POST 'http://localhost:3001/hello/world/event' \
--header 'Type: application/json' \
--header 'Content-Type: application/json' \
--data-raw '{
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

Press **Ctrl**+**C** on the command line to stop the local server.

### Saving the Events

1. With the other constants in the `index.js` file, import the `fs` dependency.  
    Type the following:

    ```javascript
    const fs = require('fs');
    ```

1. Create a `queue.json` file to save the events to while you wait to have the bandwidth to process them.  
    On the command line, type the following:  

    ```curl
    touch queue.json
    ```

1. Create the code within the post to write to a JSON file.  
    Enter the following code beneath the accept response `res.send('');` in the `app.post('/hello/world/event'...` function:  

    ```javascript
    fs.readFile('./queue.json', 'utf-8', function(err, data){
        if(err) throw err
        const arrayOfObjects = JSON.parse(data);
        arrayOfObjects.queue.push(req.body);
        console.log(arrayOfObjects);
        const writer = fs.createWriteStream('./queue.json');
        writer.write(JSON.stringify(arrayOfObjects));
    });
    ```

    **Note:** You now save the events that you receive at `localhost:3001/hello/world/event` to the `queue.json` file.

1. Put the following object in the `queue.json` file.

    ```json
    {"queue":[]}
    ```

Now, you have all the data going into the `queue.json` file
where you can get all your events and process them.

If you send the request now,
you will see the `queue.json` file populate with the information that you sent.

### Integrating Your App with Your Asynchronous API Producer

If you followed the previous tutorial on [Creating Your Asynchronous API Producer in NPM](/docs/tutorials/event-relay/tutorial-develop-an-async-api),
you can now do the following:

1. Change the URL in your asynchronous API producer from `https://webhook.site/{yourUUID}` to your local environment endpoint `http://localhost:3001/hello/world/event`.
1. Start both the asynchronous API producer server and the asynchronous API consumer server.
1. Change the payload in the asynchronous API producer.
1. See the update in the app.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Set a `health` endpoint for your app.
* Receive a call from an asynchronous API producer.
* Store the call from the asynchronous API producer for processing later.

You can find the completed project on GitHub at [localAsyncAPIConsumer](https://github.com/Fortellis/localAsyncAPIConsumer).

In the next tutorial, you can see how to register your URL on Fortellis to receive requests from Asynchronous API Developers.
