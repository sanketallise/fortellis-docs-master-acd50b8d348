---
title: Fortellis Error Codes
author: Nathaniel Sandberg
last updated: 8/9/2022
---

To help you resolve errors, Fortellis sends error codes to indicate what went wrong with an API and if you can do anything to fix the issue.
With these error codes, you can assess the root cause of the issue and fix it if possible.
Fortellis sends the following errors:

|Error codes|Error Details|Fault|Action Required|
|--|--|--|--|
|||||
|API-DOES-NOT-EXIST|The app cannot access this endpoint because the endpoint doesn't exist.|App Developers|App Developers should correct the endpoint.|
|UNAUTHORIZED-API-ACCESS|The App Developers has not requested access to this API.|App Developer|App Developers should add the API to their app. See [Updating the App](/docs/tutorials/app-lifecycle/managing-apps/#updating-the-app) for more details. |
|TOKEN-EXPIRED|The app's token has expired.|App Developers|App Developers should request a new token from the authorization server every two hours.|
|TOKEN-ISSUER-MISMATCH|The App Developer retrieved a token from an authorization server that is not $[authServerUrl].|App Developers|App Developers should use the Fortellis Authorization Server, $[authServerUrl], to get tokens.|
|TOKEN-AUDIENCE-MISMATCH|The App Developer retrieved a token with the wrong audience.|App Developers|App Developers should request a new token from the Fortellis Authorization Server, $[authServerUrl].|
|INVALID-TOKEN|The App Developer has a JSON Web Token that isn't valid.|App Developers|App Developers should request a new token from the Fortellis Authorization Server, $[authServerUrl].|
|QUOTA-EXCEEDED|The app has made the maximum number of calls to the API allowed.|App Developers|App Developers should contact Fortellis to increase their quota.|
|MISSING-AUTHORIZATION|The App Developer didn't include the authorization header in the request to the API.|App Developers|App Developers must include the authorization header in API requests.|
|MISSING-SUBSCRIPTION-ID|The App Developer didn't include the `Subscription-Id` header.|App Developers|App Developers must include the `Subscription-Id` for the subscription to the app, and API Developers must approve the subscription to the API.|
|API-SUBSCRIPTION-INACTIVE|API Developers must approve each subscription before the subscription can be active.|API Developers|API Developers must approve the activation request through the Admin API or manually through the Developer Account.|
|SUBSCRIPTION-MISMATCH|The App Developer provides a `Subscription-Id` that's not associated with the API.|App Developers|App Developers must use a `Subscription-Id` that has subscribed to the API. App Developers must also use the API Key of the app for the implicit authorization flow and the authorization code flow or the API Key and API Secret of the app for the client credentials flow.|
|SUBSCRIBER-CONFIGURATION-PENDING|The API Developer has configured the API for **Subscription-specific values**, but has not set those values for the subscription.|API Developers|The API Developer must configure the **Subscription-specific Values** for the subscription.|
|API-CONFIGURATION-ERROR|The API Developer has not configured the custom authorization and must update it.|API Developers|API Developers have enabled their custom authorization, but Fortellis cannot get the credentials because the configuration is wrong.|
|ROUTING-ERROR|Fortellis cannot determine the target system to process the request.|Fortellis|Fortellis Support must correct a subscription with active targets.|
|PROCESSING-ERROR|Fortellis cannot process your request.|Fortellis|Fortellis encountered an exception and cannot process the request. Contact Fortellis Support.|
|TARGET-SYSTEM-UNREACHABLE|The API server is not accepting requests at this time.|API Developer|API Developer must fix the implementation, or the target server has availability issues.|
|TARGET-SYSTEM-TIMED-OUT|The API server timed out before it could complete the request.|API Developers.|API Developers must look into the error.|
|TARGET-RESPONSE|The API server passed an error through Fortellis to the app.|API Developers|API Developers must fix the error in the API server.|
|PAYLOAD-TOO-BIG|Fortellis cannot send a request or response of that size.|API Developer|Wit the assistance of Fortellis Support, API Developers can update the API request and response to use streaming on the API proxy.|
