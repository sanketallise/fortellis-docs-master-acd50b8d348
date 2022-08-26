---
title: Fortellis Testing Environments
author: Kelly Rich
last updated: 4/19/2022
---

Fortellis supports two test environments to help you test while you're developing your APIs and apps:

* Dev
* Test

Both environments mirror the Production environment in functionality, but they have the advantage that any records you create or update are isolated from unauthorized access. These environments let you set up mock user accounts that can be seen only in the test environments, letting you focus your testing on your API services and app business logic instead of spinning wheels creating stub apps and services to support your testing.

Once you have your App credentials and your app's activation request has been approved by the API Developer, you can begin making calls to the APIs test environments, as provided by the API Developer . When approved, the account's **Subscription-Id** is enabled for the API and you can begin testing by using the host URIs specific to each environment.

Requests made to the test environments must be authorized using tokens you get via the OAuth client credentials flow. Here, you call the Fortellis token server to get a valid access token, as described in [Authorizing API Requests](/docs/general/making-calls/authorizing-requests).

## The Dev Environment

The *Dev* environment supports developers within an Organization during the API creation process. By populating the environment with the test accounts and data needed to test their service, developers can test APIs against the service they defined in the accompanying API specification. Before moving the API into Production, the service should correctly parse all incoming requests and return the correct payloads and error messages.

## The Test Environment

The *Test* environment is used by App Developers to test API workflows in their applications. APIs hosted in the Test environment can be viewed and tested by members outside of the Organization where the API is registered, and you can use you can give third-party users access to an API that's in the Test environment.

The Test environment is configured by API Developers to support App Developers who activate their APIs and services in the Test environment. This environment should mimic as closely as possible the behavior of the API when it is hosted in the Production environment.

After completing the testing in the Test environment, and App Developer can share their app as either a public or private app. In either case, they must update the endpoints for all the API calls in their app so they target the Production environment of the API.

For more on the Fortellis test environments, see [API Environments](/docs/tutorials/api-lifecycle/api-environments).
