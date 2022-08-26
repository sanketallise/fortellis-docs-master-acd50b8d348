---
title: Publishing Messages and Verifying Results in Node.js
author: Nathaniel Sandberg
last updated: 6/15/2022
---

You can create your asynchronous API and start receiving your events at your webhooks.site with this tutorial.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Register your asynchronous API on Fortellis.
* Update your asynchronous API from the previous tutorial to do the following:
    * Accept a request to update the `payload.json`.
    * Get a Fortellis token with your **Client Key** and **Client Secret**.
    * Use the Fortellis token to send the event to Fortellis.
    > You must send the `Data-Owner-Id` in the header of the event to send the event to your `subscriptionId`.
    > You must send the `partitionKey` in the headers to send events in order,
    > but you may exclude the `partitionKey` if the order doesn't matter.
* Subscribe to your asynchronous API to see your event go through the platform.

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

## Registering Your Asynchronous API

You must register your asynchronous API to get your access token.
Fortellis uses your ID in the access token to route the events to your Subscribers.
Use a unique name for your asynchronous API.
Fortellis gets the name of the asynchronous API from the `title` in your spec.

1. Sign in to [Fortellis]($[devNetworkUrl]).
1. Choose the organization that your want to register the asynchronous API to.
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
    1. Click the organization.
    1. Click **OK**.
1. Click **New API**.  
    **Note:** You may see the option to choose a **Namespace** and **Environment**,
    but you can only use the production environment and the `/v2/events` endpoint for asynchronous APIs.
1. Click **Choose File**.
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
1. Click **Create API**.

From this page, you can do the following:

* Download the spec.
* Get the **Broker URL**.
* Edit your spec.
* Get your **Client ID** and **Client Secret**.
* Upload your **Documentation**.  
* Enter your **Support** information.
* Enter your **Pricing** information.
* Upload your **Terms**.  
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

For this tutorial, you only need to get your **Client ID** and **Client Secret**.
Follow the instructions in [Managing APIs](/docs/tutorials/api-lifecycle/updating-apis) to update the other fields in your API.

### Editing Your Spec

Users see your spec in the [API Directory]($[apiReferenceUrl]) if you make your asynchronous API public.

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

### Creating the Event Endpoint to Update the JSON File

Complete the [Developing an Asynchronous API](/docs/tutorials/event-relay/tutorial-develop-an-async-api) or download the existing repository from GitHub at [AsyncAPIHelloWorld](https://github.com/Fortellis/AsyncAPIHelloWorld).
Do the following if you download the repository from GitHub:

* Run **npm install** to install the dependencies.
* Replace the placeholder values with values from your account so that your API works with your registered API values.

For this tutorial, we are going to trigger the asynchronous API with a post request to update the `payload.json` file that you want to send to the Subscriber.

1. Start with the project that you have from [Developing an Asynchronous API](/docs/tutorials/event-relay/tutorial-develop-an-async-api/).
1. Create the endpoint to update the content of the JSON file.  
    Above `app.listen`, add the following to create the endpoint to update the `payload.json` file:

    ```javascript
    app.post("/updatePayload", (req, res) => {
        console.log('This is the request' + req.body);
        console.log('This is the response' + res);
    })
    ```

1. Send the response back that you received the request to update.  
    Under the log for the response, `res`, type the following to send the response:

    ```javascript
    res.send("Updating payload");
    ```

    **Note:** You must stop and start the server to get this step to work.
    You can get the response by using the following curl request.

    ```curl
    curl -X POST http://localhost:5000/updatePayload
    ```

1. Install the `body-parser` dependency.  
    On the command line, type the following for the body-parser dependency:  

    ```curl
    npm install body-parser
    ```

    **Note:** You may have to stop the server to install the dependency.
1. In the `index.js` file, include the `body-parser` dependency and the command to use it.  
    With the other constants at the top of the `index.js` file, type the following:

    ``` javascript
    const bodyParser = require('body-parser');
    app.use(bodyParser.json({extended:true}), express.json());
    ```

1. Write the payload to the `payload.json` file.  
    Below the response, write the following to save the request to the `payload.json` file:  

    ```javascript
    fs.writeFile('./payload.json', JSON.stringify(req.body, null, 2), function writeJSON(err) {
        if (err) return console.log(err);
    })
    ```

You can now send the update to the file and see your request update on your <https://webhook.site> URL.

#### Request

```curl
curl -X POST http://localhost:5000/updatePayload \
--header  "accept: application/json" \
--header  "Content-Type: application/json" \
-d '{"id":"6fff8732-9e18-4692-aa97-a4667fac7d31","number":17,"haveYouSaidHello":true,"waysToSayHello":["Guttendag","Ola"],"helloID":{"language":"German","id":"6fff8732-9e18-4692-aa97-a4667fac7d31"}}'
```

### Getting a Token from Fortellis to Send with the Update

You need to get a token to send your event to Fortellis and have Fortellis forward the event to your Subscribers.
Fortellis uses the token to authorize your event is from you and associate the event with your channel.

1. Install axios to send the token request to Fortellis.  
    On the command line, type the following:

    ```curl
    npm install axios
    ```

1. Include the dependency in the `index.js` file to send the request to Fortellis.  
    With the other dependencies in the `index.js` file, type the following:  

    ```curl
    const axios = require('axios');
    ```

1. Make `fs.watchFile` function an asynchronous function.  
    Add the word async before the function:

    ```javascript
    fs.watchFile('./payload.json', async function(event, filename){
    ...
    ```

1. Create the post request to get the token for your asynchronous API.  
    Beneath the `console.log('event is'...)`, type the following and replace the values for your API:

    ```javascript
    await axios.post('$[authServerUrl]/v1/token',
        {
            'grant_type': 'client_credentials',
            scope: 'anonymous'
        },
        {
            headers: {
                Accept: 'application/json',
                Authorization: 'Basic {yourClientKey}:{yourClientSecret}',
                'Cache-Control': 'no-cache'
            }
        } )
        .then(function (response){
            console.log(response.data.access_token);
        })
        .catch(function (error){
            console.log(error);
        })
    ```

    **Note:** If you restart your local server,
    you can now change the `payload.json` file and log the token to the console.
1. Create the global variable for the token.  
    With the other constants, type the following:  

    ```javascript
    let token;
    ```

1. Assign the token variable to the response value in the token.  
    In the `index.js` file, type the following after `console.log(response.data.access_token);`:  

    ```javascript
    token = response.data.access_token;
    ```

1. Add the token to the request to the event sent to Fortellis.  
    Change the `serverOptions` to the following:

    ```javascript
    ...
    const serverOptions = {
        uri: '$[eventRelay]',
        body: JSON.stringify(arrayOfObjects),
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Accept':'application/json',
            'Authorization': 'Bearer ' + token,
            'partitionKey': '{aUniqueUUIDForYourPartition}',
            'Data-Owner-Id': '{subscribingOrganizationId}',
            'X-Request-Id': '{aUniqueRequestId}'
        }
    }
    ...
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
        Follow the instructions below to create a UUID for each request.

> You can only send an event through the event relay to one organization at a time
> because you must include the `Data-Owner-Id` in the event header with the `organizationId`.

### Creating a Universally Unique Identifier

You can manually create a UUID for each event that you want to send,
but for this tutorial, we are going to create the UUID to individually attach to each request.

1. Install the `uuid` dependency.  
    On the command line, type the following:  

    ```curl
    npm install uuid
    ```

1. Import the dependency into the `index.js` file.  
    With your other constants, include the following:

    ```javascript
    const { v4: uuidv4 } = require('uuid');
    ```

1. Add the variable for the `Request-Id`.  
    Above the `serverOptions` constant, type the following to get a unique `requestId`:

    ```javascript
    let requestId = uuidv4();
    ```

1. Use the `requestId` variable in the `serverOption`.  
    Replace the `X-Request-Id` placeholder with the `requestId` variable.

    ```javascript
    ...
    'Data-Owner-Id': '{subscribingOrganizationId}',
    'X-Request-Id': requestId
    ...
    ```

You can now post your events with a UUID.

## Registering an App to Receive Events

You now need to register an app to use your asynchronous API to receive events from it.
For this portion, we will register the app with the webhook that we previously used in [Developing an Asynchronous API](/docs/tutorials/event-relay/tutorial-develop-an-async-api).

### Creating a New App

1. Sign in to the [Developer Network]($[devNetworkUrl]).
1. Choose the organization you want to register the app for.
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
    1. Click the organization.
    1. Click **OK**.
1. Click **New App**.

### Entering Your App Details

1. Enter the following information for your app:
    * **App Name**
    * **App Description**
    * **Website**
    * **Callback URL**  
        You only need to include the callback URL if you are using the authorization code flow or the implicit authorization flow.
        See [Implicit Flow for Authorization](/docs/tutorials/solution-integration/implicit-flow) and [Authorization Code Flow](/docs/tutorials/solution-integration/authorization-code) for more information.
1. Select **Async** for the **API Type**.
1. Type the name of your asynchronous API in the search field, and then select your API.
1. Click **Webhook**.  
1. Enter your webhook URL.  
    **Example:** `https://webhook.site/e75a2b4a-573d-49fd-aa09-44e074d00f73`
1. Click **Save**.
1. Click **Register**.

Once you have registered your app on your behalf,
you can send events from your asynchronous API to your app and see them go to your webhook.

> Currently, a Fortellis Administrator must create the subscription to your app and activate it
> before your event sends events to your app webhook.
> Contact [support@fortellis.io](mailto:support@fortellis.io) to ask an administrator to create your subscription and activate it.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Register your asynchronous API on Fortellis.
* Update your asynchronous API from the previous tutorial to do the following:
    * Accept a request to update the `payload.json` file.
    * Get a Fortellis token with your **Client Key** and **Client Secret**.
    * Use the Fortellis token to send the event to Fortellis.
* Subscribe to your asynchronous API to see your event go through the Fortellis Event Relay.

You can now send events through Fortellis and see them going to your webhook, the app URL that you registered.
You can find the example at [Fortellis-asynchronous-API-implementation](https://github.com/Fortellis/Fortellis-asynchronous-API-implementation).
Do the following to get the project running locally:

1. To download the GitHub repository, type the following:  

    ```curl
    git clone git@github.com:Fortellis/Fortellis-asynchronous-API-implementation.git
    ```

1. Change directories to the cloned repository.  
    On the command line, type the following:  

    ```bash
    cd Fortellis-asynchronous-API-implementation
    ```

1. To install the `node_modules`, type the following in the project folder after downloading it to install the dependencies:

    ```curl
    npm install
    ```

1. To get your credentials, follow the instructions for registering your asynchronous API in [Registering Your Asynchronous API](/docs/tutorials/event-relay/publishing-messages-and-verifying-results#registering-your-asynchronous-api).
1. To get the subscribing organization's ID, follow the instructions in [Getting the Entity ID of an Organization](/docs/general/overview/organizations/#getting-the-entity-id-of-an-organization).
1. To replace the placeholder values, do the following in the `index.js` file:  
    * Replace the credentials with your [base64 encoded](https://www.base64encode.org/) `{clientID:clientSecret}`.  
    * Replace `{subscribingOrganizationId}` with the organization ID of the subscribing organization.  
    * Replace `'{aUniqueUUIDForYourPartition}'` with a `partitionKey` that you choose.  

1. Start the server.
    On the command line, type  the following:  

    ```bash
    node index.js
    ```

You can now update the content in the `payload.json` file and save it to send an event.

Fortellis uses the token to identify you and send the event to your Subscribers.
