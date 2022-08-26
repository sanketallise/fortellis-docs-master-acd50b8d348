---
title: Updates to API Workflows
author: Nathaniel Sandberg, Daniel New
last updated: 2/21/2022
---

Late in 2021, Fortellis improved the API registration process by providing the following:

* Streamlined, self-service workflows
* Configuration options that make API integration more intuitive and easier to use
* Stand-alone APIs that merge API specifications and API implementations into a single resource

## API Specs

Developers are no longer required to register API Specs separate from their implementations, and there is a single review processes for the two artifacts. You now upload your API Spec as part of the API registration and update workflows. In addition, the stylistic conventions that were previously enforced are now relaxed.

## Organization Namespaces

In order to provide greater freedom over your API specs, we enforce a new **Organization Namespace** scheme that is added before your API `basepPaths`. Think of the namespace as a unique domain that identifies an API as belonging to your Organization. The scheme is as follows:

```bash
/{organization-namespace}[-dev|test]/{remainder-of-basepath...}
```

There is also an environment-identifier suffix that's attached to your namespace. See the section **Environments** below for details.

Your default API namespace uses the domain found in the email you used to register your Organization. The namespace is added to the URLs of your API endpoints and these are unique to your Organization. You can use any unique `basePath` you want within the same Organization. To change your namespace, or add another, contact [Fortellis Support](mailto:support@fortellis.io).

## API Versions and Environments

You can register multiple versions and environments for your APIs. A unique version and environment combination is called an API instance.

### Versions

When you create an API instance, the version is assigned from the version number contained in the associated API Spec.

### Environments

When creating a new API instance, select the environment in which you want it to run. The following options are available:

* **Dev**
* **Test**
* **Prod**

App users make API requests to your API through Fortellis at `$[apiFortellisUrl]/{yourNamespace}-{env}/{spec-basePath}/{yourEndpoint}`. In succession, Fortellis routes API requests to `yourURL.com/{yourBasePath}/{youEndpoint}`. See the snippets below for example URLs to each of the supported environments:

Dev:

```bash
$[apiFortellisUrl]/{yourNamespace}-dev/{yourBasePath}/{yourEndpoint}...
```

Test:

```bash
$[apiFortellisUrl]/{yourNamespace}-test/{yourBasePath}/{yourEndpoint}...
```

Prod:

```bash
$[apiFortellisUrl]/{yourNamespace}/{yourBasePath}/{yourEndpoint}...
```

### Configurable API Authorization

You can now configure Fortellis API Gateway to authorize itself with your API when proxying requests. Select from the following options:

* **Fortellis OAuth 2.0**
* **External OAuth**
* **Custom OAuth 2.0**
* **API Key and Secret**
* **Basic Auth**

### Lifecycle Status

You can also select a **Lifecycle Status** for your API instance. The following statuses are available:

* **Beta**
* **General Release**
* **Deprecated**
* **Custom**

## Managing Your APIs

Additional self-service tools are now available to help you manage your registered APIs and API instances.

### API Visibility

You now have the ability to manage the visibility of your APIs. When you initially register an API, the visibility defaults to `private`. You can change the visibility of your API to `public` when the following conditions are met:

* You have attached API developer documentation
* You have attached support information
* You have defined one or more pricing plans
* You have attached terms and conditions

### Subscription Management

View and manage the connections to your APIs from the Manage Subscriptions page. Click **Manage Subscriptions** from the API Details page to access the API management functions.

The API Details page displays which Organizations and applications have connections to your API. You can also view and manage the status of the API connections by selecting "Manual" as the activation model for your API.

## Managing Legacy APIs

You can still manage legacy API implementations and API Specs if they were created before we merged the two entities. However, you can no longer create new independent API Specs and implementations.
