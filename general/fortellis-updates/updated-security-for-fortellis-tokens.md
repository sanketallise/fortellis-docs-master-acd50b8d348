---
title: API Updates
author: Nathaniel Sandberg
last updated: 7/29/2022
---

## Updates

Fortellis is updating the following for your APIs:  

* [Security Enhancements for Fortellis Tokens](#security-enhancements-for-fortellis-tokens)
* [Admin API Payload Updates](#admin-api-payload-updates)

You can read below to find out how to update your APIs to get enhanced security and prevent service interruptions.

## Security Enhancements for Fortellis Tokens

API Developers that use Fortellis tokens can now use enhanced security and consistent authorization steps when they authorize API calls and Admin API calls.

### Business API Authorization

Currently, API Developers do the following for business APIs:

* Verify the token signature using the Fortellis JSON Web Key Set.
* Verify the issuer of the token.  
* Verify the audience: `api_providers`.  
* Verify that the token is not expired.

### Admin API Authorization

Currently, API Developers do the following for Admin APIs:

* Verify the token signature using the Fortellis JSON Web Key Set.
* Verify the issuer of the token.  
* Verify the audience: `fortellis`.  
* Verify that the token is not expired.

### Updates for Business API and Admin API Authorization

Now, Fortellis has added another layer of security to the tokens.
API Developers should continue to verify the token,
but they should also verify the following:  

* The subject (`sub`) of the token is the **Client ID** from the API.
* The audience (`aud`) is `api_providers`.

API Developers get the following benefits:

* They know that the token comes from Fortellis
    because only Fortellis can issue tokens with the API's **Client ID**.
* They know that they aren't getting requests from third parties that may get tokens from Fortellis.
* They know that no one else can send requests to their APIs without the correct token.

**Note:** You can add your client ID to your whitelist of client IDs anytime if you currently verify client IDs in the token.

You can see the examples of how to verify the subject is the **Client ID** in the following:

* [Creating APIs in Node.js](/docs/tutorials/api-toolbox/creating-apis-in-node)
* [Creating APIs in Python](/docs/tutorials/api-toolbox/creating-apis-in-python)

With this enhanced functionality, API Developers can protect their APIs from anyone outside Fortellis and get the most out of Fortellis security.

### Questions

#### What Should I Change?

You should start validating the subject or `sub` of the token, which should match the **Client ID** from the API. If you have an Admin API, you should also start validating that the `aud` of the token is `api_providers` and not `fortellis`.

You should already be validating the following:  

* Signature
* Issuer
* Audience

**Note:** If you have multiple APIs registered in your account, each API has a unique **Client ID**.

#### When Should I Make This Change?

##### If You Don't Enforce Subject Validation

If you currently only verify the token signature, issuer, and audience for token validation, you can start enforcing subject `sub` validation after September 27, 2022.
Your Admin API also receives `aud` of `api_providers` after this date.

##### If You Enforce Subject Validation

If you are validating the token **Client ID** against a whitelist, you must add your API **Client ID** to your whitelist as soon as possible.
Your business and Admin API will start receiving tokens with the `sub` of your **Client ID** and the `aud` of `api_providers` starting on September 27, 2022.
Fortellis sends tokens with only your API **Client ID** in the subject `sub` and in the clientId `cid`.
You can use either of these claims for validation,
but we recommend using the subject `sub`.

#### Will This Impact Existing Traffic?

##### Yes, It Will

If you are verifying the **Client ID** against a whitelist,
Fortellis only sends your **Client ID** to you after this change.
Update your security to verify your **Client ID** in the subject of the token.

##### No, It Won't

If you are currently only verifying the `issuer`, `audience`, and `signature` in the token, you should also start validating the subject `sub` after September 27, 2022, for additional security.

#### Who Should I Contact for Questions?

Please reach out to [Fortellis Operations](mailto:FortellisOpsAPIs@cdk.com) for any questions.

## Admin API Payload Updates

You can now also benefit from an updated payload in the Admin API that more accurately reflects the domain.
We have updated the payload to show the `organizationInfo` and the `appInfo`.
We are also now removing the `solutionInfo` and the `organizationInfo`.
With this update, you can see a payload that more accurately reflects the current Fortellis domain.

```json
{
  "organizationInfo": {
    "id": "2529a384-aee3-4b9a-af35-ed08e77dee15",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "appInfo": {
    "id": "f83eaff0-3f88-4ebd-9fc8-25f051632968",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "2059d5f5-bccb-40c8-937e-f217311feabd"
  },
  "userInfo": {
    "fortellisId": "user@example.com"
  },
  "apiInfo": {
    "id": "v1-appointments",
    "name": "Appointments",
    "implementationName": "Fortellis Spec 8"
  },
  "subscriptionId": "2529a384-acc3-4b6a-a835-ed09e77dee15",
  "connectionId": "2529a384-add3-4b6a-a935-ed09e45def12"
}
```

You no longer receive the following objects:  

* `entityInfo`
* `solutionInfo`

### Questions

#### What Is The Timeline?

You should make the change before the start date of September 27, 2022.
We currently send all of the fields.
After September 27, 2022, we will not send the following:  

* `entityInfo`: Change this to `organizationInfo` if you are currently using it.
* `solutionInfo`: Change this to `appInfo` if you are currently using it.

#### Will This Impact Existing Traffic?

No, you should not see any impact on existing business APIs. However, you may not be able to accept new connections if you don't make the change before the start date.

#### What You May Need to Update?

You may need to update the following:  

* Your schema if you are validating the Admin API payload structure.
* Your code referencing the following attributes in Admin API payload:  
    * `entityInfo`
    * `solutionInfo`

#### Who Should I Contact for Questions?

Please reach out to  [Fortellis Operations](mailto:FortellisOpsAPIs@cdk.com) for any questions.
