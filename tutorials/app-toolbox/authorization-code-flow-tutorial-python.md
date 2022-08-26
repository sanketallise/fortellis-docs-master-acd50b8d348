---
title: Python Authorization Code Tutorial
author: Nathaniel Sandberg
last updated: 2/3/2022
---

You can use the Fortellis sign in process to get authorization codes for your Subscribers and get tokens to access APIs.
You can follow the below tutorial to see how to do this in an app
that uses Python to proxy requests.

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' authorization code flow is not secure in the context of javascript apps and single pages apps.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create a UI for users to redirect to Fortellis where they sign in.
* Get an authorization code from Fortellis.
* Use the authorization code to get a token from Fortellis.

## Things to Remember in This Tutorial

Remember the following before starting this tutorial:

* You must have Python and pip installed on your computer.
* You should have some familiarity with APIs.
* Your users must have Fortellis accounts to use the authorization code flow.

You can follow the instructions at the end to download the completed project and get a working copy from GitHub: <https://github.com/Fortellis/python-authorization-code-flow>.

## Creating the Virtual Environment for Python

* Follow the instructions in [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python#creating-and-updating-the-virtual-environment).  

## Creating the Python Server

You now want to create the Python server. The server does the following:

* Proxies requests from the browser to avoid  [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) errors.
* Hosts the page at `localhost`.

1. Create the file that runs the server for Python.  
    On the command line, type **touch index.py**.
1. Import the `os` library to make sure that the webserver is running in the current directory.  
    In the `index.py` file, type **import os**.
1. Import the `http.server` library to import the server.  
    In the `index.py` file below the `os` import, type **from http.server import HTTPServer, CGIHTTPRequestHandler**.
1. Make the server in the current directory.  
    Below the imports, type **os.chdir('.')** to create the server in the current directory.
1. Create the server object.  
    Below `os.chdir('.')`, type the following to have the server listen to port 80 and handle the request:  

    ```python
    server_object = HTTPServer(server_address=('', 80), RequestHandlerClass=CGIHTTPRequestHandler)
    ```

1. Start the webserver and keep it running.  
    Below the server object, type **server_object.serve_forever()** to start the server.

To start the server, type **python index.py** on the command line.
You can now go to `localhost` in any browser and see files in the directory.
To stop the server at any time, type **Ctrl** + **C** on the command line.

## Creating the UI to Get the Code

1. Create the `index.html` file.  
    On the command line, type **touch index.html** to create the index file for your site.
1. Create the necessary content in the `index.html` file to see the server delivering content.

    ```html
    <html>
        <head>

        </head>
        <body>
            <h1>Hello World!</h1>
        </body>
    </html>
    ```

Type in **python index.py** to start the server again.
When you go to `localhost` now in the browser,
you can see the contents of the `index.html` file instead of the file directory.

## Registering the Application on Fortellis

In this process, register the app with the **Website** `http://example.com` and the **Callback URL** `http://localhost:80`.
You can then see the authorization code in the URL and get the token back.

1. Follow the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps) to register your app.  
1. Follow the instructions in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) to get your credentials.  
1. [Base 64 Encode](https://www.base64encode.org) your `API Key:API Secret`.  
1. Save the output for later in the tutorial.  

## Getting a Token

You can now get a token using your values.
You need to have your API key and your base 64 encoded `APIKey:APISecret` to follow the steps below.

1. Go to the following URL in your browser.  

    ```url
    $[authServerUrl]/v1/authorize?client_id={yourAPIKey}&response_type=code&scope=openid&redirect_uri=http://localhost:80/&state={yourUUIDForState}
    ```

    > You follow one of the following flows:
    >
    > * If you are not logged to Fortellis,
    >   you go through the following process:
    >   1. Fortellis prompts you to log in.
    >   1. Once your have logged in, Fortellis directs you to `http://localhost:80/` with the authorization code in the URL fragment.
    >   1. Use the authorization code in the URL fragment to get a token.  
    >   **Or**  
    > * If you are logged in to Fortellis,
    >   Fortellis directs you to `http://localhost:80` with the authorization code in the URL fragment.

1. Get the authorization code from the URL fragment.  
    Take the authorization code from the value of `code` in the URL.  

    **Example**

    ```url
    http://localhost/?code=K5Zvt5mBiyzhqcmvoa8cloq0PT6cUu1mhl1U5TZlevk&state=fc2299c9-b84f-4fe4-a96d-e15c12bebfe7
    ```

1. Use a post request to get your bearer authorization token in the response.  
    **Note:** You must send the curl request within 60 seconds, or the authorization code expires.

    ```curl
    curl --request POST \
    --url $[authServerUrl]/v1/token \
    --header 'accept: application/json' \
    --header 'authorization: Basic base64encoded{APIKey:APISecret}' \
    --header 'content-type: application/x-www-form-urlencoded' \
    --data 'grant_type=authorization_code&redirect_uri=http://localhost:80/&code={yourAuthorizationCode}'
    ```

When you send the request,
you receive the `access_token`
that you can use to access APIs on Fortellis.
Now we want to automate the process to give your Subscribers access tokens in the browser.

## Redirect to Fortellis to Get the Authorization Code

You must redirect the user to the Fortellis sign in page to sign in to Fortellis and get an authorization code.

1. Add the redirect to the web page to send the user to Fortellis to sign in and get an authorization code from Fortellis.  
    Add the following `script` in the head to send the user to Fortellis on page load.  

    ```javascript
    <script>
        window.location.href = "$[authServerUrl]/v1/authorize?client_id={yourAPIKey}&response_type=code&scope=openid&redirect_uri=http://localhost:80/&state=fc2299c9-b84f-4fe4-a96d-e15c12bebfe7"
    <script>
    ```

    **Note:** Replace `yourAPIKey` from your registered app.
    You may choose to replace the state with a universally unique identifier of your own.
    After we add the above line of code,
    the page continuously fetches a new token.
    We now have to get the token and prevent the page from redirecting
    if we already have one.
1. Get the URL fragment
    when the user returns to your site.  
    Add the code below to the top of the script to get the authorization code from the URL:  

    ```javascript
    const params = new URLSearchParams(window.location.search);
    console.log("This is the code in the URL: " + params.get('code'));
    ```

    **Note:** You can now see the authorization code logging to the console.

1. Stop the page from redirecting if you have the code.  
    Add the following code to the `index.html` file around the `window.location.href` redirect code:

    ```javascript
    if(params.has('code')){
    }else{
        window.location.href = "$[authServerUrl]/v1/authorize?client_id={yourAPIKey}&response_type=code&scope=openid&redirect_uri=http://localhost:80/&state=fc2299c9-b84f-4fe4-a96d-e15c12bebfe7"
    }
    ```

Now, the page only redirects and reloads
if it doesn't have an authorization code in the URL.

## Send the Authorization Code to Fortellis to Get a Token

You now need to take the authorization code from the browser and send it to Fortellis to get a token for accessing APIs.
However, because we get a CORS error
if we try to send the code from the browser,
we have to proxy the request through the python server.

1. Stop the server if necessary.  
    On the command line, type **Ctrl**+**C**.
1. Install `flask` to use the dependency
    that supports APIs.  
    On the command line, type **pip install flask**.
1. Install `flask-restful` to use the rest services with Flask.  
    On the command line, type **pip install flask-restful**.
1. Create the `api.py` file to control the requests for tokens through the API.  
    On the command line, type **touch api.py**.  
1. Import the dependencies from `flask` and `flask-restful`.  
    At the top of the `index.py` file, import the dependencies with the following:

    ```python
    from flask import Flask, request
    from flask_restful import Resource, Api, reqparse
    ```

1. Create the app for the API.  
    Type the following to create the app after the imports:  

    ```python
    app = Flask(__name__)
    api = Api(app)
    ```

1. Create the `token` endpoint that you will use to route a call to Fortellis from the browser.  
    Below the app, type the following to create the app endpoint.

    ```python
    class Tokens(Resource):
      def post(self):
        return "We have received your request for a token."
    api.add_resource(Tokens, '/v1/token')
    if __name__ == "__main__":
      app.run(debug=True)
    ```

You can now start the API server with **python api.py** and get a response from your local endpoint using the following curl request:

```curl
curl --request POST http://localhost:5000/v1/token
```

## Setting the Python Endpoint to Forward Requests

You can now convert your `v1/token` endpoint to send requests to Fortellis.

1. Install the `requests` dependency.  
    On the command line in your project directory, type **pip install requests**.
1. Import the library to use it.  
    At the top of the `api.py` file, type **import requests**.
1. Do the following:  
    * Create a variable for the authorization code.
    * Create the POST request with the data and the headers to the endpoint to get your token.  
    * Print the response to the console.  
    * Send the response back to the requester.  
    In the `api.py` file, include the following lines to POST the request for the token:  

    ```python
    authorizationCode = '{yourAuthorizationCode}'
    response = requests.post('$[authServerUrl]/v1/token', data = 'grant_type=authorization_code&redirect_uri=http://localhost:80/&code=' + authorizationCode, headers= {'Authorization':'Basic base64encoded{yourAPIKey:yourAPISecret}', 'Accept':'application/json', 'Content-Type':'application/x-www-form-urlencoded' })
    app.logger.debug(response.json())
    return response.json()
    ```

1. Run the script to start the server for the APIs.  
    On the command line, type **python api.py**.
1. Go to the Fortellis authorization URL and use your information in the parameters to redirect to your site.  
    In a browser, go to the following site:

    ```html
    $[authServerUrl]/v1/authorize?client_id={yourAPIKey}&response_type=code&scope=openid&redirect_uri=http://localhost:80/&state=fc2299c9-b84f-4fe4-a96d-e15c12bebfe7
    ```

    **Note:** The site may redirect to Fortellis
    if you have not logged in to Fortellis.
1. Use your authorization code from the URL in the Python code.  
    In the `api.py` file, replace the `authorizationCode` with the code in the URL that you receive after logging in.  
    **Note:** The authorization code expires in 60 seconds.
    Make the call to the endpoint before then or get an error
    that the authorization code has expired.
1. Make a call to the endpoint to get the token from Fortellis.  
    In another terminal, use the curl request.  

    ```curl
    curl --request POST http://localhost:5000/v1/token
    ```

You see your `access_token` in the response
that you get back using your python server as a proxy.

## Send the URL Fragment to Python from JavaScript

1. Send the request to the python backend and log the response.  
    In the `if` statement in `index.html`, write the function to do the following:  
    * Send the authorization code to the proxy.
    * Wait for the response from the proxy server.
    * Log the output of the token request.

    ```javascript
    ...
    function load(){
        let request = new XMLHttpRequest();
        request.onreadystatechange = function(){
            if (request.readyState === 4){
                console.log(request.response);
            }
        }
        request.open("POST", "http://localhost:5000/v1/token");
        request.send(params.get('code'));
    }
    load();
    ...
    ```

1. Install the `flask-cors` dependency to avoid CORS errors.  
    On the command line, type **pip install flask-cors**.
1. Import `flask_cors` in the `api.py` file.  
    With the other imports, add the following:

    ```python
    from flask_cors import CORS
    ```

1. Configure CORS to prevent the error.  
    After `api = Api(app)`, add **CORS(app)**.

    ```python
    app = Flask(__name__)
    api = Api(app)
    CORS(app)
    ```

1. Change the `authorizationCode` to equal the authorization code in the URL.  
    In the `api.py` file, change `authorizationCode` to equal your decoded authorization code.  

    ```python
    authorizationCode = request.get_data().decode()
    ```

## Running the Project

1. Open a separate terminal in your Python project.  
    > You are now going to run the two Python scripts from two terminals.
1. Activate the environment in the new terminal.  
    On Windows, type **source venv/Scripts/activate** on the command line.  
    **Or**  
    On Mac, type **source venv/bin/activate** on the command line.  
1. Start the script to run the server and host the site.  
    On the command line in your Python project, type **python index.py**.
1. Start the script to run the API proxy.  
    On another command line in your Python project, type **python api.py**.
1. Go to your website.  
    In a browser, go to `localhost`.  
    **Note:** You may have to log in to Fortellis.
    Open the console in your browser to see the authorization request from Fortellis.

    ```json
    {
    "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiJlb3BFR2hDUEtPa0dLSUtZWEdMMjByUTVxem1LUlRaRSIsImp0aSI6IkFULkhVVzY0NkJtQ2Zib3FSTnFtSlh5ZnFIYnE5R2h4UWtKdERtUGR2MmZuOWciLCJzY3AiOlsib3BlbmlkIl0sInN1YiI6Im5hdGhhbi5zYW5kYmVyZ0BjZGsuY29tIiwidWlkIjoiMDB1M253anIybEN1Y1cxRW4ycDciLCJpYXQiOjE2NDMzMTY1MTksImV4cCI6MTY0MzMzMDkxOSwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlzcyI6Imh0dHBzOi8vaWRlbnRpdHktZGV2LmZvcnRlbGxpcy5pby9vYXV0aDIvYXVzMW5pNWk5bjlXa3pjWWEycDcifQ.zeP6QcryUZ2RsHkO8aBy9bkGTHFfkTz8FOn8oTgDG8lQfUEtN6d_fgbKnCH4Xzhc_B0AMFvas3lNg7ztbM8TDsLIIDfxYK9fTg-Rdq4OhD-3KZ912X84GkgXbqtPJUd6L_iJGLrldXEoKz38UAS9mcYR6GBGGaWrWlUeXiCKWtTDypjEFzwfpDKYf4PZ-pj-oPOtAmnwrqJLwtFUXjkWCnWv4GC34swXOQh-2nb70xNjh0pMmrjBhbKD1biJ7yJZPFfQaDxEYVw1OwEJcOC_wLSKwc13vkM7YOtLkKxtQGWeEir6KEzpEvhFh4kQLK9X7fPk1oq_AA5d3h-F0s03mQ",
        "scope": "openid",
        "id_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJqdGkiOiJJRC5jWHlLQ0l4THpLM0JCNktBYm5ORlZscWdQNjgxQVF0WGVaLUFFYVVZY0swIiwic3ViIjoiMDB1M253anIybEN1Y1cxRW4ycDciLCJpZHAiOiIwMG8xYW40OHExU3UxUkNGaDJwNyIsImlhdCI6MTY0MzMxNjUxOSwiZXhwIjoxNjQzMzMwOTE5LCJhdWQiOiJmb3J0ZWxsaXMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.Vbg1MCXPEUMZaBEvboDrmAvw_Hy3zxOn9HvnPP85BdDngxW1brFfJUn76QNNqs_u0HaGahCxPkF5O0gcAT_-b84qDc-0X71WbjQkRrEso1mNhcCtuFCkXKRIOdc2wBgDYhsEomeaP07H0nJxmWDNzApr4ihRVfsCLXe1T4eg1qvsNWRfLf2-3mm8P3yyYDQiG0Q69DMPAFF6IvRQXLyjUPCsk0HEjeh5SnKajTogWo9Y48jbvegOYKG5r86NuXy9Z9IbF0-tHUyLjEvIhMo59nHgjrPbJZza922t_DFe4KtoBavDPOZljYBf5RobpR4IcGZa0V-BagqRRsM21J_xsg"
    }
    ```

You can now get your token using Python.
Use the token to call APIs and authorize requests for your users.
You keep the token in the browser to use with your requests.
Remember that Fortellis attributes any actions taken using your token to you.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create a UI for users to redirect to Fortellis where they sign in.
* Get an authorization code from Fortellis.
* Use the authorization code to get a token from Fortellis.

You can find the completed project on GitHub at [python-authorization-code-flow](https://github.com/Fortellis/python-authorization-code-flow).

If you download the repository, do the following to start the project:  

1. Follow the steps to [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python#creating-and-updating-the-virtual-environment).
1. Follow the step to [Registering You App on Fortellis](/docs/tutorials/event-relay/tutorial-building-an-http-callback-to-receive-messages/#registering-your-app-to-fortellis).  
    **Note:** Register the app with the **Website** `http://example.com` and the **Callback URL** `http://localhost:80`.
1. Replace the placeholder values in the `api.py` file and the `index.html` file with the values for your registered app.
1. Use pip to install the dependencies.  
    For each of the following, type **pip install {dependency}** on the command line:  
    * **flask**
    * **flask-restful**
    * **requests**
    * **flask-cors**
1. Start the virtual environment when you open a separate command line to start both scripts.  
    On the open command line, type **source venv/Scripts/activate**.

1. Start both services.  
    Do the following:  
    * On one command line, type **python api.py** to start the API proxy.
    * On the other command line, type **python index.py** to start the server serving the html page.
1. See the project create your token.  
    1. Go to `localhost`.  
    1. Sign in to Fortellis if necessary.  
    1. Open the command line to see the payload and your token.

You can now start developing your app.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
