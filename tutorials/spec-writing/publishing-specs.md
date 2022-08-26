---
title: Publishing APIs
author: Nathaniel Sandberg
last updated: 4/1/2022
---

You can make your API available to any user in the API Directory,
or you can make your API private so it's only available to people in your organization.
When you make an API public, you can make your API available to all developers in Fortellis.

## Validation

For spec validations, the spec must pass the [OpenAPI](https://swagger.io/specification/) validation and the Fortellis schema.
The spec validation checks the following requirements:

* **Spec File in JSON or YAML**  
    * You must use a spec file that passes the Fortellis linter.
    See [Linting API Specs](/docs/tutorials/spec-writing/linting-api-specs/).  
* **API Name**  
    * You must use a unique name for your API within your organization.  
* **Description**  
    * Users see the description you enter in your spec in the [API Directory]($[apiReferenceUrl]).  
* **Category**
    * Fortellis uses the tag in your spec to categorize your API.
* **Version**  
    * Fortellis pull the version of your API from the spec.  
    * We recommend you put the version in the `basePath`.  
* **Proxy URL**  
    * Users call this URL to send requests to your API through Fortellis.  
* **Target URL**  
    * Fortellis sends requests sent to the proxy URL to the target URL you enter.
* **Lifecycle Status**
    * Use the **Lifecycle Status** to indicate the status of your API.  
* **Show in API Directory**  
    * Check this option to show your API in the [API Directory]($[apiReferenceUrl]).
* **Authentication Method**
    * Use the authentication method to enter how you want Fortellis to authenticate requests to your API.  

## Fortellis Schema Checks

|Rule|Description|
|--|--|
|Example Responses|Provides examples per the response schema|
|`basePath`|Provides the `basePath` field so app developers can call the API|
|`operationId`|Defines the API operation object|
|`tags`|Categorizes the spec within Fortellis|
|Description|Describes the API|
|Description of operation|Describes each operation|
|Standard request headers|Provide the Fortellis [standard headers](/docs/api-design/api-data-schemas/#use-of-standard-http-headers) in the request|
|Standard response headers|Provide the Fortellis [standard headers](/docs/api-design/api-data-schemas/#use-of-standard-http-headers) in the response|
|Standard request body|Provides the standard format between all Fortellis APIs for a better developer experience|
|Standard response body|Provides the response standard format between all Fortellis APIs|

## Fortellis Special Fields

Please follow the following guidelines for these spec fields when creating Fortellis specs.

|Field|Description|
|--|--|
|`hostname`|Use `$[apiFortellisUrl]`.|
|`license`|Do not include this in the spec.|
|`tags`|You must include the tags object at the root level to pass the Fortellis linter to allow API Directory users to filter your spec by category.|
