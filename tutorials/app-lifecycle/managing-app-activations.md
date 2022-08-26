---
title: Managing App Activations
author: Kelly Rich
last updated: 6/23/2022
---

Get information and manage the apps you have in development and listed in Marketplace using the **My Apps** page.

![The My Apps page]($[docsUrl]/static/images/my-apps-page.png)

## The My Apps Page

To navigate to an app's **My Apps** page:

1. Sign in to [Marketplace]($[marketplaceUrl]) and activate the Organization containing the app you want to manage.
1. Hover over your name in the top right corner, then hover over **Marketplace Account** in the resulting menu.
1. Click **Apps** in the Marketplace Account context menu, the **My Apps** page displays.

From the **In Marketplace** section of the main **My Apps** page, click the app you want to manage. The Details page for the app displays, which has the following menu items on the left nav:

* **Version and Insights**  
    * Insights
    * Manage Listing Versions
    * App Activation
    * Share
* **Activity**  
    * Leads
    * Reviews
    * Activations
    * Utilization

## The Insights Page

The **Insights** page displays the following app details at the top of the page:

* Active Activations
* Leads
* Reviews

Click the **Activity** menu items in the left nav to access details on these items.

The table at the bottom of the page details the status of the apps that you have listed on Marketplace.

![App Insights and Status of Activations]($[docsUrl]/static/images/my-app-insights-page.png)

## Managing the Listing Version of your App

To manage the version of an app you currently have listed in Marketplace:

1. From the app's Details page in **My Apps**, click **Manage Listing Versions** on the left nav.
1. If you have an approved app, the following options appear:
    * **Publish To Marketplace** – If the app is Approved, this begins the flow to list the app in Marketplace.
    * **Preview** – Displays a pop-up showing a preview of the app listing in Marketplace.
    * **View In Marketplace** – Opens a new tab that displays the live storefront-view of the app listing in Marketplace.  
    – Or –  
    If you have already published your app, the following options are displayed:  
    * **View** – Displays the App Submission Form for the app.
    * **Edit** – Opens the App Submission Form in edit mode.  
    **Note:** If you edit an app's listing version, a new version of the app is automatically created, triggering a new review.
    * **Preview** – To help when editing a listing, this displays a pop-up showing a preview of your current edits.
    * **Withdraw** – Removes the app listing from Marketplace.
    * **View In Marketplace** – Opens a new tab that displays the live storefront-view of the app listing in Marketplace.

### Editing Existing Apps

To edit the configuration of existing app, click **Edit** on the left nav to open the app in edit mode. For details on the fields you can configure on this page, see [The App Submission Form](/docs/tutorials/app-lifecycle/app-submission-form).

### Withdrawing Apps from Marketplace

Before you withdraw your app from Marketplace, remember the following:

* App users will not be able to see your app when they are browsing and searching for apps
* You will not be able to accept new activations for the app
* You should continue to support existing activations that remain active
* You must migrate app users to a new app or terminate the activations

## The App Activations Page

Use the **App Activation** menu item to search for and view current activation requests. For more, see [Manual App Activation](/docs/tutorials/app-lifecycle/manual-app-activation).

When your app is published on Marketplace, Fortellis users who have access to your app listing can make a request to activate your app. Depending on how the app is configured, the activation of the app can be either automatic or manual.

If you receive a request from an App Buyer to activate an app, you can contact the buyer to clarify any details before approving their request.

### Approving Activation Requests

Remember, as the App Developer, when you approve an app-activation request, the request gets passed to each of the APIs that the app uses.

If the app uses APIs that are configured for automatic activation, Fortellis automatically completes the connections to the API services when it receives an app activation request. However, API Developers can also configure their APIs to be activated only after a manual review.

Because the app can be active very soon after you approve the app activation request, approve the requests as soon as you complete the provisioning for the app.

* If the APIs automatically accept activation requests, then the App Buyer can immediately begin managing their connections as soon as the app-activation request is approved.

* If an API Developer manually approves activations to their API, the App Buyer must wait until their API-activation request is approved by the API Developer before they can begin managing connections to the app.

If you need assistance with approving or denying a subscription, contact a Fortellis Administrator at [support@fortellis.io](mailto:support@fortellis.io).

> Once an app is activated for an App Buyer, you must map their active `Subscription-Id` to the App Buyer's account and send that Subscription ID with all API requests made through your app on behalf of that App Buyer's account.

## Sharing Your App

The **Share** menu item is active if:

* Your app is private
* Your app is published to Marketplace

To share your app with another Fortellis user:

1. Enter the email address of the person who you want to share the app with.  
    If the email address you enter does not correspond to an active Fortellis user, then the user must create a Fortellis account before they can activate your app.
1. Check or uncheck the box at the bottom of the page to send an email, and then click **Save**.

For more, see [Sharing Private Apps])(/docs/tutorials/app-lifecycle/sharing-private-apps).

![Sharing Private Apps]($[docsUrl]/static/images/my-apps-sharing-private-apps.png)

## Viewing Leads

App Developers can view leads and other potential app user requests sent to them from Marketplace.

1. Navigate to the app's Details page in **My Apps**.
1. In the left nav, click **Leads** under the **Activity** heading.  
    The email sent through Marketplace from interested buyers are listed on the resulting page.

## Viewing Reviews

To see a list of user reviews for your app:

1. From the app's Details page in **My Apps**, click **Reviews** on the left nav.  
    The resulting page lists all the reviews and feedback users generated for your app.

## The Activations Page

The **Activations** page lists app activation requests by location. A summary for each location is listed and you can click into each listing to see details for the location.

![My Apps Activations Page]($[docsUrl]/static/images/my-apps-activations-page.png)

### The Activation Details Page

From the **Activations** page, click an activation listing to view its **Activation Details** page. This page contains two sub-pages: **Overview** and **API Configuration**.

The **Overview** page displays the following:

* The **View API Configuration** button reveals the status details for the API activations.
* The **Mark As Complete** button lets you mark the app provisioning as *complete*, click **OK** to con.

The **API Configuration** page shows which APIs are configured for the associated location and the status of the activation.

### Manually Configuring and Provisioning APIs

On the App Submission Form, can you indicate how you will provision API connections.  For details, see [Configuring Provisioning Options](/docs/tutorials/app-lifecycle/app-submission-form/#configuring-provisioning-options).

> **Note:** If you have a process for configuring your app before it can be activated, then you must confirm here that the APIs have been configured and the app is provisioned for each location where the app is to be activated.

If the app is manually configured and provisioned, the **Activation Details** page displays the **Configure API Provider** and **App Provisioning** panes.

Use these panes to indicate the configuration and provisioning is complete for a specific location. Once you indicate both options are complete, the **Activate Subscription** button becomes enabled and you can click it to activate the subscription.

![My Apps Activation Details Page]($[docsUrl]/static/images/my-apps-activation-details-page_provisioning-options.png)

## Viewing Utilization

Click **Utilization** in the left nav to see the following app utilization information:

* The total activations
* The total API calls
* The total API cost
* The total API calls by API
* The total API calls by app activation

![API Calls by Activation]($[docsUrl]/static/images/my-apps-utilization-page.png)

## Getting an App Subscription Report

If your app is a subscriptions-based app, you can get a CSV file that details the app's subscriptions.

1. Sign in to [Developer Network]($[devNetworkUrl]).
1. In the upper-right corner, hover over your name and click **Developer Account**.
1. Search for and open the app you want to review.  
    **Note:** If you can't see the app you are looking for, you may need to do one of the following:
    * Click **View All Apps**.
    * [Change Organizations](/docs/general/overview/organizations/#changing-organizations).
1. In the app's Details box, click **Download Subscription Report**.

The Subscription Report contains the following information:

* `App Name`
* `App ID`
* `Organization Name`
* `Organization ID`
* `Subscription ID`
* `Status`
* `Activation Date`
* `Deactivation Date`

## Deactivating App Activations

To deactivate an activation due to non-payment or for any other reason, see [Canceling App Activations](/docs/tutorials/app-lifecycle/canceling-app-activations).

A Fortellis Admin may also deactivate all your activations if the dealer goes out of business, cannot pay for the API and App, or for any other reason.
