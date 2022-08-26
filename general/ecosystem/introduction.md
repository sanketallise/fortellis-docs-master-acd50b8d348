---
title: The Fortellis Ecosystem
author: Kelly Rich
last updated: 3/21/2022
---

**Fortellis** revolutionizes the traditional automotive dealership business model with a scalable and feature-rich software platform that is customized for the industry.

The Fortellis platform works in the automotive retail and services vertical and provides software that simplifies consumer, dealership, and OEM workflows in the operations, services, and retail spaces. The platform provides APIs created by CDK and other 3rd-party service providers (ISVs) that application developers can leverage to create innovative applications for OEMs, dealer groups, and other ISVs.

Fortellis supports API Developers and App Developers by providing:

* A *Marketplace* where dealership service providers select and activate the apps that enhance the solutions they offer
* A Marketplace where *App Developers* list the apps they create for the Fortellis platform
* An API Directory where *API Developers* offer industry-standard Fortellis APIs to App Developers

Fortellis provides the gateway that connects API services with App Developers and service providers to create integrated solutions.

By relying on the collaborative efforts of the top players in the industry, Fortellis is able to build an industry-standard cutting-edge platform.

![The Fortellis Platform]($[docsUrl]/static/images/high-level-fortellis-architecture.png)

> The rest of this *Ecosystem* section primarily addresses the developer audience. For details on Marketplace and activating Fortellis apps, see [Marketplace for Apps](/docs/marketplace/marketplace-for-apps/marketplace-for-app-buyers).

## Developer Network

[Developer Network]($[devNetworkUrl]) is the website devoted to Fortellis API and App Developers. Create a Developer Network account, then create Organizations where you can register and manage your APIs and apps.

> For more, see [Getting Started](/docs/general/overview/getting-started).

## API Developers

API Developers use the Developer Network environment to collaborate on, test, and market the Fortellis services they create.

You can create Organizations and invite team members to test an API until it's ready for General Release.

When you have a running implementation of the service, along with the API Spec that describes the service's features, list the service in the *API Directory*. The [API Directory]($[apiReferenceUrl]) is where App Developers look for new services to expand their offerings, from experience they know they can trust the APIs listed there.

### APIs on the Fortellis Platform

Fortellis encourages the development of third-party Fortellis APIs that grow the features and functionality available to App Developers.

For API Developers, Fortellis provides:

* An environment in which they can test their API implementation
* A proxy server to their API implementation
* A Developer Network that allows a team of developers to collaborate on APIs and apps
* The spec for an Admin API that offers extra API security by working with the Fortellis back-end to handle user access and activation tracking
* An API Directory that provides a listing of production-ready Fortellis APIs

API Developers may include members of the following groups:

* Dealers who want to leverage their own data in a modern way
* Original Equipment Manufacturers (OEMs) that want to provide information to end consumers, dealerships, and other third parties
* Dealer Management Systems (DMSs) developers that provide data services to dealerships
* Independent software developers (ISVs) who can offer new services that work in conjunction with those found in the API Directory

Fortellis APIs work in conjunction with a variety of databases and data stores, including DMS CRM, OEM, and other industry data stores.

> For more, see [Developing APIs](/docs/general/api-developers/developing-apis).

### Asynchronous APIs

Fortellis Event Relay is a message broker that sends asynchronous messages from APIs to app webhooks.

> For more, see [Asynchronous API Overview](/docs/tutorials/event-relay/overview-event-relay/).

## App Developers

App Developers use the Developer Network environment to help incorporate and test the Fortellis APIs they use in applications they create. When an application is ready to monetize, it goes from testing to General Release on production, and the App Developer can build out their listing on Marketplace to promote their app to App Buyers.

Developer Network gives App Developers access to:

* The *API Directory*, where they can find APIs that they can use to create app functionality
* *Marketplace*, where they can list their apps to be viewed and activated by App Buyers
* Two test environments, Test and Dev, that give App Developers a way to test calls to APIs

The App Developer audience includes users from the following groups:

* Dealers that want to promote their custom app to a larger audience
* OEMs that would like improve business processes
* ISVs that see an opportunity to create a new app

> For more, see [Developing Apps](/docs/general/app-developers/developing-apps).

## The Fortellis Marketplace

When an App Developer deems their app is production-ready, they can list it in *Marketplace*, a location specifically configured to serve App Buyers and other related audiences in the dealership vertical, including OEMs, dealer groups, and other ISVs.

The [Marketplace]($[marketplaceUrl]) uses a formalized activation path, giving App Buyers a familiar way to select, enable and activate apps and services for the locations the choose. For App Developers, Marketplace also provides administration tools that help with the activation management of App Buyers.

Listing an app in the Marketplace lets App Developers promote:

* App features and functionality
* Pricing plans
* Trial options
* Information and support for the app

> For more, see [Marketplace for Apps](/docs/marketplace/marketplace-for-apps/marketplace-for-app-buyers).
