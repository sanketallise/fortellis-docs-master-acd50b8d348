---
title: Java Client Credentials Authorization Tutorial
author: Nathaniel Sandberg
last updated: 4/19/2022
---

You can use this tutorial to start creating an app that uses the client credentials flow.
See below to download the example and get started creating your app with the client credentials flow.

**Note:** If you use the client credentials in a production app,
you cannot expose your API key and secret to users.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Clone a Java project from a repository.
* Download Resin to run a Java web server.
* Register an application on Fortellis.
* Receive tokens in the client credentials flow.

### Things to Remember in This Tutorial

You should remember the following:

* Click `resin.exe` in the Resin folder to start the app.
* Click **Stop** to stop the server.
* Set up your environment variables.

## Prerequisites

You should have a good understanding of the following before starting this tutorial:

* A basic understanding of Java.
* A basic understanding of Git.
* [JDK 8 or later](https://www.oracle.com/java/technologies/downloads) installed on your computer.

## Setting the Path for Java

Use the following tutorial to set the path variable if you have not done that already: <https://www.geeksforgeeks.org/how-to-set-java-path-in-windows-and-linux>.

## Download Resin

1. Download [Resin](https://caucho.com/download/resin-4.0.62.zip).  
1. Unzip the folder in the directory of your choosing.  
    **Note:** Unzip the folder to a directory close to your computer's root directory.
        For example, you can use `C:\Users\{yourUserName}\Documents\`.
1. In the new resin folder, click `resin.exe` to start resin.  
1. Go to [http://localhost:8080](http://localhost:8080) to see that it is installed correctly.  

## Copying the Project

1. Click **Quit** to stop Resin.  
1. In the `resin-4.0.62/webapps`, delete the `Root` folder.
1. Install the project root.  
    On the command line in the `resin-4.0.62/webapps` folder, type the following:  

    ```bash
    git clone git@github.com:Fortellis/java-client-credentials-flow-example.git
    ```

1. Copy the `ROOT` from the new folder.
1. Paste the new root into the `webapps` folder.  

## Registering the App on Fortellis

Register the website using `https://example.com` and leave the callback URL blank.

1. Follow the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps) to register your app.
1. Follow the instructions in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) to get your **API Key** and **API Secret**.
1. Base64 encode your **API Key** and **API Secret**.  
    At [Base64](https://www.base64encode.org), encode your `{yourAPIKey:yourAPISecret}`.  
1. Replace `base64Encoded{yourAPIKey:yourAPISecret}` in the `WEB-INF\classes\Main.java` with your token.
1. To start the server, click `resin.exe` in your `resin-4.0.62` folder.  

> You can now go to `localhost:8080`,
> and click the **Get Token** button to get an access token.
> You'll see the access token in both the browser and on the command line.

You now have a Fortellis token from your Java app.  

## Overview of How It Works

Make the following observations about the client credentials authorization code flow:

* You use your app credentials on Fortellis to get tokens for your users.  
   **Note:** Your users take actions and call APIs on your behalf,
   and any actions they take are tied to you.
* Your user should never see the token on the server.
* Your user does not log in through Fortellis.
* You must store the token on the server and include it with requests to APIs.

You should now understand more about how the client credentials flow works.
Remember the following:

* You must protect your credentials on the server side.
* Your users should never see your credentials.
* Use this authentication flow if you have a server or some other way of protecting your credentials.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Clone a Java project from a repository.
* Download Resin to run a Java web server.
* Register an application on Fortellis.
* Receive tokens in the client credentials flow.

You can now start creating your project using the client credentials flow to get a token.
Remember that you must associate the user with the `Subscription-Id` in this flow and send the `Subscription-Id` with the `token` to get a response from the API.
