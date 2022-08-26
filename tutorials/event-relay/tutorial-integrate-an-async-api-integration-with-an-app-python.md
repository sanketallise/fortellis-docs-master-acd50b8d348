---
title: Integrating Your App and an Asynchronous API Producer in Python
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

* Use **python app.py** to start the script.
* Use **Ctrl**+**C** to stop the script.
* Install the following before starting the tutorial:
    * Python
    * A command-line interface
    * A text editor

### Creating the Python Project

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

1. Create the Python environment.  
    On the command line, type the following:  

    ```curl
    pip3 install virtualenv
    ```

1. Update the Python interpreter you are using.  
    On the command line, type the following:  

    ```curl
    virtualenv -p python3.9 venv
    ```

1. Activate the environment.  
    On Windows, type the following on the command line:  

    ```curl
    source venv/Scripts/activate
    ```

    **Or**  
    On Mac, type the following on the command line:  

    ```curl
    source venv/bin/activate
    ```

You can now see `(venv)` indicating that your environment is running.
To stop the environment, type **deactivate** at any time.

### Creating the Health Check Endpoint

You do not have to deactivate Python to complete the following steps.

1. Install Flask.  
    On the command line, type the following:  
    **pip install flask**.
1. Install Flask Restful.  
    On the command line, type the following:  
    **pip install flask-restful**
1. In your favorite text editor, create the `app.py` file in the `python-admin-api` folder with the following content:

    ```python
    from flask import Flask, request
    from flask_restful import Resource, Api, reqparse, abort

    app = Flask(__name__)
    api = Api(app)

    class Health(Resource):
        def get(self):
            return {'status': 'up'}

    api.add_resource(Health, '/health')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

    You now have a `health` endpoint that you can use to test your API endpoint.
1. Start the app.  
    On the command line, type the following:  
    **python app.py**.  
    **Note:** Use this command anytime you need to restart the app.

You can now get the `{"status": "up"}` response with the following from another command line:

```curl
curl http://localhost:5000/health
```

### Creating the Endpoint to Receive the Event

1. Log the request in the console.  
    Beneath the constants, type the following in the `app.py` file:

    ```python
    class Event(Resource):
        def post(self):
            app.logger.debug('Headers: %s', request.headers)
            app.logger.debug('Request: %s', request.get_data())
            return ('', 202)
    api.add_resource(Event, '/hello/world/event')
    ```

    > Fortellis appends the events endpoint to the URL that you enter when you are registering your webhook endpoint.

1. Type the following on the command line to start the server:

    ```curl
    python app.py
    ```

Now when you send a request to the local host at `http://localhost:5000/hello/world/event`,
you get a response with a  `202 Accepted` code and the empty body response.
The `Accepted` response tells Fortellis
that you received the message,
even if you don't process it immediately.

```curl
curl -v --location --request POST 'http://localhost:5000/hello/world/event' \
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

1. Create the JSON import in the `app.py` file.  
    At the top of the file with the other constants, type the following:  

    ```python
    import json
    ```

1. Create a `queue.json` file to save the events to while you wait to have the bandwidth to process them.  
    On the command line, type the following:  

    ```curl
    touch queue.json
    ```

1. Create the code within the post to write to a JSON file.  
    Under the log for the request, type the following to add the data in the request to a file:  

    ```python
    entry = json.loads(request.get_data().decode())

    with open('queue.json', '+r') as file:
        file_data = json.load(file)
        file_data['queue'].append(entry)
        file.seek(0)
        json.dump(file_data, file, indent = 2)
    ```

1. Put the following object in the `queue.json` file.

    ```json
    {"queue":[]}
    ```

Now, you have all the data going into the `queue.json` file
where you can get all your events and process them.

If you send the request now,
you will see the `queue.json` file populate with the information that you sent.

### Integrating Your App with Your Asynchronous API Producer

If you followed the previous tutorial on [Creating Your Asynchronous API Producer in Python](/docs/tutorials/event-relay/tutorial-develop-an-async-api-python),
you can now do the following:

1. Change the URL in your asynchronous API producer from `https://webhook.site/{yourUUID}` to your local environment endpoint `http://localhost:5000/hello/world/event`.
1. Start both the asynchronous API producer server and the asynchronous API consumer server.
1. Change the payload in the asynchronous API producer.
1. See the update in the app.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Set a `health` endpoint for your app.
* Receive a call from an asynchronous API producer.
* Store the call from the asynchronous API producer for processing later.

You can find the completed project on GitHub at [app-async-api-consumer-in-python](https://github.com/Fortellis/app-async-api-consumer-in-python).

In the next tutorial, you can see how to register your URL on Fortellis to receive requests from Asynchronous API Developers.
