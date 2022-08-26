---
title: Building an HTTP Callback to Receive Messages in Python
author: Nathaniel Sandberg
last updated: 6/9/2022
---

In this tutorial, we expand on the previous tutorial [Integrating Your App and an Asynchronous API Producer in Python](/docs/tutorials/admin-api/admin-api-tutorial-in-python) in creating an asynchronous app to deploy your endpoint to a public URL
that Fortellis can send events to.
Use this tutorial to find information on how to create your webhook and register your app on Fortellis to receive events.

> You must associate the `Data-Owner-Id` in the event header with the organization
> that is using the app.
> If you have multiple organizations subscribed to your app,
> you must send the event to the appropriate subscribed organization within the app.
> You can use the tutorial [Accessing Subscription Details Programmatically](/docs/tutorials/app-toolbox/using-the-subscriptions-endpoint) to get your subscriptions and match the `Data-Owner-Id` to an organization subscribed to your app.

## Tutorial  

### What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create an endpoint to show the contents of the `./queue.json` file.
* Register the app to Fortellis.
* Create an app that can consume events from Fortellis.
* Verify the token that Fortellis sends with the event.

### Things to Remember in This Tutorial

* Use **python app.py** to start the script.
* Use **Ctrl**+**C** to stop the script.
* Install the following before starting the tutorial:
    * Python
    * A command-line interface
    * A text editor

### Creating the Endpoint to Show the Queue

First, we create an endpoint so we can see when the `queue.json` file updates on the server.

* Create another get endpoint to show the contents of the file.  
    Beneath the post endpoint that receives the events, use the following to return the contents of the file:

    ```python
    class Queue(Resource):
        def get(self):
            with open('queue.json', '+r') as file:
                file_data = json.load(file)

            return file_data

    api.add_resource(Queue, '/hello/world')
    ```

You can now use the get request to get updates and process them when ready.
For this tutorial, we assume that you would automate the process for removing requests
that you have already processed.
To see events sent to you, do the following:

1. Type the following to start the server:  

    ```curl
    python app.py
    ```

1. Go to `http://localhost:5000/hello/world` in your browser.

### Creating Your Public Endpoint

You must create a publicly available endpoint for Fortellis to send events to your API.
You can use any platform you have.

For this tutorial, we use a simple deployment with [Python Anywhere](https://www.pythonanywhere.com).

> Python Anywhere disables the site after 3 months.
> Python Anywhere also only gives you 100 seconds of uptime every 24 hours.
> The app still runs after this,
> but it runs a little slower.
> For a more robust solution, use a server with more up-time.

### Creating Your Python Anywhere Project

You will now create a public project with an endpoint that Fortellis can post events to.

1. Sign up for [Python Anywhere](https://www.pythonanywhere.com).  
    **Note:** You can create a beginner account for free.  
1. Follow [Webapp basics](https://help.pythonanywhere.com/pages/WebAppBasics) to create the endpoint for your app.  
    **Note:** Select the following for your app when creating it:  
    * **Flask** for the framework.
    * **Python 3.9** for the version.
    * **app.py** for the file to hold the app.
1. Upload your `app.py` file.  
    1. Click **Account Dashboard** to go to the dashboard.  
    1. Click **Browse Files**.  
    1. Click **app.py** to open your `app.py` file.  
    1. Copy your `app.py` file from your local project to your project.
    1. Click **Save**.  
    1. Click the menu button to see the dropdown.
    1. Click dashboard to return to the dashboard.
1. Upload your `queue.json` file to your project.  
    1. Click **Browse Files** to see a list of your project files.  
    1. To upload a file, click **Upload a File**.
    1. Navigate to your `queue.json` file, and then click it.  
    1. To open the file, click **Open**.  
1. Upload the `requirements.txt` file.  
    1. Click **Upload File**.
    1. Navigate to your `requirements.txt` file, and then click it.
    1. To open the file, click **Open**.
1. Install the dependencies for your project.  
    1. Click **Dashboard** to return to the dashboard.
    1. Click **Bash** to get to the console.
    1. Install the dependencies.  
        On the command line, type the following:  

        ```bash
        pip install -r requirements.txt
        ```

        **Note:** If you see a warning about `fiona`, ignore it.

1. Remove the line at the bottom of the `app.py`.  
    Put a hashtag `#` in front of the following lines to comment them out:  

    ```python
    #if __name__ == '__main__':
    #    app.run(debug=True)
    ```

You can now type the following on the command line and start the app:  

```python
python app.py
```

You see the app run, but it also quickly returns to the command line.
Python Anywhere already runs the webserver,
and you just need the endpoints.

You can now go to the following sites:  

* `{yourUserName}.pythonanywhere.com/health`
* `{yourUserName}.pythonanywhere.com/hello/world`

### Registering Your App on Fortellis

You must register your app to receive calls from asynchronous APIs through Fortellis.
You can register for as many synchronous and asynchronous APIs as you want.
You must register the app to get your credentials.
Fortellis sends a token with your credentials to your webhook
when it sends an event to your endpoint.
Verify that the credentials in the token are yours to authorize the request to your endpoint.

#### Sign In and Choose the Organization

1. Sign in to [Fortellis]($[devNetworkUrl]).
1. Hover over your name in the upper-right corner.
1. Click **Change**.  
    ![Change Organizations]($[docsUrl]/static/images/signInToFortellis.PNG)
1. Click the organization.
1. Click **OK**.  
    ![Click Okay For Organization]($[docsUrl]/static/images/clickOkayForOrganization.PNG)

#### Create the App

1. Click **New App**.  
    ![Change Organizations]($[docsUrl]/static/images/pictureOfApps.PNG)
1. Enter the app details:  
    * **App Name**  
    * **App Description**  
    * **Website**  
    * **Callback URL**  
    **Note:** See [Authorization Flows](/docs/tutorials/solution-integration/auth/) to determine if you need a callback URL.  
        ![API Integrations]($[docsUrl]/static/images/registeringApps.PNG)
1. Select the asynchronous API that you want your app to use.
    1. Click **Async API** for the **API Type** to show only the asynchronous APIs.
    1. Click the **Category** of the API if you know it.
    1. Click the **Publisher** of the API if you know it.
    1. Search for the asynchronous API using the search box if you know the name.
    1. Click the asynchronous API that you want to use.  
        ![asynchronous APIs]($[docsUrl]/static/images/asynchronousAPI.PNG)  
    **Note:** Select your asynchronous API
    if you created one to see the webhook receiving calls from your asynchronous API at the end of the tutorial.
1. Click **Webhook** to enter your webhook where you receive events.  
    **Note:** You may use different webhooks for different asynchronous APIs.
1. Enter the URL for your Glitch app with your `/hello/world` endpoint and click **Save**.  
    **Note:** Fortellis appends the `/event` endpoint to the end of the URL and sends the event to that endpoint.
1. Click **Register** at the bottom of the page to complete your registration.  
    ![Solution Register]($[docsUrl]/static/images/solutionRegister.PNG)

You can now click the app to get to the app's page and get your **API Key** and **API Secret**.
You can find your credentials in the **Credentials** section of the page.
Use the [Client Credentials Flow for Authorization](/docs/tutorials/solution-integration/client-credentials) to get a token.

### Verifying the Access Token

Fortellis sends a token with your credentials to your webhook
when it sends an event to your endpoint.
Verify that the credentials in the token are yours to authorize the request to your endpoint.
We are going to create this locally first and then update Python Anywhere.

1. Install the `PyJWT` dependency.  
    On the command line, type the following:  
    **pip3 install pyjwt**
1. To decode the RS256 token, include the cryptography as well.  
    On the command line, type the following:  
    **pip3 install pyjwt[crypto]**
1. Add the import for `jwt` and `PyJWKClient` to the `app.py` file.  
    At the top of the file with the other imports, type the following:  

    ```python
    import jwt
    from jwt import PyJWKClient
    ```

1. Remove the Bearer prefix from the token.  
    Below the imports, type the following:  
    **PREFIX = 'Bearer '**  
    **Note:** Include the final space after `Bearer` to remove that from the token as well.

1. Update the `app.py` file to include the definition and validation of the token.  
    Above the `entry` constant, add the following:

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
        subject="{youAPIKey}",
        options={"verify_exp": True},
    )

    print(data)
    ```

    The constants define the following:
    * The JWT dependency
    * The Fortellis token domain
    * The token verifier
    * The audience in the token

You can now call the endpoint,
but in the request, you must include a token
you created with your **API Key**.

To get a token, use the following:

```curl
curl -X POST --url $[authServerUrl]/v1/token \
--header 'accept: application/json' \
--header 'authorization: Basic {Base64EncodedAPIKey:APISecret}' \
--header 'Cache-Control: no-cache' \
--data 'grant_type=client_credentials&scope=anonymous'
```

To add events to the local queue, use the following with your token.

```curl
curl --location --request POST 'http://localhost:3001/hello/world/event' --header 'Type: application/json' --header 'Content-Type: application/json' --header 'Authorization: Bearer {yourToken}' --data-raw '{
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

You must now get an authorization token with your credentials and send those to your endpoint.
For more information on getting your token, see the [Client Credentials Flow for Authorization](/docs/tutorials/solution-integration/client-credentials/).

### Updating Python Anywhere

After verifying that the request works locally, you can include the authorization in the code on the server to secure your webhook.

1. From the dashboard in your Python Anywhere account, click on the `app.py` file.
1. From your local files, copy the `app.py` file except for the last two lines:  

    ```python
    if __name__ == '__main__':
        app.run(debug=True)
    ```

1. On the server system, delete the contents of the `app.py` file.
1. On the server system, paste the copied content, and then click **Save**.
1. Click the menu button, and then click **Consoles**.
1. Click your running console.
1. Install the dependencies.  
    In bash on Python Anywhere, type the following:  

    ```bash
    pip3 install pyjwt==2.3.0 pyjwt[crypto]
    ```

### Integrating Your App with Your Asynchronous API Producer

> Currently, a Fortellis Administrator must create the subscription to your app and activate it
> before an asynchronous API sends events to your app webhook.
> Contact [support@fortellis.io](mailto:support@fortellis.io) to ask an administrator to create your subscription and activate it.

If you followed the previous tutorial on [Publishing Messages and Verifying Results in Python](/docs/tutorials/event-relay/publishing-messages-and-verifying-results-python),
you can now do the following:

1. Start your asynchronous API.
1. Send an Event from your asynchronous API to the event relay.  
    Update the payload in the asynchronous API if you, and then use the following curl request to send it:  

    ```curl
    curl -X POST localhost:5000/event \
    -H "Accept: application/json" \
    -H "Cache-Control:no-cache"
    ```

1. Receive the updates through Fortellis at your webhook in your Python Anywhere project.  
1. Refresh the page to see the updated `queue.json` file.  

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create an endpoint to show the contents of the `./queue.json` file.
* Register the app to Fortellis.
* Create an app that can consume events from Fortellis.
* Verify the token that Fortellis sends with the event.

You can find the completed project on GitHub at [python-async-app-webhook-for-deployment](https://github.com/nathansandberg-cdk-fortellis/python-async-app-webhook-for-deployment).

If you download the example repository, do the following:

1. Follow the steps to [Creating Your Python Anywhere Project](/docs/tutorials/event-relay/tutorial-building-an-http-callback-to-receive-messages-python#creating-your-python-anywhere-project).
1. Follow the steps to [Registering Your App on Fortellis](#registering-your-app-on-fortellis).
1. Add the dependencies for PyJWT.  
    In bash on Python Anywhere, type the following:

    ```bash
    pip3 install pyjwt==2.3.0 pyjwt[crypto]
    ```

1. Change the `sub` value to your **API Key** for your registered app.  
1. Start the server.  
    In bash on Python Anywhere, type the following:  

    ```bash
    python app.py
    ```
