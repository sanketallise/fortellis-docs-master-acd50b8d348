---
title: Activating Apps on Marketplace
author: Kelly Rich
last updated: 6/7/2022
---

*Activating* an app is the culmination of a process that begins when you select an app to use. The process ends when you are able to manage the active app connections and dealers are able to use the app's functionality.

You must have a *connectCDK account* before you can activate apps on Marketplace. For details, see [Your connectCDK Account](/docs/marketplace/marketplace-for-apps/connectcdk-account).

## Activating Apps

With all Fortellis apps, begin the activation process on an app's **Details page**.

![App Details page]($[docsUrl]/static/images/marketplace/app-details-page.png)

### Activation versus Subscription Apps

Depending on how the app is provisioned, the left nav of the Details page displays either an **Activate** button or a **Subscribe** button. While the activation process is nearly identical for each, there is a difference between the two in how the terms-of-use are negotiated.

* Apps with **Activate** buttons require you to have a relationship with the App Developer before the app becomes enabled for activation.

* Apps with **Subscribe** buttons let App Buyers use Marketplace web flows to complete the agreements needed for the app's activation.

### Activation Apps

If you have a relationship with the App Developer for an app that displays an **Activate** button, then click **Activate** and complete the activation flows through Marketplace.

If you do not have an existing agreement with the App Developer, send them a message as described in [Contacting the App Developer](/docs/tutorials/marketplace-for-apps/marketplace-for-apps/#contacting-the-app-developer) and negotiate an agreement with them so you can complete the app activation process.

> **Note:** Not all apps require the App Developer to approve your request to use the app; App Developers can choose to auto-enable the app for all users. However, if the App Developer decides to monitor those who want to connect to their app, then you need to set up a relationship with the App Developer before you can complete the app activation process.

### Subscription Apps

The activation process for apps with a **Subscription** button is slightly different from the process used for those with an **Activate** button. With *subscription* apps, you select the pricing plan and terms of service for the app using Marketplace web flows.

When App Buyers subscribe to apps using Marketplace flows, Fortellis manages the contracts between the App Buyer and the App Developer.

## The Activation Process

To begin, navigate to the [Marketplace]($[marketplaceUrl]) and find the Details page of the app you want to use as described in [Browsing Apps](/docs/general/app-buyers/marketplace-for-app-buyers/#browsing-apps-on-marketplace).

Once on the app's Details page, click the **Activate** or **Subscribe** button to begin the activation process.

If the app contains an **Activate** button, click the button and jump to the [Configuring App Locations](#configuring-app-locations) section below to continue the activation.

If the app contains a **Subscribe** button, click the button and configure the pricing plan you want to use with the app.

### Navigating to the App Details Page

It is easiest to navigate to [Marketplace]($[marketplaceUrl]), select the app you want to use, then sign in to your connectCDK account during the activation process.

However, if you're already signed-in to your connectCDK account, click the **Browse Fortellis Marketplace** button from the **App Management** page to get to Marketplace.

![Browse Fortellis Marketplace button]($[docsUrl]/static/images/marketplace/app-mgmt-browse-mkpl-button.png)

### Configuring App Pricing Plans

If an app has a **Subscribe** button on its Details page, clicking it routes you to the app's **Pricing** tab, where you select the pricing plan you want to use with the app.

* Click **Subscribe** button for the pricing plan you want to use.
    ![App Pricing Plans]($[docsUrl]/static/images/marketplace/app-pricing-plans.png)

After configuring the pricing, continue by configuring the locations where you want to use the app.

### Configuring App Locations

Select the location(s) that you want to give access to the app using the **Where would you like to use this app?** dialog box. Tick the check boxes next to the locations where you want to use the app:

![Selecting App Locations]($[docsUrl]/static/images/marketplace/app-locations.png)

#### Configuring the Customer Master File

If one of the selected locations has more than a single Customer Master File (CMF) configured for the location, the location listing will have a drop-down box on the right side of the location that lists all configured CMFs.

In this case, you need to configure the location to use a single CMF with the app:

1. Select the location by ticking the check box on the left side of the listing.
1. On the right side of the listing, click the arrow in the drop-down box to display all the CMFs configured for the location.
1. Click the appropriate CMF number to choose the CMF you want the app to use.

![Select Locations]($[docsUrl]/static/images/marketplace/selectLocations.png)

If you are unsure of the correct CMF number to use for a location, speak with your Dealer's Enterprise Administrator, or contact the CDK Customer Care team at **(866) 668-5394, option 8**.

After selecting the locations that you want to have access to the app, click **Next** to display the **Review and agree to terms** page.

### Agreeing to the Terms of Use

You must accept the agreements listed on the **Review and Agree to Terms** page before you can proceed:

1. Click the **Review and agree to terms** button for each set of terms listed on the page.
1. Click **Yes** after reviewing each set of terms to accept the listed terms.
1. Click **Next** after accepting all the listed terms and conditions to continue with the app activation.

Apps can have one or more sets of terms and conditions, including those listed here:

|Agreement|Purpose|
|--|--|
|**CDK data rights**|The "Addendum to Master Services Agreement" that lets CDK collect and use information from your dealer management system|
|**App terms**|The terms of use provided by the App Developer|
|**Data integration agreements**|Data integration terms of use added by third-party information providers|  

#### CDK Data Rights

You must agree to the CDK data rights addendum, noting that you only have to sign the CDK data rights consent form once for each Fortellis account.

#### App Terms

To accept the app terms and conditions:

1. Click on the **Accept Terms** button for the terms you want to review.
1. Click **Yes** under **Do You Accept?** at the bottom of the Terms and Conditions dialog box.  

A green **Terms Accepted** icon displays next to the terms that you've accepted.

#### Data Integration Agreements

Complete any Data Integration Agreements as follows:

1. Click **Review Terms** to review the data integration agreement.
1. Click **Yes** at the bottom of the modal.

**Note:** Once you have signed the **Data Rights Consent** for a dealership, this agreement applies to all related dealerships. You do not have to sign it again.

![Agreements]($[docsUrl]/static/images/marketplace/app-terms-and-agreements.png)

## Configuring the DMS

This option is enabled for apps that integrate directly with the DMS. Apps with this option give you the ability to manage the DMS configuration directly from the Marketplace app listing.

If the **Verify DMS accounts** page displays, configure your DMS account using one of the following methods:

* Select the appropriate DMS from the dropdown menu.  
    – Or –
* Enter a **Custom Value**, as needed.

![Verify DMS Account]($[docsUrl]/static/images/marketplace/verify-dms-accounts-page.png)

## The Activation Completed Page

After completing the above steps, the **Review Activation** page displays and you can review the activation details before finalizing the configuration. On the page:

1. Review the activation details and:  
    1. Share the activation details by email populating the **Email** input box.
    1. If you need, send inquiries to the App Developers using the **Installation Notes/Questions** input box.
1. When everything looks right, click **Complete Activation** or **Complete Subscription** to finalize the activation process.

![Complete Subscription Notes]($[docsUrl]/static/images/marketplace/review-subscription-dialog.png)

### App Activation Status

Depending on how the App Developer configured user activation for the app, you can either immediately configure and start using the app or you must wait until the App Developer approves your activation requests before the app is available for a dealership.

If the App Developer reviews activation requests, Fortellis sends an email to the dealer notifying them that a request is *Pending*.

![Pending Subscription Request]($[docsUrl]/static/images/marketplace/app-subscription-pending.png)

Activation requests are reviewed by the App Developer on a location-by-location basis, and it is possible for the App Developer to reject one or more of your activation requests. Your dealership account is notified whenever the status of one or more of your activation requests change.

When *Approved*, the App Developer notifies you of any configuration settings that need to be made to use the app.

If you are notified that an activation request was *Rejected*, contact the App Developer to negotiate the activation status.

### Configuring the App after Activation

Once a dealership is *activated* to use an app, it might need to be configured before you can use it.

If the **Configure App** button displays on the **Activation Successful!** or the **Subscription Successful!** page, it indicates you must configure the app, or the APIs used by the app, before the app connectivity can be completed.

Clicking the **Configure App** button redirects you to the *App Configuration* page, which is provided and maintained by the App Developer. Follow the instructions provided there to complete the configuration, noting that any external configurations you make to use the app are separate from any agreements you have with Fortellis.

And that's it!

![Successful Subscription with Configure App button]($[docsUrl]/static/images/marketplace/successfullSubscription.png)

> **Note:** It can take the service a few moments to check the app configuration and display the **Configure App** button, so pause a minute here to ensure your configuration is complete. To configure the app provisioning outside of the activation flow, see [Managing App Subscriptions](/docs/marketplace/marketplace-for-apps/managing-app-subscriptions).
