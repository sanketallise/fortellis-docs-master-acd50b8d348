---
title: Registering Apps
author: Kelly Rich
last updated: 7/22/2022
---

The app development process begins when you register a new app in an Organization.

Registering an app involves establishing some key details about the app itself and selecting the APIs you want to use with the app.

Once the app is successfully registered, Fortellis provisions the app and generates a set of App Credentials that you use to authorize API calls made from the app.

## The App Registration Process

Access the **Register a New App** form from either Marketplace or Developer Network:

* From **Developer Network**:  
    1. [Sign in](/docs/general/overview/getting-started/#signing-in) to Developer Network and activate the Organization where you want to register the app.
    1. In the Fortellis header, click your name, and then click **Developer Account**.
    1. In the **Apps** section of the Developer Account page, click **New App**.  
* From **Marketplace**:  
    1. Sign in to [Marketplace]($[marketplaceUrl]), then click **Sell On Marketplace**.
    1. Click **Create an App**.

## The Register a New App Form

The **Register a New App** form contains two tabs where you enter the details of your app:

* Details tab
* APIs tab

> **Note:** Fortellis is in the process of migrating to new app registration flows. While you can register most apps with the updated flow, you might need to use the legacy app registration flow to complete the API configuration. If you do not see an **APIs** tab, then click **Next** after entering your *App details* get to the app's API configuration flow. For details, see [Legacy Configuration Flow](#legacy-configuration-flow).  

You know you're using the updated App Registration flow if you see these configuration options:

* Select the API type
* Filter the APIs by category
* Filter the APIs by Publisher
* Search for the API by name

### The App Details Tab

Complete the following on the app's **Details** tab:

* **App Name** – The name App Buyers see.
* **App Description** – Use up to 250 characters to enter a detailed description of the app.
* **Website** – Enter the URL for the app's home page.  
  You can leave this field blank during the development phase of your app, but you must have an app website in order to list the app in Marketplace.
* **Callback URL** – If you use either the implicit flow or the authorization code flow to authorize API requests.  
  For more on authorization models, see [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/).

![Registering Apps]($[docsUrl]/static/images/registeringApps.PNG)

Continue to the APIs section. If you want to configure a listing only app, see [Registering Apps Without an API](#registering-apps-without-an-api).

### The APIs Section

Use the **APIs** tab to select and configure the APIs you want to use with the app.

**Note:** Be sure to complete any special pricing requests you might have with API Developers before finalizing the app registration request. For more, see [Requesting Special Pricing](/docs/general/app-developers/designing-apps/#requesting-special-pricing).

Complete the app's **APIs** section as follows:

1. Select the API to use.  
    * Narrow the listed APIs using these filters:  
        * **API Type**  
            * **REST**  
            * **Async API**  
                **Note:** You must contact an administrator at [support@fortellis.io](mailto:support@fortellis.io) to activate your app with an asynchronous API.
        * **Category** if you know the API **Category**  
        * **Publisher** if you know the API **Publisher**  
    * Search for the name of the API if you know the name.  
        **Note:** You might not see the API if it is a private API and the owner has not shared it with you. Ask the API owner to share the API if you don't see it listed.  
            ![Selecting APIs]($[docsUrl]/static/images/asynchronousAPI.PNG)  
1. Configure the API.  
    **Note:** While most APIs have just a single associated implementation, some APIs have more than one. Either way, you must select the implementation you want to use for the API, then indicate the price plan you want to use with the selected implementation. This must be done for each API you configure.  
    * For an **Async API**, do the following:  
        1. Click **Webhook** to bring up the pop-up for your webhook.  
        1. Enter the public webhook where Fortellis delivers events.  
        1. Click **Save** in the pop-up.  
            ![Configuring Asynchronous APIs]($[docsUrl]/static/images/configuringTheAsynchronousAPI.PNG)  
    * For a **REST API**, do the following:  
        1. At the bottom of the page, click **Select Plan** next to the API if the API has any listed pricing plans.  
        1. Select the plan you want your Subscribers to have access to.  
        1. Click **Save**.  
        1. Click **Review Terms** to review the terms for that API if the API has terms to agree to.  
        1. Click **Yes**.
        1. Enter the following information:
            * Your **Name**
            * Your **Title**
            * Your **Company Name**
        1. Click **Accept Terms**.  
        ![Agreeing To Terms]($[docsUrl]/static/images/agreeingToTermsForApps.PNG)
1. Click **Register** at the bottom of the page.

Note that in order to list an app in Marketplace, the app must use at least one Fortellis API.

### Registering Apps Without an API

If you do not want to register an API with your app, click **Register** at the bottom of the page without entering any API information.

When you are creating a listing-only app, remember the following:

> * You can list an app in Marketplace without providing an API.
> * Users cannot activate to your listing-only app in Marketplace until you add a Fortellis API.
> * You cannot remove all the APIs from the app to make it a listing-only app.
> * You can add APIs to an app at any time, but you must create a new listing version for the API.
> * Include APIs before publishing your app to avoid having to make a new listing version.
