---
title: App Development Lifecycle
author: Kelly Rich
last updated: 3/29/2022
---

App Developers make use of Fortellis APIs to create end-user products that DMS users interact with at their terminals.

To get started as a Fortellis App Developer, create a Developer Network account as described in [Getting Started](/docs/general/overview/getting-started).

The App lifecycle begins when you register an app in an Organization, and it lasts through when you monitor the activations to your app, update the app, and finally sunset and decommission the app.

## Accessing APIs

App Buyers access app functionality using this workflow:

1. App Buyers activate the app and configure the API to use with the app.
1. App Buyer causes an action that triggers an API request.
1. App passes the Subscription ID of the App Buyer along with an authorization token generated using the app's credentials to the Fortellis API endpoint.
1. Fortellis passes the OAuth token to the API service for authorization.
1. The API service uses the Subscription ID to identify the App Buyer and verify that consumer.  
    **Note:** Apps always access APIs on behalf of the App Buyer.

## API Authorization Flows

We use the [OAuth 2.0](https://en.wikipedia.org/wiki/OAuth#OAuth_2.0) authorization protocol, a standard in the industry.

> For more, see [Authorization on Fortellis](/docs/tutorials/solution-integration/auth).

## Marketplace for App Developers

App Developers access Marketplace for the following:

* List their app and upload marketing material
* Sell apps and Negotiate pricing
* Review and Manage app activations
* View the configuration of an activated app
* View their app reviews
* View leads

> For more, see [Listing Apps in Marketplace](/docs/tutorials/app-lifecycle/listing-apps).
