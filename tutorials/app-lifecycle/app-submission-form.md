---
title: The App Submission Form
author: Kelly Rich
last updated: 6/2/2022
---

You need the following information to complete the *App Submission Form*:

* Details for how you plan to provision your app
* App support information
* Marketing material for your app, which can include:  
    * A list of your app highlights
    * Details of your app's functionality
    * Videos and images that promote your app
    * A list of new features, if you're updating your app  
* The user-authorization method you want to use with your app:  
    * Auto
    * Manual

Also, depending on how your app is provisioned, you might need to include these materials:

* Pricing plans for your app
* Details for how you will bill for app usage
* Terms and Conditions for the use of your app, submitted as a PDF file

Be sure to populate every field that has an asterisk (\*) next to it, they are required fields.

As you complete the *App Submission Form*, use the **Save** button often. After saving, take a break and come back at any time to ensure everything is right before submitting the form.

> **Note:** If your *App Submission Form* includes **Terms and Conditions** and **Pricing Plan** entries on the form, see [App Terms of Use and Pricing Plans](/docs/tutorials/app-lifecycle/app-terms-and-pricing-plans).

## The App Submission Form

Once you've prepared all the material needed to list your app, locate your app on the Developer Account page and click **Sell In Marketplace** to begin the listing flow. Here is the process:

1. Sign in to Developer Network and enable the Organization containing your app.
1. Open the **Developer Account** page by hovering over your name and choosing from the context menu.
1. Find the app you want to list in the **Apps** section and double-click the listing to open its App Details page.
1. Click **Sell In Marketplace** to open the **App Submission Form** and begin the app listing flow.

The App Submission Form contains a left nav and a set of tabs that you use to add information to your app listing. The following sections describe the information you can enter into each field to describe your app.

While all fields are not required, keep in mind that the more descriptions you add to your app, the easier it is for App Buyers to choose your app.

## Completing the App Overview

The App Submission form contains three tabs that define what is displayed on the **Overview** tab of your app listing:

* Overview
* Videos
* Gallery

Provide information in each of these three tabs advertise your app on the **Overview** tab of your listing. The **App Details** page displays the **Overview** tab by default, which makes this material the first thing the App Buyer sees when they view your app.

Complete all the forms in the **Overview** tab to detail the features and functionality of your app. The fields marked with an asterisk (*) are required, and you must populate them before your app can be approved.

When filling out the Overview form, note the following:

* In the * **Copyright** field, indicate the name of the app's copyright holder.  
* In the **Privacy Policy URL** field, enter the URL to your privacy policy. The URL must respond with `200 OK`.
* For the **Solution Logo**, click **Click to upload** and choose the file to upload for your logo.  
    The image loaded here displays in your top-level app listing in Marketplace.

![App Submission Form]($[docsUrl]/static/images/app-submission-form-2.png)

### Adding Videos and Images to the App's Overview Page

Populate the **Videos** and **Gallery** tabs for the Overview as follows:

On the **Videos** tab, click **Add Video** and enter the following information:

* **Title** – Descriptive title of your video
* **URL** – Link to the video, hosted on YouTube or Vimeo
* **Description** – Video's key points

Add additional videos by clicking **Add Video** and filling out the information for each video you want to include in your app listing.

> **Note:** Videos must be hosted on either YouTube or Vimeo.

On the **Gallery** tab:

1. Click the upload button and elect the file that you want to upload from your computer.
1. Add more images by clicking **Add Gallery Image**.

The images you add to the gallery are displayed to App Buyers on your App Details page.

## Adding App Highlights and Features

The left nav of the App Submission Form contains the **Highlights** and **Features** menu items. Information entered into this section of the form display on the **Highlights** tab of your app listing.

As best as you can, detail the highlights and features of your app. But note that Marketplace does not require you to complete these sections.

## Adding Support Details

Click **Support** on the left nav to add **Tutorials and Documentation** and **Questions and Support** information.

> **Important:** You must enter a support email address on the **Questions and Support** tab.

## Configuring API Details

Use the **Registration Details > API Details** menu section to configure the plans you will use for the APIs in your app.

Open the **API Details** page and for each API used in your app, select the pricing plan that you want to use with the API.

Each API has a list of pricing plans offered for the API. To select the one you want:

1. Click the **Select** link associated with the API **Pricing Plan** you want to use.  
    This opens the **Choose Your Pricing Plan** window, which lists the plans offered by the selected API provider.
1. Click the radio button associated with the pricing plan you want to use with the app, then click **Select**..  
    This *Enables* the plan for your app.
1. Click the **Enable** link to opens the terms that you must agree to in order to use the API.
1. Follow the instructions on the consent form to enable the selected pricing plan for your app.

> **Note:** You must indicate a pricing plan to use for each API used by your app.

## Setting the Provisioning Options

Set notification and provisioning options here.

### Configuring Subscription Notifications

If you use a subscription-model for your app, you need to configure how to receive notifications for your app (such as subscription request notifications).

1. Under **Provisioning**, click **Subscription Notifications**.
1. Under **Email Notification**, enter the email address to where you want notifications directed when the app receives activation requests.
1. Toggle the **API Notification** switch to indicate you want notifications set via API requests, then enter where you want the request to be sent in the **Endpoint URL** input box.

> **Note:** If your app is subscription-based, you must enter a valid email address in the **Subscription Notifications** section before you can submit your app to Marketplace.

### Configuring Provisioning Options

1. Click **Provisioning Options** menu item to specify how you want to provision your app.
1. Choose a provisioning method from the following options:  
    * **Manual / Default Provisioning** – Provision the app outside of Fortellis manually
    * **Provisioning via API Endpoint** – Receive a notification at an endpoint when a App Buyer begins to provision your app
    * **Provisioning via Website URL** – Enter the the URL to a website you created to gather provisioning information from the App Buyer; Fortellis re-redirects the App Buyer to your website when they activate your app

## Additional Details

Complete the *Addition Details* forms to add special content related to your app.

In the **Additional Details** section:

* Select **App Notes** and add additional notes and comments to display with your app.
* Select **Status History** to see the latest status of the app. Status settings show date and time stamps.

## Customizing Your App Settings

Use the options in the **Settings** section to set the *visibility* of your app, and the *user activation* method.

### Public or Private Visibility

To configure the visibility of your app, select **Settings > Visibility**, then chose from one of the two options:  

* **Public** – Public apps listed in Marketplace are visible to all Marketplace users.
* **Private** – Private apps are visible only to those in the Organization containing the app, plus all those who are specifically given access.

If you select **Private**, you can enter the email addresses of the users you want to give access your app. You can also add additional users at any time through Marketplace. For details, see [Sharing Private Apps](/docs/tutorials/app-lifecycle/sharing-private-apps).

### Configuring User Activation

When an App Buyer activates your app, their ability to connect to the app is determined by how the app is configured.

Configure how the app handles user activation by setting **Settings > Activation** as follows:

* **Auto** – App Buyers are auto-enabled to activate an app and can complete the activation process without explicit approval from the App Developer.
* **Manual** – App Buyers are able to to manage their app connections after the API Developer manually approves their activation requests.

For details on the manual activation flow, see [Manual App Activation](/docs/tutorials/app-lifecycle/manual-app-activation).

## Submitting Your App for Review

When you have completed all the required fields in the **App Submission Form**, click **Submit For Review**, then click **Submit** in the **Submit App Listing for Review** confirmation pop up.

After you submit your app, the status changes to **Submitted**. For more on app statuses, see [Viewing the App Status](/docs/tutorials/solution-integration/app-status).

When the status of the app turns to **Approved**, publish the app as described in [listing Apps][/docs/tutorials/app-lifecycle/listing-apps)
