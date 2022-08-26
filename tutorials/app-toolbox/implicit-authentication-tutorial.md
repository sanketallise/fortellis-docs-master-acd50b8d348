---
title: Node.js Implicit Authorization Tutorial
author: Nathaniel Sandberg
last updated: 4/25/2022
---

In this tutorial,
we create an app that uses the implicit flow to authorize users with Fortellis tokens.

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' implicit flow is not secure in the context of JavaScript apps and single-page apps.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create the starter project.
* Install dependencies.
* Register the app on Fortellis.
* Start the project.
* Log in to get your token.

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
    git clone git@github.com:Fortellis/ImplicitFlow-ExampleApp.git
    ```

    **Note:** You can find the [ImplicitFlow-ExampleApp](https://github.com/Fortellis/ImplicitFlow-ExampleApp) on GitHub for more information.

1. Change directories to the `ImplicitFlow-ExampleApp` folder.  
    On the command line, type the following:  

    ```curl
    cd ImplicitFlow-ExampleApp
    ```

1. Install the dependencies.  
    On the command line, type the following:

    ```curl
    npm install
    ```

    **Note:** You may have to wait a moment for the installation to complete.
    Feel free to move on to the next steps while that's in progress.

## Registering the App on Fortellis

Register the website using `http://example.com` and the callback URL with `http://localhost:3000`.

1. Register your app by following the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps).
1. Follow [OAuth on Fortellis](/docs/tutorials/solution-integration/auth) to get your app credentials.
1. In the `ImplicitFlow-ExampleApp/src/App.js` file, replace the `client_id` with the **API Key** that you get from Fortellis.
1. Change the `encodeURIComponent` to `http://localhost:3000/`.
1. Start the app.  
    On the command line, type the following:  

    ```curl
    npm run start
    ```

## Logging In

1. When the app opens, click **Login with Fortellis**.
1. Type in your credentials.
1. Click **Sign In**.  
    **Note:** Fortellis resolves all URLs to a secure connection,
    so you receive an error stating that the site cannot create a secure connection.
1. Delete the `s` in `https` in the URL and press **Enter**.

You can now see your token in the URL and you are logged in to the app.

## Overview of How It Works

Make the following observations about the implicit authorization flow in the app code:

* When you sign in through Fortellis,
    you receive a token in the URL.
* To remove the user's access to the token,
    you add the empty string `""` to the `window.history.replaceState` at the beginning to remove the hash fragment from the URL.
* You assign the token variable at the beginning of the document in `let token`.
* You capture the token in the following:

    ```javascript
    token =
      window.location.href.match(/access_token=(.*?)&/) &&
      window.location.href.match(/access_token=(.*?)&/)[1];
    ```

* The component conditionally renders the **Get Vehicles** button
    if the user is signed in
    or renders the **Login with Fortellis** button
    if the user is not signed in.
* When users click the **Login with Fortellis** button,
    they use the implicit workflow to get the token using the URL in the `href`.

You must follow the rest of the steps in [Create Your First App](/docs/tutorials/app-toolbox/create-an-app) to click the button and get photos of vehicles.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create the starter project.
* Install dependencies.
* Register the app on Fortellis.
* Start the project.
* Log in to get your token.

You can now start creating your project using the implicit authorization flow to get a token.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
