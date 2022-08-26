---
title: API Versioning – Updating Your API
author: Nathaniel Sandberg
last updated: 7/6/2021
---

Use this versioning policy to keep APIs working for your consumers.

The versioning policy outlines the strategies and principles
that you can use to keep your API implementations updated and working with the apps.

This versioning policy should help you determine when and how to version your API
and what principles to keep in mind when updating your API to keep your API consumers loyal and your APIs working.

Follow this versioning policy to support apps that rely on your API.

## Responsibilities of the API Spec

The API Spec provides developers with the following:

* **Contract** – The API Spec is the contract between the API Developer and the App Developer, it details all the features in the API that are made available to the App Developer.
* **Specific operations** – The API supports specific operations, also known as "methods".
* **Specific requests** – Apps make specific requests to the operations in the API.
* **Specific responses** – Apps consume specific responses from the API.

An app will not correctly function with an API if the API service does not conform to its corresponding API Spec.

## Versioning Strategy Requirements

To make changes and improvements to APIs, API Developers can use a versioning strategy that performs the following:

* Preserves the functionality of existing APIs and apps.
* Provides additional functionality in new versions of APIs and new apps.

## Key Concepts

You should take into account these fundamental versioning concepts when you version your APIs.

### Postel's Law

Postel's Law says to do the following when versioning:

* Change responses conservatively.
* Accept requests from apps liberally.

With this mentality, we put together these common guidelines to help your do the following:

* Create new API versions due to the following:
    * Major changes
    * Minor changes
    * Patch changes
* Create a new version of your API using our guidelines.

Because you can disrupt current apps,
we recommend you preserve older versions of your API and sunset them gradually
when releasing new versions of your API.

### Principle of Least Astonishment

The principle of least astonishment says to remember the following
when determining whether a change is breaking:

* Users are part of the system that they use.
* The design should reflect the following attributes of the users:
    * Schemas
    * Mental models
    * Expectations

To reduce astonishment, API Developers can do the following:

* Make the API match the existing expectations of users
* Implement new features to behave according to the existing mental models that users have created

If you deviate too far from the mental model,
you may also determine that a change is breaking,
even if you consider the change minimal,
because the user does not have the knowledge of the underlying system that you do.

### Semantic Versioning

Fortellis recommends
that you use a semantic versioning strategy to introduce patches and minor version changes without breaking integrations.

#### Patches

Patches introduce changes that don't change the functionality of the API.

#### Minor Changes and Patches

Minor changes may add some functionality, but they don't produce a breaking change, such as a new required field.

#### Major Changes

Major changes require the App Developers to change how they consume the API.

#### Effects of Versions

Version changes are costly for both the API Developer and the App Developers
because they require many hours of testing to ensure the new API is working properly with the app.

#### Implementing Semantic Versioning

With semantic versioning, you can do the following:

* Identify your APIs with the following pattern for the version: Major.Minor.Patch.
* Initially release API specs with the first version 1.0.0.
* Number versions created before the initial release 0.X.X.  
    **Note:** The zero indicates that the API is still in development.
* Number minor and patch versions sequentially from zero.
* Restart numbering for minor and patch versions at zero when the next higher version change occurs.
* Include the major version number in the path when you are creating your APIs.

You can use any design for the `basePath` that you would like,
but we recommend the following:

1. Domain
1. Subdomain
1. Version
1. Dealership

Fortellis only recommends putting the major version number in the `basePath` for the following reasons:

* Semantic versioning would restrict the API Developer's ability to make changes to the spec and existing APIs.
* Minor and patch versions should not introduce breaking changes.

For example, World Car Dealership would use the following URI for its cars API: `https://world-car-dealership/sales/vehicles/v1/usedCars`.

For more information, see [Semantic Versioning](https://semver.org/).

### URI Versioning

We recommend using URI versioning to keep previous versions of an API
while you migrate users to the newer API version.
You should perform the following when updating versions:

* Indicate the version consistently.
* Keep resources separate.
* Create a new API every time the API Developer would like to provide a new version of the spec.
* Preserve the functionality of apps using the old API.

**Note:** We recommend you only use the major version in the `basePath`
because minor and patch versions should not break integrations.

API Developers support the following scenarios by updating the `basePath` for new specs:

* API Developers can preserve existing APIs.
* API Developers can create new APIs to support the new spec.
* Apps that have not switched to the updated URI are not affected by the change in the version.
* Existing apps can continue to function normally.

### Breaking Changes

In general, breaking changes result an increment to the *major* version number, while non-braking changes result in an increment to the *minor* version number.

If you roll the major version number, you need to consider all current subscriptions. Many API Developers "sunset" the current version when they roll out a new version of the API. During the sunset period, API Developers run the old version of the service alongside the new service version. This lets API subscribers migrate to the new version during the API sunset period.

You create breaking changes when the changes would cause apps on the previous version to no longer work correctly.
You create breaking changes when you do one of the following:

* Paths
    * Change the base path or the path to any of the existing API methods.
    * Change the name of an existing collection or resource.
* Requests
    * Add or remove required parameters in the HTTP headers, URL parameters, or request body.
* Responses
    * Add or remove a required response header or property from a response body.
    * Add or remove HTTP status codes.
* Methods
    * Change the behavior of the API.
    * Change the fault and error behavior of the API.

For example, your API sends information about customers from `https://yourURL.com/sales/cars/v1/used` in the following format:

```json
{
    "car": {
        "make": "Ford",
        "model": "F-150 XLT",
        "year": "2018"
    }
}
```

If you decided to remove the information on the year,
that would constitute a breaking change
because apps that relied on this information would now no longer function.

```json
{
    "car": {
        "make":"Ford",
        "model": "F-150 XLT"
    }
}
```

You could also decide that you would like to break the key model into the following to make it more granular:

* model
* sub-model

This would also be a breaking change because of the following:

* An existing app may use the old format for the model.
* Developers haven't configured the existing app to consume the information in the new format.

```json
{
    "car": {
        "make": "Ford",
        "model": "F-150",
        "subModel": "XLT",
        "year": "2018"

    }
}
```

#### Semantic Versioning for Breaking Changes

We recommend that you create an API spec with the new version and maintain API implementation using the old `basePath` to maintain existing integrations.
For major version updates, we recommend you do the following:

* Increment the major version by one.  
* Set the minor version to zero.  
    **Note:** Do not include the patch version in the versions of your spec.

### Non-Breaking Changes

With non-breaking changes, you can change the API without breaking any apps.

Non-breaking changes include the following:

* Collections, Resources, and Methods  
    * Add a collection, resource, or method.  
* Requests
    * Add parameters in headers, query parameters, and request body properties that are optional or have a default value.  
    * Add optional fields.  
* Responses  
    * Add properties to a response body.  
* Definitions  
    * Add an element to an enumeration.  

Non-breaking changes have the following qualities:

* You change the representation.
* The information is the same.
* Existing applications continue to work.
* New applications can start using new fields.
* Apps can send additional fields.  
    **Note:** You can add additional fields
    because the applications can still parse the JSON and
    find the information with the key in the key-value pair.

For example, you could update the API to include the information on the price of the vehicle and that wouldn't change the functionality of existing apps:

```json
{
    "car": {
        "make": "Ford",
        "model": "F-150 XLT",
        "year": "2018",
        "price":"32,000"
    }
}
```

#### Semantic Versioning for Non-Breaking Changes

For minor version updates, you typically perform the following:

* Increment the minor version by one.
* Set the patch version back to zero.

### Patches

Specs would only include patch updates
when updates won't affect the functionality of the spec, such as typos in the description or other errors
that don't affect functionality.

### Routing

You can update the URL,
and Fortellis correctly routes calls to the correct API.
We recommend including the version in the `basePath` to accomplish this.
You are responsible for contacting your customers and having them update to the new `basePath`
if you want them to use the new version.

#### Routing Calls through Fortellis

1. The app sends Fortellis API requests to the API Gateway using the following path:  
    `$[apiFortellisUrl]/{organizationNameSpace}[-dev|test]/{basePath}`  
1. Fortellis then routes calls to you API service using the following path:  
    `{yourURL}/` + `{yourBasePath}`.

**Note:** You are using the legacy version of the API registration process if you configured your API Spec its corresponding implementation separately in Fortellis. If so, Fortellis routes requests to your API at, `$[apiFortellisUrl]/{basePath}`.

We recommend you do the following to route calls through Fortellis and keep the `basePaths` unique:

* Update the major version in the `basePath` and the `version` in the spec to create a new version of a spec.
* Create a new spec with new resources if you wish to update the API implementation.

Fortellis recommends this method for two reasons:

* Prevent changes that could possibly break existing customers.
* Keep old applications using existing APIs working until the API Developer has deprecated that API.

### Sun-Setting and Deprecating APIs

When you roll out a new version of an API, you will want to deprecate the old version so you don't need to maintain two service implementations for the API.

*Sun-Setting* an API places an API version into a limited-maintenance mode, which gives API subscribers time to migrate to a newer version of the API. API Developers should provide a sun-setting and deprecation policy, stating upfront the policy for any sunset period.

For the best customer service, perform the following
when you sunset and deprecate APIs:

1. Make an announcement to inform all API users that they must move to the new version of the API.  
    This notice should include any sunset period and when you plan to deprecate the service.  
    API Developers using the Admin API should have their users' email addresses.
1. Sunset the API with a warning that defines the sunset period.
1. Deprecate the API when users have discontinued use.

### Migrating to Newer Versions

Fortellis recommends the following steps to migrate older users to the new API over time.

1. Keep the old endpoint up to avoid breaking existing apps.  
    **Note:** If the new version uses a different `basePath`,
    it functions as a separate API.
1. Send a notification to users about needing to migrate.  
    **Note:** You must implement the Admin API to obtain API subscribers' email addresses.
    For more information, see [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs).
1. Deprecate the old endpoint with warning messages.  
1. Contact Fortellis to deactivate the old endpoint.  
