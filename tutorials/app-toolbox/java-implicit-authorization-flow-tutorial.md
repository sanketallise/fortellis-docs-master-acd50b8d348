---
title: Java Implicit Authorization Tutorial
author: Nathaniel Sandberg
last updated: 8/2/2022
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

* Clone a Java project from a repository.
* Download Resin to run a Java web server.
* Register the app on Fortellis.
* Receive tokens with the implicit authorization flow.

### Things to Remember in This Tutorial

You should remember the following:

* Click `resin.exe` in the `resin-4.0.62` folder to start the app.
* Click **Stop** to stop the server.
* Click **Start** to restart the server
* Click **Quit** to exit Resin.

Your users must have Fortellis credentials to use this authorization flow.

## Prerequisites

You should have the following before starting this tutorial:

* A basic understanding of Java.
* A basic understanding of Git.

Have [JDK 8 or later](https://www.oracle.com/java/technologies/downloads) installed on your computer before starting this tutorial.

## Setting the Path for Java

Use the following tutorial to set the path variable if you have not done that already: <https://www.geeksforgeeks.org/how-to-set-java-path-in-windows-and-linux>.

## Download Resin

1. Download [Resin](https://caucho.com/download/resin-4.0.62.zip).  
1. Unzip the folder in the directory of your choosing.  
    **Note:** Unzip the folder to a directory close to your computer's root directory.
        For example, you can use `C:\Users\{yourUserName}\Documents\` if you are using a Windows computer.
1. In the new resin folder, click `resin.exe` to start resin.  
1. Go to [http://localhost:8080](http://localhost:8080) to see that it is installed correctly.

## Copying the Project

1. Click **Quit** to stop Resin.  
1. In the `resin-4.0.62/webapps`, delete the `Root` folder.
1. Install the project root.  
    On the command line in the `resin-4.0.62/webapps` folder, type the following:  

    ```bash
    git clone git@github.com:Fortellis/implicit-authorization-code-flow-in-Java.git
    ```

1. Copy the `ROOT` from the new folder.
1. Paste the new root into the `webapps` folder.  

## Registering the App on Fortellis

1. Follow the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps) to register your app.
1. Follow the instructions in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) to get your **API Key** and **API Secret**.
1. Replace `{yourAPIKey}` in the `ROOT/index.jsp` with your **API Key**.
1. To start the server, click `resin.exe` in your `resin-4.0.62` folder.  

> You can now go to `localhost:8080`,
> and click the **Get Token** button to get an access token.
> You see the access token in the browser URL.

You now have a Fortellis token for your Java app.  

## Logging In

1. When the app opens, click **Get Token**.
1. Type in your credentials.
    > Fortellis may not prompt you for your credentials
    > if you are already logged in.
1. Click **Sign In**.  
    **Note:** Fortellis resolves all URLs to a secure connection,
    so you receive an error stating that the site cannot create a secure connection.
1. Delete the `s` in `https` in the URL and press **Enter** to see the original site again.

You can now see your token in the URL and you are logged in to the app.

## Overview of How It Works

Make the following observations about the implicit authorization flow in the app code:

* When you sign in through Fortellis,
    you receive a token in the URL.
* You assign the `client_Id` and the `redirect_uri` before the html `body` tag.
* When users click **Get Token**, they use the implicit workflow to get a token in the URL.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Clone a Java project from a repository.
* Download Resin to run a Java web server.
* Register the app on Fortellis.
* Receive tokens with the implicit authorization flow.

You can now start creating your project using the implicit authorization flow to get a token.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
