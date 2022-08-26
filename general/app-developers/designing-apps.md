---
title: Designing Apps
author: Nathaniel Sandberg
last updated: 3/24/2022
---

When creating a new app, take the time to plan it out in advance. Consider the tips below when creating a plan:

* Identify your market
* Identify your architecture
* Browse APIs looking for new functionality
* Identify the authorization that works with your architecture
* Create the look and feel of your UI

## Finding New Functionality

Look through the *API DIRECTORY*, a feature of Developer Network, to find APIs that offer functionality you can use to create a new app or enhance an existing one.

App Developers can work hand-in-hand with API Developers to coordinate products they can market. With the correct APIs, App Developers can create apps that best serve their clients.

To create unique apps, App Developers can do the following:

* Use multiple APIs to get content from more than one source
* Create unique user experiences

> For more on the API Directory, see [API Directory Listings](/docs/general/api-developers/api-directory).

### Requesting Special Pricing

When browsing the [API Directory]($[apiReferenceUrl]) for APIs you want to integrate with your app, review the pricing details of the APIs you are interested in using:

1. Sign in to the API Directory and click on API you want to use.  
    The API Details page opens.
1. Open the **Pricing** tab to see the available pricing information.  
    Pricing is often based on the number of calls you make to the API.

Depending on the app you're developing, you might be able to negotiate a special pricing plan for an API you see listed. To request special pricing:

1. Use the **Publisher** link at the bottom of the page to request custom pricing from the API Developer.  
    * Fortellis configures your message with the ID of your Organization, which the API Developer uses to assign pricing
    * The Subject line is pre-filled with: `Pricing Inquiry by {yourOrganizationName}`  
    Edit these items as necessary, being sure to include the details of the pricing plan you'd like to pay for your API subscription.
1. Click **Send** to complete and send your message.

**Note:** You can accept only one pricing plan for each API you select for your app.

## Identify Your Market

Here are some items to review when planning new app functionality

* **Marketplace** – Explore [Marketplace]($[marketplaceUrl]) to see existing apps and find a gap
* **Personas** – Create a persona that you can keep in mind throughout the development process
* **Persona Research** – Conduct research to find out what your user is like
* **Competitive Analysis** – Find other products that currently offer an app or a proxy app that may challenge you in Marketplace

## Identify Your Architecture

Identify the architecture for your app and understand the advantages and disadvantages of that particular architecture. You may ask yourself some of the following questions:

* Do I have a server or want to purchase one?
* Do I want to use shared hosting?
* Do I want to create my own authorization or use another authorization service?
* Do I want to create a device app, mobile app, or neither?
* How will the app scale with a given architecture?

## Identify the Authorization Method

When you identify the architecture that works within your constraints, you narrow the options for authorization.

* For machine-machine interactions, you can authorize using the [Client Credentials Flow](/docs/tutorials/solution-integration/client-credentials-flow):
    1. authorize the user on your end
    1. Use your credentials to get a token
* For Single-page Applications (SPAs), you must authorize with another service. With Fortellis, you can use OAuth and authorize with the [implicit flow](/docs/tutorials/solution-integration/implicit-flow).
* For simple apps with a backend, you can authorize using the [Authorization Code Flow](/docs/tutorials/solution-integration/authorization-code-flow).

See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth) for more information on each of the authorization flows.

## Creating the User Experience

Focus on the following when you start to create your app:

* What the screens are going to look like
    * Use wireframes to help plan the look of your app.
* How users will navigate through your app
    * Use storyboards to figure out how users will navigate through the application.

### UI Design Guidelines

You can use the following guidelines to create the user experience for your app:

* Conduct user testing often  
    User testing should give you enough information to develop your app for the next month after a two-hour session.
* Users will surprise you  
    They may not follow the "happy path" you expect. Leave room for recovery from errors in your workflow.
* Favor simplicity  
    Users will often use an inferior product that lacks functionality simply because they can use it.
* Users don't read pages  
    Users scan pages. Apps should facilitate scanning.
* Users form an impression of your app in the first few seconds  
    Be sure to welcome users with a solid entry into your app.

### Quick Tips

These app-development tips augment the general practices mentioned above:

* Use known conventions when possible
    Users are more familiar with a known convention than with an innovation.
* Create effective visual hierarchies
* Break pages up into clearly defined areas
* If it's clickable, make it obvious
* Eliminate distractions that disrupt the user's focus
* Format content to support scanning
