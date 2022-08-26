---
title: API Connection Management 
author: Nathaniel Sandberg
last updated: 10/18/2021
---

You can quickly get an app running and still manage your API connections.

In the UI, Fortellis offers a UI tool in the Subscriptions where you can see the following:

* The `Subscription-Id`
* The connection status

With this feature, you can get the benefit of a review process for your API without the added work of the Admin API.

With this option, you can do the following:

* View pending connections in the Fortellis user interface.
* Approve them when you are ready.
* Identify the app connections that connect to your API with either of the following:
    * The `Subscription-Id` that
    Fortellis sends in the header of each request.
    * A custom header that you create within the Fortellis UI.

You can use Connection Management to do the following:

* Get notified when an organization chooses you to be their API Developer.
* Get notified when an organization removes you as their API provider.
* Activate the connection to an API.
* Get and save the current user's activation context to use within an API.
* Deactivate the connection to an API.  
    **Note:** Once you deactivate an API, you cannot reactivate it in the current app.

You can use Connection Management to gain functionality similar to the Admin API.

## Managing Subscriptions

1. Sign in to the [Developer Network Platform]($[devNetworkUrl]).
1. Enable the Organization in which the API is registered:  
    1. Hover over your name in the upper-right corner and click **Change**.
    1. Click the Organization where the API is registered, then click **OK**.
1. In the API section, click **View All** and find the API whose subscriptions you want to manage.
1. Click the API listing to see the API's Details Page.  
    **Note:** If the UI does not list any subscriptions or options, then no one has yet subscribed to the service.
1. Click **Manage Subscriptions**.  
    **Note:** From this page, you can view the following:  
    * The **Organization** that the API activation belongs to  
    * The **App** through which the user has activated the API  
    * The **Connection ID** for the request  
    * The **Subscription ID** for the user  
    * The **Subscribed Date**  
    * The **Status** of the subscription  
    ![Connections]($[docsUrl]/static/images/connections.PNG)
1. Click the subscription you want to update.
1. To activate the pending subscription, click **Activate**, and then click **Continue** to finish activating the subscription.  

Fortellis will send the **Key** and the **Value** in the header with any call made to your API so you can identify the subscriber.
