---
title: Creating APIs in Python
author: Nathaniel Sandberg
last updated: 1/21/2022
---

You can use this tutorial to start making your API in Python and bring your API to more consumers quickly through Fortellis.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Send the response from the `inventory.json` file.
* authorize users with a Fortellis token.
* Verify the `Subscription-Id` of the user sending the request.
* See the request payload of the JSON web token on your command line.
* Receive a response with your curl request.

### Things to Remember in This Tutorial

You should remember the following:

* Use the following to start the server:  

    ```curl
    python app.py
    ```

* Use **Ctrl**+**C** to stop the server.
* Install Python before starting the tutorial.

You can find a completed project with the example on GitHub: [Python-API](https://github.com/Fortellis/Python-API).

## Creating and Updating the Virtual Environment

1. Create the folder for the new project.  
    On the command line, type the following:

    ```curl
    mkdir python-api
    ```

1. Change directories to your new project.  
    On the command line, type the following to access the directory:

    ```curl
    cd python-api
    ```

1. Install the dependency to create virtual environments.  
    On the command line, type the following:

    ```curl
    pip3 install virtualenv
    ```

1. Create the virtual environment directory.  
    On the command line, type the following to start the virtual environment:

    ```curl
    virtualenv venv
    ```

1. Update the Python interpreter you are using.  
    On the command line, type the following:

    ```curl
    virtualenv -p python3.9 venv
    ```

1. Activate the environment.  
    On Windows, type the following on the command line to activate the environment:

    ```curl
    source venv/Scripts/activate
    ```

    **Or**  
    On Mac, type the following on the command line to activate the environment:

    ```curl
    source venv/bin/activate
    ```

    **Note:** You can now see `(venv)` indicating that your environment is running.
    To stop the environment, type **deactivate** at any time.

## Creating the Endpoint

1. Install Flask.  
    On the command line, type the following:

    ```curl
    pip install flask
    ```

1. Install Flask Restful.  
    On the command line, type the following:

    ```curl
    pip install flask-restful
    ```

1. Freeze your dependencies.  
    On the command line, type the following:

    ```curl
    pip freeze > requirements.txt
    ```

1. Create the `app.py` file with the following content:

    ```python
    from flask import Flask, request
    from flask_restful import Resource, Api

    app = Flask(__name__)
    api = Api(app)

    class HelloWorld(Resource):
        def get(self):
            return {'hello': 'world'}

    api.add_resource(HelloWorld, '/')

    if __name__ == '__main__':
        app.run(debug=True)
    ```

1. Start the app.  
    On the command line, type the following:

    ```curl
    python app.py
    ```

You can now get the `{"hello": "world"}` response with the following from another command line:

```curl
curl http://localhost:5000
```

## Adding Logging

1. Add the logging dependency to the `app.py` file.  
    At the top of the `app.py` file, type the following:

    ```curl
    import logging
    ```

1. Define the logger.  
    Beneath `api = Api(app)`, type the following:

    ```curl
    logger = logging.getLogger('werkzeug')
    ```

1. Log some information in the `HelloWorld` class.  
    Beneath `def get(self):`, type the following:

    ```curl
    logger.info('Here\'s some logged info.')
    ```

1. After the `logger.info`, add the logger for the request headers and get the authorization token.  
    Under the logger info, type the following:

    ```curl
    logger.info(request.headers['Authorization'])
    ```

## Verifying the JSON Web Token

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

1. Install PyJWT.  
    On the command line, type the following:

    ```curl
    pip install pyjwt
    ```

1. Add the import to the `app.py` file.  
    At the top of the file with the other imports, type the following:

    ```curl
    from jwt import PyJWKClient
    ```

1. Add the JWT import at the top of the `app.py` file.  
    At the top of the `app.py` file with the other constants, type the following:

    ```curl
    import jwt
    ```

1. Remove the Bearer prefix from the token.  
    1. Below the imports, type the following:

    ```python
    PREFIX = 'Bearer '
    ```

    **Note:** Include the final space after `Bearer` to remove that from the token as well.
    1. After the code to log the authorization header, check if the authorization header includes the word `Bearer`.  
        Type the following after `logger.info(request.headers['Authorization'])`:

        ```python
        if not request.headers['Authorization'].startswith(PREFIX):
            return 'Not a Bearer token'
        ```

1. Update the `app.py` file to include the definition and validation of the token.  
    Above `{"hello": "world"}`, add the following:

    ```python
    token = request.headers['Authorization'][len(PREFIX):]

    url = "$[authServerUrl]/v1/keys"

    jwks_client = PyJWKClient(url)

    signing_key = jwks_client.get_signing_key_from_jwt(token)

    data = jwt.decode(
        token,
        signing_key.key,
        algorithms=["RS256"],
        audience="api_providers",
        subject = "{yourClientID}",
        options={"verify_exp": True}
    )

    print(data)

    logger.info(data)
    ```

1. Install the `authlib` dependency.  
    On the command line, type **pip install authlib**.

**Note:** You must now use your App Developer credentials to get a token.
See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) for more information on getting your credentials and getting tokens.

### Client Credentials Authorization Code Request

You must now include your App Developer credentials to get a response.

```bash
curl --request POST --url $[authServerUrl]/v1/token \
--header 'accept: application/json' \
--header 'authorization: Basic {Your base 64 encoded Client ID:Client Secret}' \
--header 'Cache-Control: no-cache' \
--data 'grant_type=client_credentials&scope=anonymous'
```

### Request to Your Local Server

Include your token in the header of your request to the local server.

```bash
curl http://localhost:5000 --header "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiJabGVFSFRZd2VIYlFjaUZvbWVBVEhiTUhnSFNjM0JEaCIsImp0aSI6IkFULjNOUXRIdkJJSDFnZDk3QjJhQVBTUFUxY1NRMUtWa0E1MFhqS3UwZHFsNzAiLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IlpsZUVIVFl3ZUhiUWNpRm9tZUFUSGJNSGdIU2MzQkRoIiwiaWF0IjoxNjE3OTExNzg2LCJleHAiOjE2MTc5MTUzODYsImF1ZCI6ImFwaV9wcm92aWRlcnMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.OgM1CmgOHvdZAeZVEtLxd_rrs8GCQww4vbeqeGVkeLChBKfWXfhDOdUYKEmq_RV98knZ-HNuxn86TkQ6aVXHq7L3gaWTtSGMI43BtD5eJ4VpoMR1pj0yg_52mm9wG6r3gk0p4CM_Q9nDiWzMCEHPbxX_BUuJIFCA99tRcMTyq84MsygC17eCX2dYA90UXNWwGV0nZFE6aEPleTDC29t8JRsR_0pboX7xbg4oPjGAGU9iy6W_IYFJI-4hyX4Z3077wnTUy7fcfUUY-ROk24gnAR_vLMnXH4d7dnAmHDFFBSJY-HjwfB0WQ-QS904J67Iej6o-6gdCMhEyDKWWspuY2w"
```

You can now see the information from the token on the command line and verify the information in it.

## Verifying the Subscription-Id

You should also verify the `Subscription-Id` in the header that you get in your requests.
For this demonstration, we are going to confirm a single UUID.
You can get the `Subscription-Id` in one of the following:

* The connection request if you have the Admin API.
* The UI if you accept connections manually.

1. Under the check for the bearer in the `Authorization` header, check for the `Subscription-Id` in the header.  
    In the `app.py` file, type the following:

    ```python
    if not 'Subscription-Id' in request.headers:
        return 'You must have a Subscription-Id.'
    ```

1. Under that, check if the request has `Subscription-Id` of one of your active apps.  
    In the `app.py` file, type the following:

    ```python
    if not request.headers['Subscription-Id']=="7e465be1-4fef-4840-8b84-a95ab4c74f0c":
        return 'Not subscribed'
    ```

    **Note:** For this example, we use the `Subscription-Id` `7e465be1-4fef-4840-8b84-a95ab4c74f0c`.

You now confirm that the request has a `Subscription-Id` and that it matches your active apps.

## Creating the API

1. Create the `inventory.json` file.  
    On the command line, type the following to create the `inventory.json` file:

    ```curl
    touch inventory.json
    ```

1. In the `inventory.json` file, add the following for the return response:

    ```json
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

1. Import the JSON dependency for python.  
    At the top of the `app.py` file, type the following to import JSON:

    ```curl
    import json
    ```

1. Import the inventory file into the `app.py` file.  
    In the `app.py` file, type the following above the `HelloWorld` class:

    ```python
    with open('inventory.json') as f:
        inventory = json.load(f)
    ```

1. Log the `inventory.json` file to the command line that the API server is running on.  
    In the `app.py` file below `logger.info(data)`, type **logger.info(inventory)**.
1. Send the inventory back to the API call instead of `{'hello': 'world'}`.  
    Replace `{'hello': 'world'}` with `inventory`.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Send the response from the `inventory.json` file.
* authorize users with a Fortellis token.
* Verify the `Subscription-Id` of the user sending the request.
* See the request payload of the JSON web token on your command line.
* Receive a response with your curl request.

You can download the project from GitHub at [Python-API](https://github.com/Fortellis/Python-API).

If you download the example repository, do the following:

1. In the directory of your choice download the repository from GitHub.  
    On the command line, type the following:  

    ```curl
    git clone git@github.com:Fortellis/Python-API.git
    ```

1. Follow the instructions in [Creating and Updating the Virtual Environment](#creating-and-updating-the-virtual-environment).
1. Install the dependencies for Python.  
    On the command line, type the following:  

    ```curl
    pip install
    ```

1. Start the server.  
    On the command line, type the following:

    ```curl
    python app.py
    ```

1. Get a token using cURL.  
    In a different command line, type the following:  

    ```curl
    curl -X POST --url $[authServerUrl]/v1/token \
    --header 'accept: application/json' \
    --header 'authorization: Basic Base64Encoded{yourClientID:youClientSecret}' \
    --header 'Cache-Control: no-cache' \
    --data 'grant_type=client_credentials&scope=anonymous'
    ```

1. Use the token that you get from Fortellis and the `Subscription-Id` to send a request to your API.  
    On the command line, type the following:

    ```curl
    curl --verbose -X GET  -H 'Subscription-Id:7e465be1-4fef-4840-8b84-a95ab4c74f0c' \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -H "Authorization: Bearer {youToken}" \
    http://localhost:5000 \
    ```
