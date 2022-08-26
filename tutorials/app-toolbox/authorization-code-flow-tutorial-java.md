---
title: Java Authorization Code Tutorial
author: Nathaniel Sandberg
last updated: 4/26/2022
---

You can quickly get up and running with the following app that uses the authorization code flow.
You would want to use an authorization code when you have a backend server,
but you want your users to sign in through Fortellis.

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' authorization code flow is not secure in the context of JavaScript apps and single pages apps.

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

You should have a good understanding of the following before starting this tutorial:

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
    git clone git@github.com:Fortellis/authorization-code-flow-in-Java.git
    ```

1. Copy the `ROOT` from the new folder.
1. Paste the new root into the `webapps` folder.  

## Logging In

1. When the app opens, click **Get Your Authorization Code**.
1. Type in your credentials.
    > Fortellis may not prompt you for your credentials
    > if you are already logged in.
    > If so, you get your authorization code in the URL,
    > and you can click the **Get Token** button.
1. Click **Sign In**.  
1. Click **Get Token** to get your token through the authorization flow.  
    **Note:** You must use the authorization code within one minute.  

You can now see your token in the URL and you are logged in to the app.

## Overview of How It Works

Make the following observations about the implicit authorization flow in the app code:

* When you click **Get Your Authorization Code**,
    you get your authorization code in the URL.  
* When you click **Get Token**,
    the app uses the authorization code in the URL to get a new token.  
* Your use your [base64 encoded](https://www.base64encode.org) credentials on behalf of your users to get a token.  

## Next Steps

In this tutorial, you should have learned how to do the following:

* Clone a Java project from a repository.
* Download Resin to run a Java web server.
* Register the app on Fortellis.
* Receive tokens with the authorization code flow.

You can now start creating your project using the authorization code flow to get a token.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
