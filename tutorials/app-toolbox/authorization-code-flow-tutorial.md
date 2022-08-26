---
title: Node.js Authorization Code Tutorial
author: Nathaniel Sandberg
last updated: 2/24/2022
---

You can quickly get up and running with the following app that uses the authorization code flow.
You would want to use an authorization code when you have a backend server and a method for getting tokens from Fortellis.

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' authorization code flow is not secure in the context of JavaScript apps and single pages apps.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create the starter project.
* Install dependencies.
* Register the app on Fortellis.
* Start the project.
* Log in to get your token.

### Things to Remember in This Tutorial

You should remember the following:

You should remember the following:

* Use the following to start the server:  

    ```curl
    node index.js
    ```

* Use **Ctrl**+**C** to stop the server.
* Install the following before starting the tutorial:
    * Node
    * NPM
* Use a Fortellis account to sign in and get a token.

## Creating the Starter Project

1. Create a new folder.  
    On the command line in the directory of your choice, type the following to create the new directory
    where you want to create your project:

    ```curl
    mkdir {yourProjectName}
    ```

1. Change to that directory.  
    On the command line, type the following to access that directory:

    ```curl
    cd {yourProjectName}
    ```

1. On the command line, type the following to clone the repository for the project:

    ```curl
    git clone git@github.com:Fortellis/authorization-code-example-app.git
    ```

    **Note:** You can find the [authorization-code-example-app](https://github.com/Fortellis/authorization-code-example-app) on GitHub for more information.

## Installing the Dependencies

1. Change directories to the example app.  
    On the command line, type the following to access the cloned repository:

    ```curl
    cd authorization-code-example-app
    ```

1. Install the express dependencies.  
    On the command line, type type the following to install the dependencies:

    ```curl
    npm install
    ```

1. Change directories to the client directory.  
    On the command line, type the following to access the client folder:

    ```curl
     cd client
     ```

1. Install the dependencies in the client directory.  
    On the command line, type the following to install the dependencies in the client folder:

    ```cur
    npm install
    ```

## Registering the App on Fortellis

In this process, register the app with the **Website** `http://example.com` and the **Callback URL** `http://localhost:5000`.
You can then see the authorization code in the URL and get the token back.

1. Follow the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps) to register your app.  
1. Follow the instructions in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) to get your credentials.
1. In the `authorization-code-example-app/client/src/App.js` file, replace the `client_id` with the **API Key** that you get from Fortellis.
1. Change the `encodeURIComponent` to `http://localhost:5000/`.  
    **Note:** Include the final slash after the URL.
1. In the `authorization-code-example-app/server.js` file, change the following:  
    * Replace the `CLIENT_ID` with your **API Key** from the Fortellis UI.  
    * Replace the `CLIENT_SECRET` with your **API Secret** from the Fortellis UI.  
    For more on the authorization code and encoding, see [Authorization Code Flow](/docs/tutorials/solution-integration/authorization-code-flow).
1. In `authorization-code-example-app/client` folder, build the app.  
    On the command line, type the following to build the react app:

    ```curl
    npm run build
    ```

1. Change directories back to the root directory.  
    On the command line, type the following:

    ```curl
    cd ..
    ```

On the command line, type the following to start the app and see it running:

```curl
npm run start
```

## Viewing Your Token

You can now see the token that you get back in the browser when you do the following:

1. In your browser, sign out of any sessions that are open on Fortellis.  
    **Note:** You must complete this step so that you do not get an old token when attempting to log in.
1. Go to `http://localhost:5000`.
1. Click **Login with Fortellis**.
1. Enter your credentials in the Fortellis UI.
1. Click **Sign In** in the Fortellis UI.  
    Fortellis sends you back to the URL that you used for the **Callback URL** when you registered the app.
1. Click the **Get Your token** button.

You can now see your Bearer token returned in the console of your browser and on the command line.
To access Fortellis, use the access token. In a real application, you would store this in the browser to send with every request to the API.

## Overview of How It Works

Make the following observations about the authorization code flow:

* You store your `API Key` and `API Secret` in the values in the `server.js` file.
* The `index.js` file base 64 encodes these to give to the API.
* You store your base 64 encoded credentials on the express app in the `authorization` header.
* When you click **Login with Fortellis**,
    you use your encoded App Developer credentials to get an authorization code.
* The app captures the code in the `let code` variable at the top of the `authorization-code-example-app/client/src/App.js`.  
    **Note:** You can see your authorization code in the URL when you click **Login with Fortellis**.
* Users click **Get Your Token** to get the token.  
    **Note:** In an application, you would automate this step,
    but you may increase latency with the added step.
* The React app sends the token to the express server.
* The express server forwards the authorization code to Fortellis to retrieve the actual authorization token.
* The express server forwards the response from Fortellis to the React app.
* The browser shows the token in the console.

You now have a working model of the authorization code and how it works.

You can now do the following:

* Serve a react app on an express server.  
* Use the express server to proxy requests to Fortellis.  
* Receive an access token to use apps.  

You can get started on Fortellis right away using this or one of the other authorization flows today.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create the starter project.
* Install dependencies.
* Register the app on Fortellis.
* Start the project.
* Log in to get your token.

You can now use the authorization flow to get your token and start using your app.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
