---
title: Verifying Fortellis JSON Web Tokens
author: Nathaniel Sandberg
last updated: 2/8/2022
---

To protect your API, you can verify calls to your API using Fortellis OAuth 2.0 JSON Web Tokens (JWTs)
if you do not provide your own authorization method for your APIs.
Fortellis can send one of two JWTs to your endpoint:

* One from a call that Fortellis makes to the Admin API URL at the `/activate` and `/deactivate` endpoints.  
    **Note:** API Developers only receive this token
    if they implement the Admin API.  
* One from a call sent from an app through Fortellis to the API endpoint.  
    **Note:** API Developers must verify the token in every call to the API endpoint to ensure
    they are receiving a request from Fortellis.

Fortellis issues tokens to simplify and share the authorization process with API Developers.

Tokens verify the following:

* The public key matches the secret hashed key
* The key comes from a trusted source

## Validating a Token

1. Find a library in the language of your choice for validating tokens.
1. Validate that the tokens have the following information checked against the Fortellis identity server:

    * Audience: The audience the token was issued for
    * Issued at Time: The time that the token was issued  
    * Expiration: The time that the token expires  
    * Issuer: The issuer of the token  
        **Note:** Remember the following:  
        * Do not hard-code the Key Identity (kid)
        because we change the kid at least once every 90 days and randomly as needed.
        * Your library will probably get your keys by appending the `/v1/keys` endpoint to the issuer URL.

For more information on implementing the library, please refer to the documentation on that library.
For more information on what you must verify in the token, please see below.

## Token Information

The tokens
that Fortellis sends to the following endpoints have different values:

You can go to one of the following sites to find libraries
that will assist you in validating and verifying tokens.

* [Okta JWT Verifier](https://www.npmjs.com/package/@okta/jwt-verifier)
* [https://jwt.io](https://jwt.io)

Remember the following:

* You will only receive the token at the `/activate` and `/deactivate` endpoints if you have implemented the Admin API.
* You then follow the steps in [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) to get a token to make a call to the `/connections/:connectionId/callback` endpoint.

> Use the `api_providers` audience in your token to develop and implement your Admin API.

Make sure to verify the issuer of the token and the audience for the token at each of these endpoints.

### Example JWT

```text
eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULmhlZEJjYlo3Ym9UM1RBZjctcEJVMXhSVGQtWkRveWdnb2JXbE9rY1drbzgiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3IiwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlhdCI6MTU2NzExMjgzNiwiZXhwIjoxNTY3MTE2NDM2LCJjaWQiOiJObHRjdXdJdFpWdmI3VEZ6RzV1QkNubEdZWDFyUzRuUyIsInNjcCI6WyJhbm9ueW1vdXMiXSwic3ViIjoiTmx0Y3V3SXRaVnZiN1RGekc1dUJDbmxHWVgxclM0blMifQ.lLURAZh9XqKVXVNhCxPn2gV_KCfHKbphvCB_ZGIN9MJKpTVDKJ2l-vYbXdMVZLkA0ODIb4E3tV1GhkZXeIdyIVW3xM1ofQJ_1i_tMlfSeRumsnxqgXfmJOGkLtN4DNOmARsD3cHJJD_EIe_VHC1rWdGLrcQTmnyI4-h36A7b4XEfHLUXfsuVjSafvxZwPG5118lfkgNb4SHAnKMm0DTxHkYhen7saTDo2IBx1Nbcw3VtLpanJN-J8DcJfFHJLZkalJ1RbHeDamTUM4FMfASeok8MJMbCE5DzfpILHqkGMl6HWpFGmGOUnKYfS6A2wZxsbqqQ9k7kKlWLiZRBydYp9w
```

To decode a token manually, do the following:

1. Get a token.
1. Go to [https://jwt.io](https://jwt.io/).
1. Enter your token in the encoded token field.  

You will want to complete these operations programmatically in your application. Use the links above to find a library that works with your codebase.

#### Example Header

**Value:**  

```text
eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ
```

**Decoded:**  

```json
{
  "kid": "zgw2Q_cD2v7KBTCH5H33xPysT7jKEQ-GzPtSKR-oEvQ",
  "alg": "RS256"
}
```

The header provides the following information:

* Key ID (kid): What is the key ID of the token
* Algorithm (alg): What algorithm was used to hash the token

**Note:** Fortellis uses the RS256 algorithm.

#### Example Body

**Value:**  

```json
`eyJ2ZXIiOjEsImp0aSI6IkFULmhlZEJjYlo3Ym9UM1RBZjctcEJVMXhSVGQtWkRveWdnb2JXbE9rY1drbzgiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3IiwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlhdCI6MTU2NzExMjgzNiwiZXhwIjoxNTY3MTE2NDM2LCJjaWQiOiJObHRjdXdJdFpWdmI3VEZ6RzV1QkNubEdZWDFyUzRuUyIsInNjcCI6WyJhbm9ueW1vdXMiXSwic3ViIjoiTmx0Y3V3SXRaVnZiN1RGekc1dUJDbmxHWVgxclM0blMifQ
```

The body provides the following information:  

* Version (ver): What is the version of JWT
* JWT ID (jti): What the JWT ID is
* Issuer (iss): Who issued the token
* Audience (aud): Who should use the token to verify the user
* Issued at (iat): When the token can start to be used
* Expires (exp): When the token expires
* Client ID (cid): Who the token was issued to
* Scope (scp): What is the scope of the token
* Subject (sub): What is the subject of the token  
  **Note:** Verify that the subject in the token is `{yourClientID}` in the following:
    * Calls to your API.
    * Calls to your Admin API.

**Decoded:**  

```json
{
  "ver": 1,
  "jti": "AT.hedBcbZ7boT3TAf7-pBU1xRTd-ZDoyggobWlOkcWko8",
  "iss": "$[authServerUrl]",
  "aud": "api_providers",
  "iat": 1567112836,
  "exp": 1567116436,
  "cid": "NltcuwItZVvb7TFzG5uBCnlGYX1rS4nS",
  "scp": [
    "anonymous"
  ],
  "sub": "NltcuwItZVvb7TFzG5uBCnlGYX1rS4nS"
}
```

### Ending

**Value:**  

```text
lLURAZh9XqKVXVNhCxPn2gV_KCfHKbphvCB_ZGIN9MJKpTVDKJ2l-vYbXdMVZLkA0ODIb4E3tV1GhkZXeIdyIVW3xM1ofQJ_1i_tMlfSeRumsnxqgXfmJOGkLtN4DNOmARsD3cHJJD_EIe_VHC1rWdGLrcQTmnyI4-h36A7b4XEfHLUXfsuVjSafvxZwPG5118lfkgNb4SHAnKMm0DTxHkYhen7saTDo2IBx1Nbcw3VtLpanJN-J8DcJfFHJLZkalJ1RbHeDamTUM4FMfASeok8MJMbCE5DzfpILHqkGMl6HWpFGmGOUnKYfS6A2wZxsbqqQ9k7kKlWLiZRBydYp9w
```

The library of your choosing verifies the signature of the token.
