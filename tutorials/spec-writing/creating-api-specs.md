---
title: Creating API Specs
author: Nathaniel Sandberg
last updated: 4/1/2022
---

You can collaborate with App Developers using API Specs.
With the API specs, the App Developer and the API Developer can work simultaneously
because they both know what the requests and responses are going to be for the API.
With the spec, you can develop integrations more quickly, leading to a faster time to market.

API Developers create the designs to implement to do the following:

* Collaborate
* Work asynchronously
* Map functionality

API Developers and App Developers need specs to get the following:

* Expected request payloads
* Expected response payloads

## Creating API Specs Using the Fortellis VS Code Extension

You can now use the Fortellis Visual Studio Code extension to create your APIs.
The extension does the following:

* Provides a preview of the human-readable spec.
* Provides you with immediate feedback on your specs.
* Enforces Fortellis specific rules that other editors may miss.
* Shows you the error in the document on the line where the mistake is.
* Offers options to update on save or update on change.

Follow the instructions [Visual Studio Code Extension](/docs/tutorials/spec-writing/vs-code-extension) to create specs for your API.

## Creating API Specs Using SwaggerHub

You can use the spec creator at [Swagger Hub](https://app.swaggerhub.com/apis) to get help when you are creating your spec.
You must create an account in [Swagger Hub](https://app.swaggerhub.com/apis) to complete the steps below.

1. Sign in to [spec creator](https://app.swaggerhub.com/apis) on SwaggerHub.
1. Click **Create New**.
1. Click **Create New API**.
1. Enter the name of your API, and click **Create API**.  
    **Note:** Currently, Fortellis uses Swagger version 2.0 and OpenAPI version 3.  
1. Use the editor to create your API.  

You can start with the boilerplate text below or use the template that SwaggerHub provides.

## Creating the API Spec Using an Existing API

Developers can also automatically generate API specs from an existing API using SwaggerHub.

1. Go to [Swagger Inspector](https://inspector.swagger.io/builder).
1. Make the calls to the existing endpoints.
1. When you have successfully called all the endpoints for your API,
    check the box next to the endpoints that you would like to use, and click **Create API Definition**.
1. Enter the name of the API and the version.
1. Click **Import**.

## Creating the API Spec Using the Swagger Editor

**Advanced Uses:** You can use the method below to create the following:

* Your spec
* Your API
* Your app

When you generate the code, you will have to do the following:

* Troubleshoot the code.
* Configure the code to work properly with your back end system.

You can also share the apps to help others onboard to your API.

1. Go to the [swagger editor](https://editor.swagger.io/).
1. Enter the information for the spec.  
  **Note:** You can start using the existing spec below or create your own spec from scratch.
1. When you are finished creating your spec and have no errors, you can click the **Generate Server** button at the top to create the API and run it locally.
1. When you are ready to use the API for an app, you can click **Generate Client** at the top to run it in your app's native environment.

## Example Specs

Use the example specs from the [Fortellis GitHub Repository](https://github.com/Fortellis/example-spec). For the best results, do the following:

1. Start with a working spec that passes the linter.
1. Make incremental changes to the spec.
1. Check that the updated spec passes the Fortellis linter using one of the following:
    * [Fortellis Spec Tools](https://marketplace.visualstudio.com/items?itemName=Fortellis.fortellis-spec-tools).
    * The spec upload on your developer account.
1. Continue to modify the spec incrementally until you have made the changes that you need to.

You can find more information on the rules for linting the spec on the [Linting API Specs](/docs/tutorials/spec-writing/linting-api-specs) page.
