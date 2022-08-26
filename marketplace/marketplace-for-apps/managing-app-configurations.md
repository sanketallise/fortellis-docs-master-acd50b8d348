---
title: Managing App Configurations
author: Kelly Rich
last updated: 6/7/2022
---

After activating a Fortellis app, sign in to your connectCDK account to manage your app activations.

When you sign in, you're presented with the **App Management** page for the account with the **Active** tab having focus.

The **Active** tab lists all your currently active apps and, among other items, it lists each app's activation method (**Activation** or **Subscription**) and the number of currently activated locations.

![The App Management page]($[docsUrl]/static/images/marketplace/app-management-page.png)

## The App Context Menu

Manage an app by clicking the menu icon on the right side of the app's listing, then choose from the menu options:

![The App Context menu]($[docsUrl]/static/images/marketplace/app-context-menu.png)

* [**Configure App**](#configuring-apps) — Configure app provisioning details.  
    **Note:** This option is displayed only for apps that let App Buyers configure the provisioning details for the app.
* [**Add Locations**](#adding-locations) — Give one or more locations access to the app.
* [**Remove Locations**](#removing-locations) — Remove an app from a location where it is enabled.
* [**Manage DMS Accounts**](#managing-dms-accounts) — For apps that integrate with a dealership's DMS, this option provides access to the DMS account.  
    **Note:** This option is displayed only for apps that integrate directly with the DMS.
* [**Manage API Integrations**](#managing-api-integrations) — Manage the integration between the app and the APIs it uses, by dealership.
* [**App Documents**](#viewing-the-app-consent-forms) — Review the consent documents created for each app activation.
* [**Deactivate/Unsubscribe**](#deactivating-and-unsubscribing-from-apps) — Deactivate or unsubscribe from an app.

### Configuring Apps

The **Configure App** menu option is available only if the app or the APIs used by the app have provisioning details that need to be configured by the App Buyer. Use this option to finalize the app-configuration details.

When you select this menu option, you are redirected to an **App Configuration** page that is provided and maintained by the App Developer. Follow the instructions there to configure the app's provisioning details.

![Configure App menu option]($[docsUrl]/static/images/marketplace/app-context-menu-configure.png)

### Adding Locations

This option lets you add the dealerships that you want to have access to the app:

1. Choose **Add Locations** from the app's context menu.
1. If the app is subscription-based, you are prompted to select a pricing plan to use for the locations you are adding. When presented with the **Choose Your Pricing** dialog, select the plan you want to use and click **Proceed**.
1. On the **Where would you like to use this app?** page, check the location(s) where you want to add app connectivity, then click **Next**.  
    ![Adding Locations]($[docsUrl]/static/images/marketplace/app-locations.png)  
1. The **Review and Agree to Terms** page displays showing the added locations inherit the acceptance configurations from the other previously activated locations. Review the consent details here and click **Next** when you are ready.
1. If necessary, verify your DMS account for each location you add:  
    1. On the **Verify DMS accounts** dialog, select the correct DMS account to use from the dropdown menu.  
        If needed, enter a **Custom Value** DMS value, then click **Next**.
    1. Click **Next** to confirm your selections.  
1. Enter any installation notes or questions you might have, then click **Complete Activation** or **Complete Activation**, as appropriate.

### Removing Locations

This option lets you cancel an activation for a specific dealership location.

To remove the connectivity to an app from a specific location:

1. Choose **Remove Locations** from the app's context menu.
1. Click the locations from which you want to cancel app access, then click **Next**.
1. Click **Remove** to confirm the removals.

### Managing DMS Accounts

This option is enabled for apps that integrate directly with the DMS and gives you the ability to manage the DMS configuration via the Marketplace app listing.

From the app listing:

1. Choose **Manage DMS Accounts** from the app's context menu.  
    ![Manage DMS Accounts context menu item]($[docsUrl]/static/images/marketplace/app-context-menu-manage-dms.png)
1. Select the appropriate DMS for each location from the location's drop-down menu.  
    You can also enter a custom value in this field, as needed.
1. Click **Save** at the bottom of the page.  
    ![The Manage DMS Accounts page]($[docsUrl]/static/images/marketplace/manage-dms-accounts-page.png)

### Managing API Integrations

Some apps allow you to enable or disable the APIs used by the app. If so:

1. Choose **Manage Integration** from the app's context menu.  
    The **API Integrations** page, showing all the APIs used by the app.  
    ![Managing Integrations]($[docsUrl]/static/images/marketplace/api-integrations-page.png)
1. Click the **Manage** button for the API you want to configure, the **Integrations** page displays.
1. Toggle the **Enabled/Disabled** switch to enable or disable the API for each of the listed locations as needed.  
    **Note:** You may not have the option to disable an API if there are no substitute APIs listed for the app.
1. Click **Save** at the bottom of the page.

### Viewing the App Consent Forms

The **App Documents** menu item lets you review the consent forms created for each app activation or subscription.

From the App Management context menu, select **App Documents**. The App Documents page displays, listing all the current consent forms. Click the download button on the right to save and review any of the listed consent forms.

![App consent forms]($[docsUrl]/static/images/marketplace/app-consent-forms.png)

### Deactivating and Unsubscribing from Apps

Depending on the activation method of the app, you either deactivate the app or unsubscribe from it. Either way, when you cancel the activation of an app, the action removes all configured locations.

1. Choose **Deactivate** or **Unsubscribe** from the app's context menu.
1. Confirm the action on the resulting confirmation page.

See the section below for details on reactivating an app.

## Checking the Status of Your Apps

After making updates to an app, use the **Pending** tab to check the status of the app:

1. [Sign In]($[accountsServerUrl]) with your connectCDK credentials.  
    The **App Management** page displays.
1. Click the **Pending** tab, then search the displayed list of apps for the one you're reviewing.

## Reactivating and Resubscribing to an App

You can use the **Unsubscribed/Deactivated** tab to view and reactivate the apps that have been previously deactivated.

1. [Sign In]($[accountsServerUrl]) with your connectCDK credentials and open the **Unsubscribed/Deactivated** tab.
1. Scroll to find the app you want to reinstate, then click the app's **Reactivate** or **Resubscribe** button, as appropriate.

Even though the app was previously active, you still need to reconfigure its settings when you reactivate it.

For details on activating and configuring apps, see [Activating Apps](/docs/marketplace/marketplace-for-apps/activating-apps).
