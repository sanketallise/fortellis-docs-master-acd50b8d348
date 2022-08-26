---
title: Configuring Key and Secret Authorization
author: Nathaniel Sandberg
last updated: 3/30/2022
---

If you select **API Key and Secret  2.0** when registering your API, do the following:

You can use the same API Key and Secret for all API requests,
or you can use **Subscription-specific** parameters for each of the requests
that you can then configure later.
In both cases, you can add as many additional parameters as you want.

To use the same parameters for every request, do the following:

1. Enter the **Name** of the parameter.
1. Enter the **Value** of the parameter.
1. Select the parameter **Type**:
    * **Header**
    * **Query**  

To use specific credentials for each app buyer, do the following:  

1. Click **Subscription-specific credentials** to grey out the option to enter the authorization for Fortellis.  
    **Note:** You do not enter the value if you are using **Subscription-specific** values.
1. Enter the following information:  
    * The **Name**  
    * Whether it's a header parameter or a query parameter  

**Note:** When users request access to your API,
    configure buyer-specific values using one of the following:  

* Use [Connection Callback API](/docs/tutorials/admin-api/fortellis-connection-callback) when you activate connections to your API with one of the following:  
    * The [Admin API](/docs/tutorials/admin-api/admin-api-overview)  
    * [Manual activation](/docs/tutorials/configuration/updating-apis/#subscription-activation)  
* Configure the buyer-specific values when you are [Managing App Activations](/docs/tutorials/api-lifecycle/updating-apis/#managing-app-activations)
