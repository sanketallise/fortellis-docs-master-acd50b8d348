---
title: Create Your First App
author: Nathaniel Sandberg
last updated: 2/24/2022
---

You can use this tutorial to get started creating your first app.
In this tutorial, we create an app that uses Amazon Web Services and npm to serve car photos for a dealership.
You can learn the basic skills to onboard to the Fortellis platform with this tutorial.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Download the sample app from GitHub.
* Start an app using npm.
* Create an S3 bucket.
* Update the bucket to host a static site.
* Update the app in your Developer Account.
* Build the app.
* Update the S3 bucket.

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
* Sign up for the tree tier of [Amazon Web Services](https://aws.amazon.com).  
    **Note:** Make sure you understand the pricing for the free tier of support before you start.
* Your `Subscription-Id` must be active in order to make calls to the API.

## Acquiring OAuth Tokens with the Implicit Flow

In this tutorial, we will use the implicit authorization flow.
You only want to use the implicit flow in the following apps:

* Single-page apps
* Native iOS
* Native Android apps

In all these cases, the user can do the following:

* Access the source code.  
* See any IDs and secrets included in the code.  

You also need to get the user'ss `Subscription-Id` in the implicit flow to send to the APIs on Fortellis.
In this example, we will create and deploy a single-page react app.

For more information on authorization flows, see [Authorization on Fortellis](/docs/tutorials/solution-integration/auth).

## Registering an App on Fortellis

You must register your app to call a Fortellis API. Follow the instructions in [Registering Apps](/docs/tutorials/app-lifecycle/registering-apps) to register your app on Fortellis using the Merchandisable Vehicles V1 API.

## Developing the App

### Sample App

To get you started, we've created a sample app for you to use as a template.
You can see the repository [here](https://github.com/Fortellis/ImplictFlow-ExampleApp).

This tutorial creates a simple one page React app that does the following:

* Calls the Merchandisable Vehicles API
* Lists available cars at a dealership

### Getting the Sample App from the Repository

1. Clone the repository from GitHub.  
    On the command line in the directory you choose, type **git clone <https://github.com/Fortellis/ImplicitFlow-ExampleApp>**.
1. Change directories to the folder you just cloned.  
    Type **cd ImplicitFlow-ExampleApp**.
1. Install npm within that folder.  
    Type **npm install**.
1. Replace the placeholder client ID with your client ID from Fortellis.  
    In /ImplicitFlow-ExampleApp/src/App.js, change the placeholder **placeholder-client-id** to your API key that you got from Fortellis.
1. Replace the redirect URI with your app's URI.  
    Change the placeholder redirect URI `https://placeholder-redirect-uri.com` to your `redirect_uri`.  
    **Note:** For the first part of this tutorial,
    we use `localhost:3000`,
    but you cannot enter that in the Fortellis UI for the **Website**
    when you are registering the app.

### Starting the Application

1. Start the application with React's start script.  
    In the project directory, type **npm start**.  
    **Note:** The terminal will say **localhost running on 3000** once you have everything working. If the app doesn't automatically open, go to `http://localhost:3000`.

    ```bash
    Compiled successfully!

    You can now view ImplictFlow-ExampleApp in the browser.

    Local:            http://localhost:3000/
    On Your Network:  http://192.168.56.1:3000/

    Note that the development build is not optimized.
    To create a production build, use npm run build.
    ```

    **Note:** If you enter the correct API key and the same redirect URL that you entered when you were registering your app, the app redirects you to log in to the Fortellis platform.
1. Click **Login with Fortellis**.  
1. Enter your Fortellis credentials to sign in.  
    **Note:** Fortellis routes you back to your localhost after you have logged in.
1. Click **Get Vehicles**.  
    **Note:** Your app should look like this.

![vehicle finder app success]($[docsUrl]/static/images/working_app.jpg)

Congratulations! You've just created your first Fortellis app.

## Hosting your App

You've successfully run your app locally. Now you must host it somewhere so that others may reach it.

### Creating an S3 Bucket

Depending on your usage, you may incur additional charges
if you're hosting an app through Amazon S3.
Make sure you understand and accept Amazon's charging model before using it.
For this tutorial, we are deploying the react app on Amazon S3 and configuring those buckets to host the website.

1. If you don't already have an account, follow the steps to [Create a New AWS Account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/).  
1. Create an [S3 Bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html).  
    ![create bucket 1]($[docsUrl]/static/images/create_bucket1.jpg)  
    Remember the following:  
    * Choose the option to **Do not grant Amazon S3 Log Delivery group write access to this bucket**.  
    * Choose **Grant public read access to this object(s)** to make the objects publicly readable.  
    ![create bucket 2]($[docsUrl]/static/images/create_bucket2.jpg)  

### Updating the Bucket to Host a Static Site

1. To enable users to see the contents of your bucket, go to the **Properties** tab and click **Static Web Hosting**.
1. Select **Use this bucket to host a website**.  
    ![enable static web hosting]($[docsUrl]/static/images/enable_hosting.jpg)
1. Enter the location of the index document.  
    **Note:** Amazon suggests the correct value of index.html, and it will be in the root directory of the site.
1. Type it in and click **Save**.
1. View the URL that S3 will host the app on.  
    **Note:** Once you save the configuration, you can start to host your app on this bucket.

### Updating the App in Your Developer Account

1. Navigate to your [Developer Account]($[devNetworkUrl]/developer-account).
1. Select your app.
1. Click **Edit**.
1. Replace the localhost URL with the URL from S3 for the **Website** and the **Callback URL**.  
 **Note:** Some apps may use different URLs for these two, but we'll use the same one for this app.
1. Click **Next** when you are finished.
1. Click **Update**.

### Building the App

1. In App.js, update the **redirect_uri** from the localhost URL to the S3 URL.
1. Save the file.
1. Create the production build of the react app.  
    To get the app running, type **npm run build**.  
    **Note:** The npm command `create-react-app` includes this script that builds the app for you. It creates a directory called build. The files in this directory represent your app. You can upload them to your S3 bucket as is.
1. Go back to your bucket.

### Uploading the Updated Files

1. Click **Upload**.  
1. Click **Add Files** button to complete the process manually.  
1. Select the file from your computer.  
1. Click **Permissions**, and click **Grant public-read access**.  
1. To confirm, click **I understand the risk of granting public-read access to the specified objects.**  
1. Click **Upload**.  
1. Click **Close**.  

The app takes the token from the URL and hides it from the user.
The user can click the **Get Vehicles** button.
The app will call the Fortellis API with the token it just acquired and display the vehicles
that it gets back to the user.

![working app]($[docsUrl]/static/images/working_app.jpg)

## Next Steps

In this tutorial, you should have learned how to do the following:

* Download the sample app from GitHub.
* Start an app using npm.
* Create an S3 bucket.
* Update the bucket to host a static site.
* Update the app in your Developer Account.
* Build the app.
* Update the S3 bucket.

You can now see a sample app.
