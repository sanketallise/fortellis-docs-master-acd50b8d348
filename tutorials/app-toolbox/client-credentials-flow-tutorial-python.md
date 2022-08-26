---
title: Python Client Credentials Authorization Tutorial
author: Nathaniel Sandberg
last updated: 1/21/2022
---

You can use this tutorial to get your token using the client credentials flow in Python.

## What You Will Learn in This Tutorial

You will learn how to do the following in this tutorial:

* Create an authorization token using your credentials.
* Save your token to decrease latency in your app.
* Request a new token when your token expires.

## Things to Remember in This Tutorial

Remember the following before starting this tutorial:

* You must have Python and pip installed on your computer.
* You should have some familiarity with APIs.

You can follow the instructions at the end to download the completed project and get a working copy from GitHub: <https://github.com/Fortellis/client-credentials-for-python>.

## Creating the Virtual Environment

* Follow the instructions in [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python#creating-and-updating-the-virtual-environment).  

## Calling the Endpoint to Get Your authorization Token

1. Install the `requests` dependency.  
    On the command line in your project directory, type the following:

    ```curl
    pip install requests
    ```

1. Create the file where you are going to have the script to get the token.  
    On the command line, type the following:  

    ```curl
    touch index.py
    ```

1. Import the library to use it.  
    At the top of the `index.py` file, type the following:

    ```python
    import requests
    ```

1. Create the POST request to the endpoint to get your token.  
    In the `index.py` file, include the following lines to POST the request for the token:  

    ```python
    response = requests.post('$[authServerUrl]/v1/token', data = 'grant_type=client_credentials&scope=anonymous', headers= {'Authorization':'Basic base64encoded{yourAPIKey:yourAPISecret}', 'Accept':'application/json', 'Cache-Control':'no-cache' })
    ```

1. Print the response to see the request working.  
    On the next line, type the following:

    ```python
    print(response.json())
    ```

1. Run the script to see your token print to the console.  
    On the command line, type the following to run the script:

    ```curl
    python index.py
    ```

## Saving the Response to Another File

1. Create the file where you save the token.  
    On the command line, type the following to create the file:  

    ```curl
    touch currentToken.json
    ```

1. Import json to parse the json with Python.  
    At the top of the `index.py` file, type the following:  

    ```python
    import json
    ```

1. Write the returned response to the JSON file.  
    At the bottom of the file, write the following to save the response to the `currentToken.json` file:

    ```python
    with open('currentToken.json', 'r+') as file:
        json.dump(response.json(), file, indent=2)
    ```

You can now save your token to the file.
We now need to get a new token every two hours to make API requests.

## Calling the Function Every Two Hours

1. Import the dependency to run the function at intervals.  
    At the top of the `index.py` file, type the following:

    ```python
    import time
    ```

1. Add the while loop to repetitively call the function.  
    After the import, type the following:  

    ```python
    while true:
    ```

1. Put the rest of the file in the rest of the while statement.  
    Indent the rest of the file under the `while` to make it run continuously.  
1. After writing to the `currentToken.json` file, set the function to sleep for two hours.  
    On the bottom line with one tab indent, type the following to keep the function from running for the next two hours:  

    ```python
    time.sleep(72000)
    ```

## Registering the Application on Fortellis

Register the website using `https://example.com` and leave the callback URL blank.

1. Register your app by following the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps).
1. Get your App credentials, as described in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth).
1. [Base 64 encode](https://www.base64encode.org) your `API Key:API Secret` with the colon in the middle.
1. In the `index.py` file in the root folder, replace `base64encoded{yourAPIKey:yourAPISecret}` with your value.
1. Start the `index.py` server.  
   On the command line, type the following:  

   ```curl
   python index.py
   ```

You can now see the initial token populate on startup.
If you want to see the token populate more quickly,
lower the time variable.
You can see the 7,200 in seconds,
and you can lower it to 10 seconds to see the new token every 10 seconds instead of 7,200 seconds.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create an authorization token using your credentials.
* Save your token to decrease latency in your app.
* Request a new token when your token expires.

In the client credentials flow, you must do the following:

* Use your credentials to get your token.
* Associate the user with the `Subscription-Id`.
* Use your token and the `Subscription-Id` to make calls to the API.
* Hide your token from your app users.  
    **Note:** Remember that all actions taken with your token are attributable to you.
* Implement your own log-in flow if necessary.

You can find the project on GitHub at [client-credentials-for-python](https://github.com/Fortellis/client-credentials-for-python).

If you download the example repository, do the following:

1. Follow the instructions in [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python/#creating-and-updating-the-virtual-environment).
1. Install the dependency for `requests`.  
    On the command line, type the following:  

    ```curl
    pip install requests
    ```

1. Follow the instructions on [Registering the Application on Fortellis](#registering-the-application-on-fortellis) to register your app and get your credentials.
1. Replace `base64encoded{yourAPIKey:yourAPISecret}` with your [base 64 encoded](https://www.base64encode.org) `yourAPIKey:yourAPISecret` from your app.
