---
title: Sharing Private Apps
author: Kelly Rich
last updated: 6/10/2022
---

When you create and publish a private app in an Organization, anybody who is a member of that Organization can view and activate the app via the Organization's Developer Account page. With private apps, users outside the app's Organization cannot view or use the app.

> **Note:** For more on organizations, see [Working with Organizations](/docs/general/overview/organizations).

## How to Share a Private App with a Third-Party User

Note that before you can share an app, the app must be listed in Marketplace. To create a private app listing in Marketplace, set the *Visiability* of the app to **Private**, as described in [Listing Apps](docs/tutorials/app-lifecycle/listing-apps/#customizing-your-app-settings).

To share a private app with users who are not members of the Organization where the app resides:

1. Sign in to [Marketplace]($[marketplaceUrl]).  
1. Select the Organization the app belongs to:  
    1. Hover over your name in the upper-right.
    1. Click **Change** to get a list of organizations.  
    1. Select the organization.  
    1. Click **OK**.  
1. Select the app:  
    1. Hover over your name.  
    1. Hover over **Marketplace**.  
    1. Click **Apps**.
1. Select the app from the list.
1. Choose **Share** from the menu on the app's Details page to configure access for users outside the Organization.  

    ![App listing and manage button]($[docsUrl]/static/images/general/app-home.jpg)  

    The app's **Share** page opens and you manage user access from here as follows:  
    1. In the **Share** input box, enter the email address of the Fortellis user with whom you want to share your app, then press **Enter**.  
        You can give access to multiple users and they can have email addresses that are either internal or external to your company.  
        The **Share** box has an input line for entering addresses and list box that displays the users to who you've given access. To disable access for a user, click the `X` button next to the user's email in the list box.  

        ![Invite App Users]($[docsUrl]/static/images/general/invite-subscribers.jpg)  

    1. The URL of your app's Details page is automatically populated in the **Share App URL** input box.  
        You can copy the URL to the clipboard by clicking the **Copy** icon next to the URL.
    1. Check **Send Email** if you want to send an email invitation to everybody on the Share list.  
        Users on the Share list do not need to verify their email address via the email that is sent. All users on the Share list can activate your app if they have the URL to the app's Details page.
1. Be sure to click **Save** when you have completed the configuration to save the new access rights.

If you checked the **Send Email** box on the Share page, Fortellis sends email the users you invited. The email includes a **View App Details** button that navigates the invitee to your app Details page. From there they can click **Activate** to the initiate the activation process.

> **Note:** Only the users who have been granted access to an app are able activate the app and use it. While any user view the app's Details page through the URL, Fortellis issues an error message if the user tries to activate the app without explicit permission.

![Invite email]($[docsUrl]/static/images/marketplace/invite-email.jpg)
