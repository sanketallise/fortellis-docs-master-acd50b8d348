---
title: Managing APIs
author: Nathaniel Sandberg
last updated: 6/9/2022
---

You can update several configuration options of an API, providing flexibility as you work with your customers and develop your API.

> For details on versioning API updates, see the [API Versioning Policy](/docs/general/api-design/api-versioning).

## Performance Metrics

On the API, click **Performance Metrics** to see your APIs metrics.
You see your the metrics for all of your APIs.
You can click into the individual API to see the following metrics for that API:  

* **Traffic**  
* **Latency**  
* **Error Rates**  

See [Viewing Your API Metrics](/docs/tutorials/api-lifecycle/api-metrics/) for more information on the metrics that you receive.  

## Updating an API

1. Sign in to [Developer Network Platform]($[devNetworkUrl]) and enable the Organization where the API is registered:  
    1. Hover over your name in the upper-right corner and click **Change**.
    1. Click the Organization where the API registered, then click **OK**.
1. In the API section, click **View All** and find the API you want to update.
1. Click on the API listing to view its Details Page.

### Creating an API in a Different Environment

You can create different instances of the same API by registering it in different environments. Even if they are identical versions of the API, each instance has its own base path, making calls to the same API in each environment unique.

Create versions of an API in different environments to:

* Experiment with Fortellis in the Dev or Test environments.
* Test your APIs.
* Give full production features to your API users in a non-production environment.

For more information on the environment options, see [API Environments](/docs/tutorials/api-lifecycle/api-environments).

Fortellis creates the proxy for your endpoint using the following:

* Your namespace
* The environment
* Your `basePath`

For more information on the target URLs, see [Target URL](/docs/tutorials/api-lifecycle/registering-apis/#target-url).

To create an existing API in a different environment, do the following:

1. Click the tab of one of the following environments that you can create the API in.
    * **Production**
    * **Test**
    * **Dev**
1. Click **Create New Version** to start creating a new API.
1. Follow the instructions in [Registering Your API](/docs/tutorials/api-lifecycle/registering-apis/#api-specification) to create your API in the new environment.

### Changing an API

> Click the banner at the top of the page to update previously existing APIs and specs.

1. Select the environment where you want to host the API.  
    **Note:** From the environment, you can see the following:
    * The **Version** of the API
    * The **Spec File**  
        **Note:** You can click the spec file to download it.
    * The **Proxy URL**
    * The **Access Control** the API uses
    * The **Show in API Directory** option selected
    * The **Status** of the API
    * The menu button to make changes to the API
    * The option to **Create New Version**
    * The option to **Manage Subscriptions** if you choose to manage those manually
1. In the **API Versions** section, click the menu button.
1. Click one of the following options:
    * **Postman Collection:** Download the Postman collection that Fortellis creates for you to test your API.
    * **Edit:** Edit the details of your API.
    * **Delete:** Delete your API.

### Editing Your API Details

**Note:** You cannot change the **Namespace** for your API once it is registered.

1. From the API page, click the environment.  
    **Note:** Users see the API in the API directory and can select the environment.
        Only users in your organization can see APIs in the dev environment.
1. In the **API Versions** section, click the menu button and **Edit** to change any of the following at any time:
    * Update the spec.
    * Update the **Target URL**.
    * Update the **Lifecycle Status**.
    * Update the API to **Show in API Directory**.
    * Update the **Access Control** method that you use.  
        **Note:** See [Access Control](/docs/tutorials/api-lifecycle/registering-apis/#access-control) for details on these options.
1. Click **Save Changes** at the bottom of the page when you are finished.  

> You must wait until Fortellis has provisioned the API to use it.

You return to the API details page after saving the API.

### Creating New Versions

You can create new versions of your API at any time to deliver updates to your API and offer your App Developers additional functionality.
App Developers must implement the new endpoint for the new version to get this functionality.
However, the existing API connections remain activated.
App Developers can switch to the new endpoint but maintain the `Subscription-Id` for each Organization and use the same authorization flow they had previously used.

1. In the **API Version** section, click **Create New Version**.  
1. Follow the instructions in [Registering APIs](/docs/tutorials/api-lifecycle/registering-apis) to create the new version of your API.  
    **Note:** You must create a new `basePath` in the spec
    or you get an error telling you the `basePath` must be unique.
    You can use the same `basePath` within a different namespace.

### Checking Errors

1. Click **Errors List** to see a list of errors.  
1. Select the following options:  
    * **Environment**
        * **Production**
        * **UAT**
        * **Dev**
    * **Version**
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
    **Request ID:** You can only use this filed to check on one request, and iut may not have the error code you select.

1. Click **Refresh** to get your most recent updates from your selection, the **Custom Timespan** or **15 Minutes**.

![Error Codes]($[docsUrl]/static/images/errorCodes.PNG)

#### Reading Error Codes

To ensure a great experience with your APIs, you can check on the error codes that your APIs are sending from Fortellis and take necessary actions to correct any issues.
Fortellis provides the following:

* Codes for the errors that are generated on the app and API.
* Brief explanations for the errors.
* Possible steps to fix the errors.

See [Fortellis Error Codes](/docs/general/making-calls/fortellis-error-codes) for the following:

* A list of error codes.
* Error code details.
* The cause of the error code.
* Actions required to correct the error.

### Managing Subscriptions

**Note:** You see this option regardless of your activation method.

1. Click **Manage Subscriptions** in the **API Versions** section.  
1. Check the API activations you'd like to **Activate** or **Deactivate**.  
1. Click **Activate** or **Deactivate** at the top of the page.  
    > If you choose to have any **Subscription-specific values**,
    > click **Configure** to configure those.
    > You can configure subscription-specific values at any time whether the user is already active.
1. Click **Configure** to configure any **Subscription-specific** values.
1. Enter the values to **Configure Credentials**.
1. Click **Save and Activate** to save the configuration and activate the subscription.  

### Credentials

The **Credentials** section shows you your `client_Id`, your `client_secret`, your test `Subscription-Id`, and your **Test Connection Status** or your **Test Deactivation**.  
**Note:** You only use your **Test Connection Status** to monitor test connections for the `Subscription-Id` that you complete either manually or through the Admin API.

### Activating API Connections

1. Click **Edit** to change your **Subscription Activation**.
1. Click one of the following:
    * **Automatic:** Fortellis automatically accepts any connection requests to your API.
        You cannot select automatic activations if you selected **Subscription-specific values**.
    * **Manual:** You manually approve your subscription requests through the UI.  
    * **Admin API:** You approve your subscription requests through the Admin API.
        See the [Admin API](/docs/tutorials/admin-api/overview) section for more information.
1. Click **Save**.

### API Details

You can see the following from the spec in this section:

* **API Name:** The title from the API spec
* **Description:** The description from the API spec
* **Categories:** The tags from the API spec

### Uploading Documentation

1. Click the **Documentation** section.
1. Click **+Documentation**.
1. Select the option to upload your documentation or enter the URL from an existing site.  
    To upload your documentation, do the following:  
    1. Enter the **Link Text** that Fortellis shows in the [API Directory]($[apiReferenceUrl]).
    1. Select **Choose File**.
    1. Select the file from your computer.
    1. Click **Open**.  
    – Or –  
    To enter a URL for an existing site, do the following:  
    1. Click **Paste URL**.
    1. Enter the **Link Text** that Fortellis shows in the *API Directory*.
    1. Enter the URL for the site that you use for your documentation.  
1. Click **Save**.

### Uploading Support Information

1. Click **Support**.
1. Click **Edit**.
1. Enter the following for your support team.
    * **Email**
    * **Phone**
    * **Ext** if any
    * **Website**
1. Click **Save**.

### Entering Your Pricing Information

1. Click **Pricing**.
1. Click **Pricing**.
1. Type the **Pricing Plan Name**.
1. Select one of the following types of visibility for the pricing plans:
    * **Private**
    * **Public**
1. Select one of the following types of pricing plans:
    * **Free**  
        **Note:** Subscribers can make unlimited free calls to your API.  
        * Click **Save**.
    * **Transactional**  
        **Note:** You charge your subscribers on a per transaction basis. You only have this option for REST APIs.
        1. Enter the number of API calls your subscribers can make for that tier.
        1. Enter the **Cost Per Call** at that **Tier**.
        1. Click **Tier**
        if you would like to add more tiers.
        1. Click **Save**.
    * **Subscription**  
        **Note:** You charge subscribers for their subscriptions within a given time.  
        1. Enter the **Cost Per Month**.
        1. Click **Freemium**
        if you would like to provide customers with free calls on a limited basis
        when they start using the API.
        1. Enter the number of free calls your subscribers will get.
        1. Click **Save**.

### Uploading Your Terms and Conditions

1. Click **Terms**.  
1. Click the pencil icon next to one of the following:  
    * **Terms**  
    * **Addendum**  
    **Note:** You can click the trash can icon next to the **Integration Addendum** to delete it.
1. Select **One-Click Acceptance**.  
    **Note:** Fortellis no longer supports signing through DocuSign.
1. Click **Choose File** to select the file from your computer.
1. For **One-Click Acceptance**, click **Save and Finish** to make subscribers agree to these
    1. Click **Save Terms** or **Save Addendum** to make subscribers agree to these
    before they can access your API.  
    – Or –  
    For **Electronic Signature**, click **Prepare Terms**.  
    1. Click **Signature**.
    1. Move the **Signature** to where you want it on the document.
    1. Click **Next**.  
    **Note:** You can view the preview of the terms and conditions on the next page.
    1. Review the terms that your API users are going to see.
    1. Click **Save Terms** or **Save Addendum** to make API subscribers agree to these
        before they can access your API.

### Sharing Your API

You make APIs private by default for the following reasons:

* You may need to continue developing your API.
* You don't want to share an API with others while you are still developing it.
* You may want to share your API with only certain parties while testing.
* You may only want to share your API with a few organizations.

When you have completed all the required fields,
you can do one of the following to share your API with others:

* You can click **Make Public** at the bottom of the page to make the API public.  
    After you make the API public, you can click **Revert to Private** to make it private again. You must agree to the Fortellis terms to make the API public.
    1. Click **Yes** on the terms page.
    1. Enter the following information:
        * Your **Name**.
        * Your **Title**.
        * Your **Company Name**.
    1. Click **Accept Terms**.
* You can click **Share API** to share the API with other organizations.  
    1. Enter the **Org Id** of the organization you want to share the API with.  
        **Note:** You must get the **Org Id** from someone in the organization.
        See [Getting the Entity ID of an Organization](/docs/general/overview/organizations/#getting-the-entity-id-of-an-organization) for information on how to get your Organization ID.
    1. Click **Share API** to add that organization to the list of organizations you have shared the API with.
    1. To confirm the organization you are sharing with, click **Share**.  
        You can now see the organization on the list of organizations that you have shared your API with.

#### Removing Organizations You Have Shared With

You can remove organizations that you have shared the API with.

* Click **Share API**, and then click **Remove** next to the name of the organization you don't want to share the API with.
