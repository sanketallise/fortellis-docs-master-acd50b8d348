---
title: Node.js Client Credentials Authorization Tutorial
author: Nathaniel Sandberg
last updated: 6/9/2021
---

You can use this tutorial to start creating an app that uses the client credentials flow.
See below to download the example and get started creating your app with the client credentials flow.

**Note:** If you use the client credentials in a production app,
you cannot expose your API key and secret to users.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Clone a Node project from a repository.
* Install the dependencies for the project.
* Register an application on Fortellis.
* Receive tokens in the client credentials flow.

### Things to Remember in This Tutorial

You should remember the following:

* Use the following to start the server:  

    ```curl
    node server.js
    ```

* Use **Ctrl**+**C** to stop the server.
* Install the following before starting the tutorial:
    * Node
    * NPM

## Prerequisites

You should have a good understanding of the following before starting this tutorial:

* A basic understanding of Node.js.
* A basic understanding of Git.
* A basic understanding of JavaScript.
* Node.js and npm installed on your computer.

## Creating Your Own Project

1. Create a new folder.  
   On the command line in the directory, type the following and the name of the directory where you would like to create your project:

   ```curl
   mkdir {yourProject}
   ```

1. Change to that directory.  
   On the command line, type the following and the name of the directory that you just created:  

   ```curl
   cd {yourProject}
   ```

1. Clone the repository.  
   On the command line, type the following to clone the repository:

   ```curl
   git clone git@github.com:Fortellis/client-credentials-flow-app.git
   ```

   **Note:** You can find the client-credentials-flow-app on GitHub for more information.

## Installing Dependencies and Build the React App

**Note:** Remember the following:

* You may have to wait a moment for each install to complete.
* You can move on to the next steps if you are confident you can keep your place.
* You must complete the final build step to run the application locally.

1. Change directories to the `client-credentials-flow-app` folder.  
   On the command line, type the following to access the cloned repository:

   ```curl
   cd client-credentials-flow-app
   ```

1. Install the express dependencies.  
   On the command line, type the following:  

   ```curl
   npm install
   ```

1. Changes directories to the client folder.  
   On the command line, type the following:  

   ```curl
   cd client
   ```

1. Install the client directory dependencies.  
   On the command line, type the following:  

   ```curl
   npm install
   ```

1. Build the production react app.  
   On the command line, type the following to build the react app:

   ```curl
   npm run build
   ```

1. Go back to the root directory.  
   On the command line, type the following:  

   ```curl
   cd ..
   ```

## Registering the Application on Fortellis

Register the website using `https://example.com` and leave the callback URL blank.

1. Follow the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps) to register your app.
1. Follow the instructions in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) to get your credentials.
1. In the `server.js` file in the root folder, replace the placeholder `CLIENT_ID` with your **API Key** that you get from the Fortellis UI.
1. In the `server.js` file in the root folder, replace the placeholder `CLIENT_SECRET` with your **API Secret** that you get from the Fortellis UI.
1. Start the `server.js` server.  
   On the command line, type the following to start the server:  

   ```curl
   node server.js
   ```

> You can now go to `localhost:5000`,
> and click the **Login to Fortellis** button to get an access token.
> You'll see the access token in both the browser console and on the command line.

## Overview of How It Works

Make the following observations about the client credentials authorization code flow:

* You use your app credentials on Fortellis to get tokens for your users.  
   **Note:** Your users take actions and call APIs on your behalf,
   and any actions they take are tied to you.
* Your user never sees the token on the server.
* Your user does not log in through Fortellis.
* You must store the token on the server and include it with requests to APIs.

You should now understand more about how the client credentials flow works. Remember the following:

* You must protect your credentials on the server side.
* Your users should never see your credentials.
    * We logged the credentials to the console to show them in the browser for this tutorial.
* Use this authorization flow if you have a server or some other way of protecting your credentials.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Clone a Node project from a repository.
* Install the dependencies for the project.
* Register an application on Fortellis.
* Receive tokens in the client credentials flow.

You can now start creating your project using the client credentials flow to get a token.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
