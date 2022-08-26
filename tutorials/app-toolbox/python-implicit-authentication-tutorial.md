---
title: Python Implicit Authorization Tutorial
author: Nathaniel Sandberg
last updated: 3/22/2022
---

With this flow, your users sign in to Fortellis to get tokens for your app.
Once the users are signed in,
you can use the token to make calls to APIs for them.
With this tutorial, you can create apps that retrieve tokens
that you can use with APIs on Fortellis.

**Note:** Apps must be active before you can make API calls to the Production service and receive live data in responses.
If API Developers selected to activate apps manually or through the Admin API,
they must activate the connections before your app can make calls to get real data as well.

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' implicit flow is not secure in the context of javascript apps and single page apps.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create a UI for users to redirect to Fortellis where they sign in.
* Keep the user on your site once they have a token.
* Get the token value.
* Log that value to the console.

## Things to Remember in This Tutorial

Remember the following before starting this tutorial:

* You must have Python and pip installed on your computer.
* You should have some familiarity with APIs.
* Your users must have Fortellis accounts to use the implicit flow.

You can follow the instructions at the end to download the completed project and get a working copy from GitHub: <https://github.com/Fortellis/python-implicit-flow-tutorial>.

## Creating the Virtual Environment for Python

* Follow the instructions in [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python#creating-and-updating-the-virtual-environment).  

## Creating the Python Server

You now want to create the Python server.
The server hosts the webpage at `localhost`.
You can also use it to proxy requests from the browser and avoid [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) errors.

1. Create the file that runs the server for Python.  
    On the command line, type **touch index.py**.
1. Import the `os` library to make sure that the webserver is running in the current directory.  
    In the `index.py` file, type **import os**.
1. Import the `http.server` library
    that you use to create the server.  
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
1. Start the server.  
    On the command line, type **python index.py**.

You can now go to `localhost` in any browser and see files in the directory.
To stop the server at any time, type **Ctrl** + **C** on the command line.

## Creating the UI to Get the Code

1. Stop the server to change the folder directory.  
    On the command line, type **Ctrl**+**C**.
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

1. Start the server.  
    On the command line, type **python index.py**.

When you go to `localhost` now in the browser,
you can see the contents of the `index.html` file instead of the file directory.

## Registering the Application on Fortellis

In this process, register the app with the **Website** `http://example.com` and the **Callback URL** `http://localhost:80`.

1. Follow the instructions in [Development](/docs/tutorials/solution-integration/development) to register your app.  
1. Follow the instructions in [Acquiring Your API Secret and Key](/docs/tutorials/solution-integration/auth/#acquiring-your-api-secret-and-key).  
1. Save your **API Key** and **API Secret** for later in the tutorial.  

## Getting a Token

To get a token using the implicit flow, do the following:

1. Insert your **Client ID** from your app into the following code:

    ```curl
    $[authServerUrl]/v1/authorize?client_id={yourClientID}&response_type=token&scope=openid&redirect_uri=http://localhost:80/&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7&nonce=true
    ```

1. Enter the URL you created in the browser of your choice, and then press **Enter**.
1. If you are not logged in to Fortellis,
    sign in to Fortellis.  
    **Or**  
    If you are logged in,
    Fortellis directs you to your `localhost` with your token in the URL.  

## Redirect From Your URL to Fortellis

We are now going to authorize using the Fortellis sign in screen.

* Redirect the users to Fortellis to get a token in their URL
    to authorize through Fortellis.  

    ```javascript
    <script>
        if(window.location.hash){
            console.log('This is the hash for the location: ' + location.hash.substr(1).split('=')[1]);
        }else{
            window.location.href = "$[accountsServerUrl]/v1/authorize?client_id=XYx6mKImK6g1wLFK67zoBzZG7MBjH7EG&response_type=token&scope=openid&redirect_uri=http://localhost:80/&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7&nonce=true"
        }
    </script>
    ```

You can now see the following:

* If you go to `localhost`,
    you are redirected to sign in to Fortellis
    until you have a token in your URL.  
* You get the token in your URL.  
* You can see your token logged to the console in your web browser.  
    **Note:** Fortellis automatically adds the `https` protocol to your **Callback URL**.
    Remove the `s` in `https` to go to your local site.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create a UI for users to redirect to Fortellis where they sign in.
* Keep the user on your site once they have a token.
* Get the token value.
* Log that value to the console.

You can find the completed project on GitHub at [python-implicit-flow-tutorial](https://github.com/Fortellis/python-implicit-flow-tutorial).

If you download the repository, do the following to start the project:  

1. Follow the steps to [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python#creating-and-updating-the-virtual-environment).
1. Follow the step to [Registering Your App on Fortellis](/docs/tutorials/event-relay/tutorial-building-an-http-callback-to-receive-messages/#registering-your-app-to-fortellis).  
    **Note:** Register the app with the **Website** `http://example.com` and the **Callback URL** `http://localhost:80`.
1. Replace the placeholder for `{yourAPIKey}` in the `index.html` file.  
1. Start the server.  
    On the command line, type **python index.py**.
1. Go to `localhost` in your browser.  
1. Do one of the following:  
    * If you are not signed in,
    sign in with your Fortellis credentials to get your token and be redirected back to the local server.  
    **Or**  
    If you are signed in to Fortellis,
    you get a token in the URL that you can see logged to the console.  

You can now start developing your app and getting tokens to use APIs.
You must have a `Subscription-Id` in your request to get live data.
See [Making Calls to the Test Server](/docs/tutorials/solution-integration/development/#making-calls-to-the-test-server) for more information.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
