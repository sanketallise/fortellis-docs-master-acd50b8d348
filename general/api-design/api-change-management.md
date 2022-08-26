---
title: Change Management
author: Sandberg, Nathaniel
last updated: 6/10/2021
---

To maintain the continuity of your API, use the change management process to ensure your API consumers trust the API.
The change management process guarantees
that you keep your API consumers from experiencing any interruptions and
that API consumers continue to subscribe to the API.

Organizations must often decide how to perform the following:

* Manage their internal changes.  
* Make updates.  

To make changes to internal APIs, we expect organizations to work with the following groups:

* Internal stakeholders
* Decision makers
* Employees

However, with external APIs, organizations must carefully manage how they announce any changes they make to their APIs.

If existing API Developers make breaking changes to APIs, they risk the following:

* They risk destroying existing functionality in current apps.
* They risk consumers losing trust in the business.

Breaking changes include the following:

* Adding required fields
* Removing fields

See [Breaking Changes](/docs/general/api-design/api-versioning/#breaking-changes) for a full list.

## Supporting New Endpoints

If the new endpoint is replacing an old one,
perform the following:

* Mark the old endpoint as deprecated in your documentation.  
* Send out "deprecated" messages during run time for the users of that old endpoint.  
* Create a new endpoint in your documentation.  
* Highlight that new endpoint as the proper one to use.  

## Going Out of Business

If you can no longer support your APIs,
please notify your users.
When the APIs does go down,
any apps that relied on the API will no longer work.

You can recommend these steps to App Developers:

* Create new APIs from the same spec to meet their needs.  
* Use another existing API.  

Consumers must perform the following:

* Resubscribe to the app.  
* Select the other API.  

## Contact Information

Please email Fortellis support to get more information: [Fortellis Support](mailto:support@fortellis.io).
