---
title: Managing App Configurations
author: Nathaniel Sandberg
last updated: 8/2/2022
---

You can do the following with your apps:  

* Update the app configuration.
* Delete apps you have registered.

## Updating App Configurations

You can update details at any time throughout an app's lifecycle. You may update apps to do the following:

* Offer your app to users to get feedback during development.
* Capitalize on your investment in the app while you continue to improve it.
* Discover problems and use cases through user beta testing.
* Create new versions of the app.
* Use a new API that you want to incorporate in your app.  

Do the following to update the app:  

1. [Sign in](/docs/general/overview/getting-started/#signing-in) to Developer Network.  
1. Select the organization that the app is registered with.  
    1. Hover over your name in the upper-right corner.  
    1. Click **Change**.
    1. Select the organization.  
    1. Click **OK** to select the organization.  
1. Click **View All** to see a list of all your apps.  
1. Click the app.

From this page, you have the following options:  

* **Metrics**
* **Errors List**
* **Download Subscription Report**
* **Edit App**

You can also see your credentials:  

* **API Key**
* **API Secret**  

See [Renewing App Credentials](/docs/tutorials/solution-integration/renewing-credentials) to find out how to generate a new **API Key** and **API Secret**.  

You can also get your **Test Subscription ID**.
With it, you can access any APIs that the API Developer publishes to the test environment.  

You can also see the following about the APIs that your app uses:  

* **API:** You can see the name of the API that the app uses.  
* **Publisher:** You can see the name of the organization that created the API.  
* **Pricing:** You can see one of the following pricing models for the API:  
    * **Free**
    * **Subscription**
    * **Transaction**
* **Terms:** You can see if the API has terms and if you agreed to them.
* **Configure:** You can see the endpoint where the app receive events for asynchronous APIs.
* **Accepted On:** You can see the date when you accepted the terms.  

You can also select **Create Listing** to sell the app in Marketplace.
See [Listing Apps in Marketplace](/docs/tutorials/app-lifecycle/listing-apps) for more information.

## Metrics

You can view the API metrics for your app to see the error rates for your APIs.
See [Viewing Your App Metrics](/docs/tutorials/app-lifecycle/app-metrics) for more information.

## Checking Errors

1. Click **Errors List** to see a list of errors.  
1. Select the following options:  
    * **API**
    * **Environment**
        * **Production**
        * **UAT**
        * **Dev**
    * **Timespan**
        * **15 Minutes**  
        * **Custom Timespan**  
        **Note:** You can use the use the modal dropdown to select the **Custom Timespan**.
    * **Status Code**
        * **4XX**
        * **5XX**
        * Enter the code of your choice.
    * **Method**
        * **Put**
        * **Get**
        * **Post**
        * **Patch**
        * **Delete**
    * **Source**
        * **Fortellis**
        * **API**
    * **Refresh**
    * **Request ID:** You can only use this filed to check on one request, and it may not have the error code you select.

1. Click **Refresh** to get your most recent updates from your selection, the **Custom Timespan** or **15 Minutes**.

## Download Subscription Report

You can download a CSV file with your subscription report.
In the file, you can see the following:  

* App Name
* App ID
* Organization
* Organization ID
* Subscription ID
* Status
* Activation Date
* Deactivation Date

With the subscription report, you can get the information on all your Subscribers and the organizations they belong to.

## Editing App Details

Click **Edit** in the **App Details** to edit the app.

You can update the following in the app:  

* **App Name**  
    **Note:** Current subscribers do not see updates you make to the name of your app.
* **App Description**  
* **Website**
* **Callback URL**
* **API Integrations**  
    **Note:** You can add APIs at any time.
    You cannot remove APIs
    if you have active subscriptions.

### Adding APIs

Do the following to add a new API:

1. Select the **API Type**.
1. Select the **Category** if you like.
1. Select the **Publisher** if you like.
1. Enter the API name in the **Search APIs** box if you know it.
1. Click API to add to your app.

### Selecting Pricing Plans and Accepting Terms

Once you have selected your APIs, configure them at the bottom of the page.

1. Click the **Select Plan** next to the API to choose your pricing plan.  
1. Select the pricing plan.  
1. Click **Save** to save the plan.  
1. Click the **Terms** column next to the API.  
1. Click **Yes**.
1. Enter your information:  
    * Your **Name** should be prepopulated.
    * Enter your **Title**.
    * Enter your **Company**.
1. Click **Accept Terms** to agree to the terms and conditions.

### Configuring Async APIs

For Async APIs, you must also add a webhook.
You can add as many asynchronous APIs as you want.
You must enter a webhook for each of the asynchronous APIs you choose.
You only receive one channel per an asynchronous API.

1. Click **Webhook**.
1. Enter the URL for your webhook where you receive events.
1. Click **Save**.
1. To save your updated API, click **Save** at the bottom of the page.

### Removing APIs

To remove an API, click the trash can icon next to the API.
You can only remove APIs from an app that has no subscriptions.

### Best Practices for Updating APIs

Updating certain app configuration settings can disrupt the app's functionality for any active app users. Whenever you plan to update an app that is active, be sure to notify the app users of your update plans.

Depending on the API Developer's support for an existing version of an API, you might not need to update your app when a new version of an API is released. However, you must update your app if you need to use a new `basePath` for an API.

Normally, API Developers change the `basePath`of their API only if updates cause breaking changes to the existing version. If the `basePath` changes, roll out your app update only after you have addressed any breaking changes the new API might introduce.

Be sure to consider the following when you update the version of an API in an app that's already in use:

> * New versions of an API may introduce potentially breaking changes to existing apps.  
> * You may have to update the app to consume any new fields introduced with an API update.  
> * If you introduce a new version of an app, you must tell your active app users to use the new app listing and to add any new API versions to their existing app configurations.

## Deleting Apps

If you find the need to delete an app, note that you can only delete it when it has no active users.

To delete an app:

1. Sign in to [Developer Network]($[devNetworkUrl]) and enable the Organization containing the app:  
    1. Hover over your name.
    1. Click **Change**.
    1. Select the organization the app belongs to.
    1. Click **OK**.
1. Find the app listing.
1. Click the app's menu button on the right side of the listing.
1. Click **Delete** from the menu.
1. Click **OK** to confirm the action.  

If you try to delete an app that's **In Review**, Fortellis displays a message detailing the app's status:

![The App is Being Reviewed]($[docsUrl]/static/images/cantDeleteAppInUse.PNG)
