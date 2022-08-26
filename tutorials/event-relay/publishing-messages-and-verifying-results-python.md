---
title: Publishing Messages and Verifying Results in Python
author: Nathaniel Sandberg
last updated: 6/16/2022
---

Use the previous tutorial to create your project and this tutorial to send your events to Fortellis.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Register your asynchronous API on Fortellis.
* Update your asynchronous API from the previous tutorial to do the following:
    * Accept a request to update the payload.json.
    * Get a Fortellis token with your Client Key and Client Secret.
    * Use the Fortellis token to send the event to Fortellis.
    > You must send the `Data-Owner-Id` in the header of the event to send the event to your `subscriptionId`.
    > You must send the `partitionKey` in the headers to send events in order,
    > but you may exclude the `partitionKey` if the order doesn't matter.
* Subscribe to your asynchronous API to see your event go through the platform.

### Things to Remember in This Tutorial

You should remember the following:

* Use the following to start the script:  

    ```bash
    python app.py
    ```

* Use **Ctrl**+**C** to stop the script.
* Install the following before starting the tutorial:
    * Python
    * A command-line interface
    * A text editor

For this tutorial, we also use [https://webhook.site](https://webhook.site) so you don't have to set up a web tunnel,
but you can set up the receiving endpoint however you would like to.

You can follow the instructions at the end to download the completed project and get a working copy from GitHub: <https://github.com/Fortellis/Publishing-Messages-and-Verifying-Results-in-Python>.

## Registering Your Asynchronous API

You must register your asynchronous API to get your access token.
Fortellis uses your ID in the access token to route the events to your Subscribers.
Use a unique name for the asynchronous API from the `title` in your spec.

1. Sign in to [Fortellis]($[devNetworkUrl]).  
    ![Sign In]($[docsUrl]/static/images/signInToFortellis.PNG)  
1. Choose the organization that your want to register the asynchronous API to.  
    1. Hover over your name in the upper-right corner.  
    1. Click **Change**.  
        ![Change Organizations]($[docsUrl]/static/images/changeOrganizations.PNG)  
    1. Click the organization.  
    1. Click **OK**.  
        ![Click Okay for Organization]($[docsUrl]/static/images/clickOkayForOrganization.PNG)  
1. Click **New API**.  
    **Note:** You may see the option to choose a **Namespace** and **Environment**,
    but you can only use the production environment and the `/v2/events` endpoint for asynchronous APIs.  
        ![New API]($[docsUrl]/static/images/clickOkayForOrganization.PNG)  
1. Click **Choose File**.  
    ![Choose File]($[docsUrl]/static/images/NewAPIUpload.PNG)  
1. Do the following to select the spec from your computer:  
    1. Navigate to your asynchronous API spec.  
    1. Select your asynchronous API spec.  
    1. Click **Open** to choose the spec.  
        **Note:** Fortellis reads that it is an asynchronous API and populates the information on the following:  
        * **Async API Name:** Fortellis populates the name from the title in the spec.  
        * **Description:** Fortellis populates the description from the description in the spec.  
        * **Version:** Fortellis populates the version from the version in the spec.  
        * **Broker URL:** Fortellis populates the broker URL from the server in the spec.  
        * **Access Control** Fortellis populates the access control model from the security scheme in the spec.  
        ![Choose File]($[docsUrl]/static/images/NewAPIUpload.PNG)  
1. Select the following options:  
    1. Select the **Lifecycle Status**.  
    1. Select whether to **Show API Directory**.  
1. Click **Create API**.  
    ![Create API button]($[docsUrl]/static/images/NewAPIUpload.PNG)  

From this page, you can do the following:  

* Download the spec.  
* Get the **Broker URL**.  
* Edit your spec.  
* Get your **Client ID** and **Client Secret**.  
* Upload your **Documentation**.  
* Enter your **Support** information.  
* Enter your **Pricing** information.  
* Upload your **Terms**.  
* Make the API public.  
* Revert the API to private.
* Choose one of the following options:  
    * Make the API public.  
        **Note:** You must complete the following required field to make the asynchronous API public:  
        * **Documentation**
        * **Support**
        * **Pricing**
        * **Terms**
        If you choose to make the API public, users can see your API and spec in the [API Directory]($[apiReferenceUrl]) when you make it public.
    * Revert the API to private.
    * Add an organization from your **Organizations with Access** list by doing the following:  
        1. Click **Share API**.  
        2. Enter the ID of the organization.  
            **Note:** You must get the organization ID from a member of the organization.
            Follow the instruction in [Getting the Entity ID of an Organization](/docs/general/overview/organizations#getting-the-entity-id-of-an-organization) to get your organization ID.  
        3. Click **Share API** to confirm.  
        4. Click **Share** again.  
    * Remove an organization from your **Organizations with Access** list by doing the following:
        1. Click **Share API**.  
        2. Click **Remove** next to the organization to remove that organization from your **Organizations with Access** list.  
        3. Click **Remove** to confirm the removal of the organization.  

![Make Changes to the API]($[docsUrl]/static/images/makeTheFollowingOptions.PNG)  

For this tutorial, you only need to get your **Client ID** and **Client Secret**.  
Follow the instructions in [Update API](/docs/tutorials/configuration/updating-your-api-implementation) to update the other fields in your API.  

### Getting Your Client ID and Secret

Under credentials, use the copy icon to copy the **Client ID** and the **Client Secret** and save them for later.

## Sending Asynchronous APIs through Fortellis

Fortellis uses the token in your asynchronous API calls to route the payload and send it to all your Subscribers.

### Sending Your Event

1. Go to [Base64 encode](https://www.base64encode.org).
1. Enter `{yourClientID}:{yourClientSecret}` in the top grey box.
1. Click **Encode** below the box.
1. Copy your credentials from the box below encode.
1. Use your encoded credentials to get your token.

    ```curl
    curl -X POST --url $[authServerUrl]/v1/token \
    --header 'accept: application/json' \
    --header 'authorization: Basic {yourEncodedClientIDAndSecret}' \
    --header 'Cache-Control: no-cache' \
    --data 'grant_type=client_credentials&scope=anonymous'
    ```

1. Send the event to Fortellis for routing to your Subscribers.  

    ```curl
    curl -X POST $[eventRelay] \
    --header  "Accept: application/json" \
    --header  "Content-Type: application/json" \
    --header 'Data-Owner-Id: {dataOrganizationOwnerIdOnFortellis}' \
    --header "Authorization: Bearer {yourToken}" \
    --header 'partitionKey: {aUniqueUUIDForYourPartition}' \
    --data '{your event}'
    ```

> Use a unique identifier for your `partitionKey`.
> Fortellis associates any events sent with that `partitionKey` with events on that partition.
> If any events fail to send successfully to a webhook with that `partitionKey`,
> Fortellis attempts to resend the message until it receives a success response.
> It does not send any more messages with that `partitionKey` to that webhook until it receives a success response to ensure in order delivery.

### Creating the Event Endpoint to Send the Event

Complete [Developing an Asynchronous API in Python](/docs/tutorials/event-relay/tutorial-develop-an-async-api-python) or download the existing repository from GitHub at [python-async-api-example](https://github.com/Fortellis/python-async-api-example).
Do the following if you download the repository from GitHub:

1. Follow the instructions in [Creating and Updating the Virtual Environment](/docs/tutorials/admin-api/admin-api-tutorial-in-python#creating-and-updating-the-virtual-environment).
1. Install the Python dependencies.  
    On the command line, type the following:  

    ```bash
    pip install -r requirements.txt
    ```

1. Install the following dependencies:  
    * `flask`
    * `flask-restful`  
    * `uuid`  
    On the command line, type the following:  

    ```bash
    pip3 install flask flask-restful uuid
    ```

1. Import flask and flask-restful.  
    At the top of the `app.py` file with the other imports, type the following:  

    ```python
    from flask import Flask, request
    from flask_restful import Resource, Api, reqparse, abort
    import uuid
    ```

1. Create the Flask app.  
    Below the imports, type the following to accept API calls.  

    ```python
    app = Flask(__name__)
    api = Api(app)
    ```

1. Add the code to start the Flask server.  
    Replace the `src_path` script with the following `if` statement:  

    ```python
    if __name__ == '__main__':
        app.run(debug=True)
    ```

1. Create the endpoint to accept the post requests.  
    Replace the `EventHandler` class with the following:  

    ```python
    class Event(Resource):
        def post(self):
            app.logger.debug('headers: %s' , request.headers)
            print("Request received")
            token = requests.post('$[authServerUrl]/v1/token', data = {'grant_type': 'client_credentials', 'scope': 'anonymous'}, headers= {'Authorization':'Basic UW5OeExtYVJFVFdDZVZ3bEZUeUgwUnBucnQ3S2pyT286VnhubDVva2VVQ2M1eGFuTg==', 'Accept':'application/json', 'Cache-Control':'no-cache' })
            print((token.json()['access_token']), flush=True)

            requestId = str(uuid.uuid4())
            print(requestId)

            event = requests.post('$[eventRelay]', json = {"id":"6620800b-7029-4a7a-8e80-cdd852fc01c8", "number": 16,"haveYouSaidHello": True, "waysToSayHello": ["Hello", "Hola", "Hallo", "Bonjour", "Guttendag", "Ola"],"helloID": { "language": "English", "id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"}}, headers = {'Accept':'application/json', 'Content-Type': 'application/json', 'Data-Owner-Id': '{subscribingOrganizationId}', 'X-Request-Id': requestId,'Authorization': 'Bearer ' + token.json()['access_token'], 'partitionKey': '{aUniqueUUIDForYourPartition}'})
            print(event.json(), flush=True)

            return(event.json())
    api.add_resource(Event, '/event')
    ```

    Get the values from the following locations:
    * Get your `partitionKey` using a unique combination of letters and numbers.  
        **Note:** Fortellis uses the `partitionKey` to ensure your events are delivered in order.
        To deliver events in any order, leave out this header parameter.
    * Get your `Data-Owner-Id` from your organization ID that subscribes to the app.  
        Do one of the following to subscribe to the app:
        * **Activate** the app in [Marketplace]($[marketplaceUrl]).
        * Contact [support@fortellis.io](mailto:support@fortellis.io) to have them add you subscription to the app.  
        **Note:** Only the subscription to the app with the `Data-Owner-Id` receives the event.
        See [Getting the Entity ID of an Organization](/docs/general/overview/organizations/#getting-the-entity-id-of-an-organization) for more information on how you can get your organization ID.  
    * Create a Universally Unique Identifier (UUID) to track each request across systems.  
        We created this the `requestId` variable.  

> You can only send an event through the event relay to one organization at a time
> because you must include the `Data-Owner-Id` in the event header with the `organizationId`.

Start the server.  
On the command line, type the following:  

```bash
python app.py
```

You can now start your server and post to your local Python script and send an event through Fortellis.
On a separate command line, type the following to send the request to your locally running Python script.

```curl
curl -X POST localhost:5000/event \
-H "Accept: application/json" \
-H "Cache-Control:no-cache"
```

You should receive a response that looks like the following:

```bash
{
    "eventId": "2fceacc0-4fa4-4911-91ba-82a099de7ad7"
}
```

## Registering an App to Receive Events

You now need to register an app to use your asynchronous API to receive events from it.
For this portion, we will register the app with the webhook that we previously used in [Developing an Asynchronous API in Python](/docs/tutorials/event-relay/tutorial-develop-an-async-api-python).

### Creating a New App

1. Sign in to the [Developer Network]($[devNetworkUrl]).
    ![Sign In]($[docsUrl/static/images/signInToFortellis.PNG)
1. Choose the organization you want to register the app for.
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
        ![Change Organizations]($[docsUrl]/static/images/changeOrganizations.PNG)  
    1. Click the organization.
    1. Click **OK**.
        ![Click Okay for Organization]($[docsUrl]/static/images/clickOkayForOrganization.PNG)  
1. Click **New App**.

### Entering Your App Details

1. Enter the following information for your app:
    * **App Name**
    * **App Description**
    * **Website**
    * **Callback URL**  
        You only need to include the callback URL if you are using the authorization code flow or the implicit authorization flow.
        See [Implicit Flow for Authorization](/docs/tutorials/solution-integration/implicit-flow) and [Authorization Code Flow](/docs/tutorials/solution-integration/authorization-code) for more information.  
            ![App Options]($[docsUrl]/static/images/signInToFortellis.PNG)
1. Select **Async** for the **API Type**.  
    ![API Integrations]($[docsUrl]/static/images/asynchronousAPI.PNG)
1. Type the name of your asynchronous API in the search field, and then select your API.  
1. Click **Webhook**.  
1. Enter your webhook URL.  
    **Example:** `https://webhook.site/e75a2b4a-573d-49fd-aa09-44e074d00f73`
1. Click **Save**.
1. Click **Register**.

Once you have registered your app on your behalf,
you can send events from your asynchronous API to your app and see them go to your webhook.

## Next Steps

This tutorial should have shown you how to do the following:

* Register your asynchronous API on Fortellis.
* Update your asynchronous API from the previous tutorial to do the following:
    * Accept a request to update your asynchronous API.
    * Get a Fortellis token with your **Client Key** and **Client Secret**.
    * Use the Fortellis token to send the event to Fortellis.
* Subscribe to your asynchronous API to see your event go through the Fortellis Event Relay.

You can now send events through Fortellis and see them going to your webhook, the app URL that you registered.
You can find the example at [Publishing-Messages-and-Verifying-Results-in-Python](https://github.com/Fortellis/Publishing-Messages-and-Verifying-Results-in-Python).
Do the following to get the project running locally:

1. To download the GitHub repository, type the following in the directory of your choice:  

    ```curl
    git clone git@github.com:Fortellis/Publishing-Messages-and-Verifying-Results-in-Python.git
    ```

1. Change to that directory.  
    On the command line, type the following:  

    ```bash
    cd Publishing-Messages-and-Verifying-Results-in-Python
    ```

1. Create the virtual environment in Python.  
    1. Install the dependency to create virtual environments.  
        On the command line, type the following:  

        ```curl
        pip3 install virtualenv
        ```

    1. Create the virtual environment directory.  
        On the command line, type the following:  

        ```curl
        virtualenv venv
        ```

    1. Update the Python interpreter.  
        On the command line, type the following:  

        ```curl
        virtualenv -p python3.9 venv
        ```

    1. Activate the environment.  
        On Windows, type the following on the command line:  

        ```curl
        source venv/Scripts/activate
        ```

        On Mac, type the following on the command line:  

        ```curl
        source venv/bin/activate
        ```

    1. To install the Python modules, type the following in the project folder after downloading it to install the dependencies:

        ```curl
        pip install -r requirements.txt
        ```

1. To get your credentials, follow the instructions for registering your asynchronous API in [Registering Your Asynchronous API](/docs/tutorials/event-relay/publishing-messages-and-verifying-results-python#registering-your-asynchronous-api).
1. To include your credentials to get your token, replace the credentials in the `app.py` files with your `base64Encoded{yourClientID:yourClientSecret}`.
1. To get the subscribing organization's ID, follow the instructions in [Getting the Entity ID of an Organization](/docs/general/overview/organizations/#getting-the-entity-id-of-an-organization).
1. To replace the placeholder values, do the following in the `index.js` file:  
    * Replace the credentials with your [base64 encoded](https://www.base64encode.org/) `{clientID:clientSecret}`.  
    * Replace `{subscribingOrganizationId}` with the organization ID of the subscribing organization.  
    * Replace `'{aUniqueUUIDForYourPartition}'` with a `partitionKey` that you choose.  

You can now send a request to your endpoint to send a webhook.  
On the command line, type the following to start the script:  

```bash
python app.py
```

With your script running, type the following on a different command line to send the curl request:  

```curl
curl -X POST localhost:5000/event \
-H "Accept: application/json" \
-H "Cache-Control:no-cache"
```

Fortellis uses the token to identify you and send the event to your Subscribers.
