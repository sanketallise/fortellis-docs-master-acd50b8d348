---
title: Admin API Tutorial in Python
author: Nathaniel Sandberg
last updated: 11/5/2021
---

Fortellis has created a schema that API Developers can use to integrate with Fortellis.
API Developers may want to integrate and implement that schema for the following reasons:

* You can use the schema to protect your APIs and vet users.
* Fortellis only sends requests from users that you approve if you implement the Admin API.

You can use the [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs) to receive requests from Fortellis at your endpoint,
    and you can use the [Connection Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback) to approve those requests.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create the `/activate` endpoint.
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Deploy your Admin API implementation to Heroku.
* Use GitHub for your code repository.  

You can use the repository software of your choice,
but GitHub has an out-of-the-box Heroku integration.

### Things to Remember in This Tutorial

You should remember the following:

* Use the following to start the server:  

    ```curl
    python app.py
    ```

* Use **Ctrl**+**C** to stop the server.
* Install Python before starting the tutorial.
* Create an API on Fortellis before you start.  
    **Note:** For more information on registering your API, see [Registering APIs](/docs/tutorials/api-lifecycle/registering-apis).
* View the [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs) for the exact spec and endpoints.

You can find a completed project with the example on GitHub: [python-admin-api](https://github.com/Fortellis/python-admin-api).  

## Workflow

1. Enter your Admin API URL.
    when you are updating your API.
1. Implement the Admin API spec to receive requests at that endpoint.
    when Fortellis calls your Admin API URL.
1. Use an API or another method to get the connection requests.
    that Fortellis sends to your Admin API URL.
1. Send the connectionId to `$[connectionCallbackEndpoint]/connections/:connectionId/callback` endpoint to accept the request.  

You do not have to take any action to refuse the connection.

## Creating and Updating the Virtual Environment

1. Create the folder for the new project.  
    On the command line, type the following to make the directory:  
    **mkdir python-admin-api**
1. Change directories to your new project.  
    On the command line, type the following:  
    **cd python-admin-api**
1. Install the dependency to create virtual environments.  
    On the command line, type the following:  
    **pip3 install virtualenv**
1. Create the virtual environment directory.  
    On the command line, type the following:  
    **virtualenv venv**
1. Update the Python interpreter you are using.  
    On the command line, type the following:  
    **virtualenv -p python3.9 venv**
1. Activate the environment.  
    On Windows, type the following on the command line:  
    **source venv/Scripts/activate**  
    **Or**  
    On Mac, type the following on the command line:  
    **source venv/bin/activate**  

1. Create the app.py file.  
    On the command line, type the following:  

    ```curl
    touch app.py
    ```

You can now see `(venv)` indicating that your environment is running.
To stop the environment, type **deactivate** at any time.

## Creating the Health Check Endpoint

You do not have to deactivate Python to complete the following steps.

1. Install Flask.  
    On the command line, type the following:  
    **pip install flask**.
1. Install Flask Restful.  
    On the command line, type the following:  
    **pip install flask-restful**
1. Freeze your dependencies.  
    On the command line, type the following:  
    **pip freeze > requirements.txt**
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

## Implementing the Admin API

Fortellis calls the `/activate` endpoint after a subscriber configures their app to use your API.
Fortellis will include the following:

* `organizationInfo`
* `appInfo`
* `userInfo`
* `apiInfo`
* `connectionId`

The `connectionId` uniquely identifies the request,
and you'll use it to accept the request using Fortellis Connection Callback API,
which we will cover later in this tutorial.
You must authorize requests to your `/activate` endpoint.
See below for more information.

### Setting the Activate Endpoint to Receive Requests

1. Log the request in the console.  
    Beneath the `Health` class, add the `Activate` class.

    ```python
    class Activate(Resource):
        def post(self):
            app.logger.debug('Headers: %s', request.headers)
            app.logger.debug('Body: %s', request.get_data())

    api.add_resource(Activate, '/activate')
    ```

    Consider the following:
    * You will probably want to send this information to your database, but we use a JSON file in this tutorial.
    * You can then get the subscriber's information from the database when you are verifying your subscriptions.

1. Create the response that Fortellis expects to receive when sending activation requests.  
    Type the following in the `app.py` file after the loggers:

    ```python
    return {"links":[{"href":"string","rel":"string","method":"string","title":"string"}]}
    ```

You can now call the endpoint from Postman or any other API testing tool at `http://localhost:5000/activate`:

* Use the Postman collection that you get when you initially submit your API for testing.
* Replace the URL in the activation request with the `http://localhost:5000/activate`.
* Continue to call the endpoint throughout the tutorial to test it before moving on to the next steps.
* Use the payload that you will get when Fortellis sends activation requests to your `activate` endpoint found in the test collection.

#### Request to the Server

```curl
curl -X POST http://localhost:5000/activate -H "accept: application/json" \
-H  "Content-Type: application/json" -d '{"organizationInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"appInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"entityInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"solutionInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"userInfo":{"fortellisId":"user@example.com"},"apiInfo":{"id":"v1-appointments","name":"Appointments","implementationName":"Fortellis Spec 8"},"subscriptionId":"2529a384-acc3-4b6a-a835-ed09e77dee15","connectionId":"2529a384-add3-4b6a-a935-ed09e45def12"}'
```

#### Response from the Server

```json
{
    "links": [
        {
            "href": "string",
            "method": "string",
            "title": "string",
            "rel": "string"
        }
    ]
}
```

### Verifying the Token

You should verify the tokens that Fortellis sends with activation requests.

1. Before you download more dependencies, stop the server.  
    On the command line, type the following:  
    **Ctrl**+**C**
1. Install the `PyJWT` dependency.  
    On the command line, type the following:  
    **pip install pyjwt**
1. To decode the RS256 token, include the cryptography as well.  
    On the command line, type the following:  
    **pip install pyjwt[crypto]**
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
    Above `return {"links":[{"href":"string","rel":"string","method":"string","title":"string"}]}`, add the following:

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
        options={"verify_exp": True},
    )

    print(data)

    ```

    The constants define the following:
    * The JWT dependency
    * The Fortellis token domain
    * The token verifier
    * The audience in the token

If you send the same request as before,
you should receive a forbidden error.

### Successfully Sending a Request to Your Local Admin Implementation

You can now get a token from Fortellis and send requests to your local Admin API implementation.

1. Get a token from Fortellis.  
    In the Postman collection in the Admin API Testing folder,
    click the **Generate Infrastructure Bearer Token**,
    and then click **Send** to get your token.
1. Copy the `access_token` that you receive in the response body.
1. Call your local endpoint `http://localhost:5000/activate` with the Bearer token in the header.  
    In the `/activate` request in Postman, click **Authorization**,
    and paste your token into **Token**.  
    **Note:** The token expires in one hour.
    You should now be able to successfully send a request.

```curl
curl -X POST http://localhost:5000/activate -H "accept: application/json" -H  "Content-Type: application/json" -d '{"organizationInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"appInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"entityInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"solutionInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"userInfo":{"fortellisId":"user@example.com"},"apiInfo":{"id":"v1-appointments","name":"Appointments","implementationName":"Fortellis Spec 8"},"subscriptionId":"2529a384-acc3-4b6a-a835-ed09e77dee15","connectionId":"2529a384-add3-4b6a-a935-ed09e45def12"}' -H "Authorization: Bearer {yourToken}"
```

### Saving the Requests to a JSON File

You should probably save the `subscriptionId` to a database,
but for this tutorial, we'll use a JSON file.
You can use the `Subscription-Id` in the header to identify the subscriber
that you have already approved and map them to their information.

1. Create the JSON import in the `app.py` file.  
    At the top of the file with the other constants, type the following:  
    **import json**
1. Add the data to a JSON file in the `connectionRequest` object.  
    Under `print(data)`, type the following to add the data to the JSON file:

    ```python
    entry = json.loads(request.get_data().decode())

    with open('connectionRequests.json', 'r+') as file:
        file_data = json.load(file)
        file_data['connectionRequests'].append(entry)
        file.seek(0)
        json.dump(file_data, file, indent = 2)
    ```

1. Create the `connectionRequests.json` file with the following `connectionRequests` object
    that stores your new connection requests:

    ```json
    {"connectionRequests":[]}
    ```

You can now send the payload to your activate endpoint and store it in your `connectionRequests.json` file.

### Validating Data to Your Activate Endpoint

With validation, you can protect your backend systems from bad data and data that may have been tampered with or may compromise your system.
With a real implementation of the Admin API endpoints, you can validate the data to ensure you are only getting requests in a valid format.
With the following steps, you can ensure you are only receiving requests to your API from Fortellis and real Subscribers.

1. Stop the server running.  
    On the command line, type the following:  
    **Ctrl**+**C**
1. Install the dependency to validate the request payload.  
    On the command line, type the following:  
    **pip install jsonschema**
1. Include the dependency at the top of the json file with the other dependencies to include the `validate` and the `jsonschema` dependencies.  
    Under the other dependencies, type the following:

    ```python
    import jsonschema
    from jsonschema import validate
    ```

1. Create the schema file for activation requests on a site, such as [JSONschema.net](https://www.jsonschema.net), using the activate payload.  
    **Note:** You can use the payload below to make the schema.

    ```json
    {
        "organizationInfo": {
            "id": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d",
            "name": "Jareds Entity TBD",
            "address": "6701 Burnet Rd Austin TX 78757 US",
            "countryCode": "US",
            "phoneNumber": "2819234655"
        },
        "appInfo": {
            "id": "575d7513-2d43-4f10-a07b-c8e6c3c5ab3a",
            "name": "b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf Testing App",
            "developer": "Jared.Jones@cdk.com",
            "contactEmail": "Jared.Jones@cdk.com",
            "appOrgId": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d"
        },
        "entityInfo": {
            "id": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d",
            "name": "Jareds Entity TBD",
            "address": "6701 Burnet Rd Austin TX 78757 US",
            "countryCode": "US",
            "phoneNumber": "2819234655"
        },
        "solutionInfo": {
            "id": "575d7513-2d43-4f10-a07b-c8e6c3c5ab3a",
            "name": "b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf Testing App",
            "developer": "Jared.Jones@cdk.com",
            "contactEmail": "Jared.Jones@cdk.com",
            "appOrgId": "b74c3f9a-17ee-4943-81f2-ae22bb4f260d"
        },
        "userInfo": {
            "fortellisId": "test.email@cdk.com"
        },
        "apiInfo": {
            "id": "api-v2-b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf",
            "name": "api-v2-b6bc47a9-dbda-4c4f-81cb-8e5e29ff04bf",
            "implementationName": "Fortellis Spec 8"
        },
        "subscriptionId": "568a62f4-a7c3-468c-973c-004e6cc50a9b",
        "connectionId": "5b8a6242-a1aa-42c5-bec8-4eaa634d5470"
    }
    ```

1. Save the schema file as `schema.json` in the root directory.  
1. Remove the following from the schema in the `schema.json` file:
    * `type` in schema
    * `$id` for each of the properties
    * `examples`
1. Add the validation to check the incoming payload validate against the schema.  
    Above the `entry` variable, type the following:

    ```python
    with open('schema.json') as json_file:
        payloadSchema = json.load(json_file)
        validate(instance=request.get_data().decode(), schema=payloadSchema)
    ```

You now are verifying that all your requests are valid.
To verify, you can send the following:

* The payload from the example you created the schema from
    * You should see a new entry in your `connectionRequests.json` file.
* The payload from the example missing any comma or other punctuation
    * You should see an error.
* A completely different payload unrelated to the Admin API
    * You should see an error.

## Creating the Endpoint to Delete the Request

You do not need to take any action to reject a connection.
For this step, we simply remove the request,
and Fortellis will never connect the subscriber to the API.

### Adding the Delete Endpoint

1. To delete the request, add create an endpoint.  
    To add the `Delete` class below the `Activate` class, include the following:

    ```python
    class Delete(Resource):
        def post(self):
            with open('connectionRequests.json', 'r+') as file:
                parsed = json.load(file)
                print(json.dumps(parsed, indent=2, sort_keys=True))

            return {"status": "200 - Success"}

    api.add_resource(Delete, '/delete' )

    ```

1. Add the constant to get the `subscriptionId` from the request and print the `connecitonRequest.json` file to the console.  
    Below the print command, type the following;

    ```python
    app.logger.debug('These are the current conneciton requests: %s', parsed)
    app.logger.debug('Body: %s', request.get_data().decode())

    ```

You can now see the information on the `subscriptionId`
that you send to your endpoint in the body.
You can also see the JSON data in the `connectionRequests.json`.
Send the following request to see the `connectionRequests` and the `subscriptionId` in the console:

```curl
curl -X POST http://localhost:5000/delete -d '{"subscriptionId":"809f9dc3-c85b-497d-8ea0-edfcd3cc44eb"}'
```

### Writing the Updated JSON to the File

To update the information, we are going to remove the object associated with the `subscriptionId` that we get in the body of the request.

1. Get the `subscriptionId` from the header.  
    After decoding the data in the body, get just the `subscriptionId` with the following:

    ```python
    app.logger.debug('This is the `subscriptionId in the body: %s', request.json['subscriptionId'])
    ```

1. Use the `subscriptionId` for the deleted request and create a new JSON array.  

    ```python
    subscriptionIdInRequest = request.json['subscriptionId']
    connectionRequestsList = parsed['connectionRequests']
    newListOfConnectionRequests = [i for i in connectionRequestsList if not (i['subscriptionId'] == subscriptionIdInRequest)]
    app.logger.debug('This is the new list after removing the item in the request: %s', newListOfConnectionRequests)
    ```

1. Remove the contents of the `connectionRequests.json` file.  
    Below the log for the `newListOfConnectionRequests`, type the following to delete the contents of the JSON file:

    ```python
    file.truncate(0)
    ```

1. Push the new JSON array to your `connectionRequests.json` file.  
    Below `file.truncate(0)`, type the following to push the array to your JSON file.

    ```python
    file.seek(0)
    json.dump({"connectionRequests": newListOfConnectionRequests }, file, indent=2)
    ```  

You should now be able to delete the request by sending the `subscriptionId` in the body of your request locally.

```curl
curl -X POST http://localhost:5000/delete -H "accept: application/json" \
-H  "Content-Type: application/json"  -d '{"subscriptionId":"{yourSubscriptionId}"}'
```

### Creating an Endpoint to Get Your New Requests

You are now going to get your new requests.
With each `connectionId`, you can send the response back to Fortellis to accept the connection request.
You can use the information in the connection request to save information about your Subscribers and reduce the latency of your API.

* Create a new class to get the requests that you have pending.  
    In the `app.py` file, create a new class below the `Delete` class:

    ```python
    class ConnectionRequests(Resource):
        def get(self):
            with open('connectionRequests.json', 'r+') as file:
                parsed = json.load(file)

            return parsed
    api.add_resource(ConnectionRequests, '/connectionRequests')
    ```

You can now get a list of your request with the following:

```curl
curl http://localhost:5000/connectionRequests \
-H "Accept: application/json" \
-H  "Content-Type: application/json"
```

You can now send the `connectionId` to Fortellis to accept connection requests and delete connection requests once you have addressed them by either accepting or decided to refuse. Now, we need to deploy the python app to get the connections from Fortellis.

```curl
curl --location --request POST '$[subscriptionCallbackUrl]/connections/{connectionId}/callback' \
--header 'Content-Type: application/json' \
--data-raw '{"status": "accepted","error": "string"}' \
--header 'Authorization: Bearer {yourBearerToken}}'
```

## Deploying to Heroku

You must make the implementation available on a public URL to receive requests from Fortellis.
Follow the steps below to get your requests from Fortellis.
Remember that Heroku lets the server sleep after 30 minutes of inactivity.
You may want to use a more robust deployment for your production APIs.

### Creating a GitHub Repository

You may have to get a personal access token
if you are using your personal account.
See [Creating a personal access token](https://docs.github.com/en/authorization/keeping-your-account-and-data-secure/creating-a-personal-access-token) to create your token.
You should enter this token instead of your GitHub password
when you are pushing repositories to GitHub.

1. Stop the Python server if it is still running.  
    On the command line, type the following:  
    **Ctrl**+**C**
1. Sign up for a GitHub account at <https://github.com/>.
1. In the upper-left corner, click the **New** button.
1. Type the **Repository name**.
1. Type a **Description**.  
1. Click **Create Repository**.
1. From the command line, type the following:  
    **git init**  
    **git add ***  
    **git commit -m "Initial Commit"**  
    **git remote add origin {your repository}**  
    **Note:** Get the repository name from GitHub.  
    **git push -u origin master**
1. Enter your the following when GitHub prompts use:  
    * Your GitHub username.
    * Your GitHub password or your authentication code in the password field.

### Setting Up the Package Dependencies

1. Create the `Procfile` for Heroku to read the requirements for your app from.  
    On the command line, type **echo "web: gunicorn app:app" > Procfile**.
1. Install the dependencies to `gunicorn` from the previous step to deploy to Heroku.  
    On the command line, type **python -m pip install gunicorn==20.0.4**.
1. Freeze the requirements again so Heroku can see your dependencies.  
    On the command line, type **python -m pip freeze > requirements.txt**.
1. Tell Heroku which version of python to run.  
    On the command line, type **echo "python-3.9.0" > runtime.txt**.
1. Add the changes to GitHub.  
    On the command line, use the following command to push to GitHub and enter your password or personal token to complete the process:  
    * **git add *** to add the files
    * **git commit -m "Add Heroku deployment files"** to commit the files
    * **git push** to push the files to GitHub

### Creating the App in Heroku

1. Create a [Heroku](https://heroku.com) account if you don't already have one.
1. In the upper-right corner, click **New**.
1. Click **Create new app**.
1. Type the app name.  
    **Note:** You must use a unique app name.
1. Click **Create app**.

### Integrating Your Heroku App with GitHub

1. In the middle of the page, click **GitHub**.
1. Click **Connect to GitHub** to connect your account to your GitHub account.  
1. At the prompt, click **Authorize heroku** to grant access to your GitHub account.  
1. Type the repo name.  
1. Click **Search**.  
    **Note:** Make sure your Herko account has the right settings to connect to your GitHub account.  
1. Click **Connect**.  
1. At the bottom of the page, click **Deploy Branch**.  
    **Note:** You may have to wait a few moments as Heroku downloads the dependencies.
    You may have to wait for the Heroku app to fully provision as well.

You can now click **View** at the bottom of the page to see your app.
You should see an internal server error at this point,
but you can now type `/health` after that to see `status: UP`.

## Adding the Admin API URL to Your API

1. Sign in to the [Developer Network]($[devNetworkUrl]).
1. Do the following to select the organization that the API belongs to:
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
    1. Click the organization that the API belongs to.
    1. Click **OK**.
1. Click **View All** next to **APIs**.
1. Click the API you want to add the **Admin API URL** to.
1. Click **Edit** next to **Subscription Activation** to change the method to **Admin API**.
1. Click **Admin API**, and enter the **Admin API URL**.
1. Click **Save**.

## Approving Connection Requests

You need the `connectionId` from the activation request made by Fortellis.
The `connectionId` uniquely identifies the connection in Fortellis' system.

Store the connection request in a database to do the following:

* Verify subscribers.
* Maintain subscription information.
* Improve performance of your API.

Store the connection request in a database to do the following:

* Verify subscribers
* Maintain subscription information
* Improve performance for your API

You have to periodically check your `/connectionRequests` endpoint.
You cannot receive connection requests if the Heroku server is asleep.
The Heroku server falls asleep after 30 minutes of inactivity.

1. Use the `/connectionRequests` endpoint to get your connections.
1. Using your Fortellis Client Key and Client Secret, get your Fortellis token from `$[authServerUrl]`.  
    See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) for more information.
1. Create a new request in Postman.
1. Under **Auth**, enter your `access_token`.
1. Use the following URL to accept the connection: `$[subscriptionCallbackUrl]/connections/{connectionId}/callback`.

    ```bash
    curl --location --request POST '$[subscriptionCallbackUrl]/connections/:connectionId/callback' \
    --header 'Content-Type: application/json' \
    --data-raw '{"status": "accepted","error": "string"}' \
    --header 'Authorization: Bearer {your token}'
    ```

    **Note:** You should receive the following back: `OK`.

1. After you approve connections, send the JSON object for the request that you approved to the `deleteRequest` endpoint to remove approved entries.  

## Deactivation Requests

The Subscriber may request to deactivate the subscription for the following reasons:

* The Subscriber is going out of business.  
* The Subscriber no longer uses the solution that works with that API.
* The Subscriber wants to change API Developers and moves to a different API.

API Developers may request to have a subscriber deactivated through Fortellis by contacting [Fortellis Support](mailto:support@fortellis.io).

### Creating the Deactivate Endpoint

API Developers receive the `connectionId` at the `/deactivate` endpoint and can clean up their backend system when they are ready.

1. Copy the `Activate` class and resource endpoint, and then paste a copy below it.
1. Remove the validate schema function from it.
1. Change `/activate` to `/deactivate/<string:connectionId>`.
1. Change the class and the resource name to **Deactivate**, and then include the **connectionId** variable with `self`.  

    ```python
    class Deactivate(Resource):
        def post(self, connectionId):
    ```

1. Change `'connectionRequests.json` to `deactivationRequests.json` in the open call.

    ```python
    with open('deactivationRequests.json', 'r+') as file:
    ```

1. To append the `connectionId` to the `deactivationRequests` array, change `connectionRequests` to `deactivationRequests` and `entry` to `connectionId`.  

    ```python
    ...
    entry = connectionId
    ...
    file_data['deactivationRequests'].append(entry)
    ```

1. Create a new file `deactivationRequests.json`, and include the following JSON object in the file:

    ```json
    {"deactivationRequests":[]}`
    ```

You can now see the deactivation request going to your `deactivationRequests.json` file locally.
You may need to stop and restart the server to see the `connectionId` save to the `deactivationRequests.json` file.

```python
class Deactivate(Resource):
    def post(self, connectionId):
        token = request.headers['Authorization'][len(PREFIX):]

        url = "$[authServerUrl]"

        jwks_client = PyJWKClient(url)

        signing_key = jwks_client.get_signing_key_from_jwt(token)

        unverified_header = jwt.get_unverified_header(token)

        data = jwt.decode(
            token,
            signing_key.key,
            algorithms=["RS256"],
            audience="api_providers",
            options={"verify_exp": True},
        )

        print(data)

        entry = connectionId

        with open('deactivationRequests.json', 'r+') as file:
            file_data = json.load(file)
            file_data['deactivationRequests'].append(entry)
            file.seek(0)
            json.dump(file_data, file, indent = 2)

        return {"links":[{"href":"string","rel":"string","method":"string","title":"string"}]}

api.add_resource(Deactivate, '/deactivate/<string:connectionId>')
```

You can use a curl request to make sure you are receiving the deactivation requests.
Make sure to get a new token when you are calling the `deactivate` endpoint.

```curl
curl -X POST http://localhost:5000/deactivate/yourDeactivationRequest -H "accept: application/json" \
-H  "Content-Type: application/json" -H "Authorization: Bearer {yourToken}"
```

### Getting the Connections to Deactivate

1. Copy the `Connection Requests` class and the resource and create a copy underneath it.
1. In the class, change `'/connectionRequests'` to `'/deactivationRequests'`.
1. In the open file command, change `connectionRequests.json` to `deactivationRequests.json`.
1. Change the class and the resource to `DeactivationRequests`.
1. Use Postman or another API testing tool to get the connections that have been deactivated.

You can now receive deactivation requests and remove them from your backend systems.
Use the following curl request to get your deactivation requests.

```curl
curl http://localhost:5000/deactivationRequests -H "accept: application/json" \
-H  "Content-Type: application/json"
```

Push your repository to GitHub and redeploy to Heroku to use the deactivate endpoint on the Heroku server.

## Conclusion

You should have learned how to do the following in this tutorial:

* Create the `/activate` endpoint.
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Deploy your Admin API implementation to Heroku.
* Use GitHub for your code repository.  

You can extend this tutorial to store information about your users and confirm the user's `Subscription-Id`
when you receive requests.
Remember that the Heroku server sleeps after 30 minutes,
and you may need to implement a more scalable solution for a production API.

You can find the example code in the [python-admin-api](https://github.com/Fortellis/python-admin-api)  repository on GitHub.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create the `/activate` endpoint.
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Deploy your Admin API implementation to Heroku.
* Use GitHub for your code repository.  

Store the information you receive when you get an activation request to find subscriptions and other information you need to process requests.
Remember the following about the Heroku deployment:

* The Heroku server sleeps after 30 minutes.
* You may need to implement a more scalable solution for a production environment.

You can download the project from GitHub at [python-admin-api](https://github.com/Fortellis/python-admin-api).

If you choose to download the example repository, do the following:

1. In the directory of your choice download the repository from GitHub.  
    On the command line, type the following:  

    ```curl
    git clone git@github.com:Fortellis/python-admin-api.git
    ```

1. Change directories to the project folder.  
    On the command line, type the following:  

    ```curl
    cd python-admin-api
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

1. Install the dependencies.  
    On the command line, type the following:  

    ```curl
    pip install
    ```

1. Follow the instructions in [Deploying to Heroku](#deploying-to-heroku).  
    1. Follow the instructions in [Creating a GitHub Repository](#creating-a-github-repository).  
    1. Follow the instructions in [Setting Up the Package Dependencies](#setting-up-the-package-dependencies).  
    1. Follow the instructions in [Creating the App in Heroku](#creating-the-app-in-heroku).  
    1. Follow the instructions in [Integrating Your Heroku App with GitHub](#creating-the-app-in-heroku).  
1. Follow the instructions in [Adding the Admin API URL to Your API](#creating-the-app-in-heroku).  
1. Do one of the following:  
    * To approve connection requests, follow the steps in [Approving Connection Requests](#approving-connection-requests).
    * To get deactivation requests, follow the steps in [Getting the Connections to Deactivate](#getting-the-connections-to-deactivate).
