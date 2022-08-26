---
title: Implicit Flow for Authorization
author: Nathaniel Sandberg
last updated: 2/24/2022
---

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' implicit flow is not secure in the context of JavaScript apps and single page apps.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to retrieve tokens with the implicit flow.

### Things to Remember in This Tutorial

You should remember the following:

* Use tokens for 3600 seconds with no refresh tokens.
* Use the `response_type` `token`.
* Include the `nonce` parameter in the request.
* Enter your `redirect_uri` for the **Callback**
    when you are registering your app in your Developer Network Account to prevent other applications from getting tokens on your behalf.
* Only use the implicit flow to control access between a Single-Page Application (SPA) and a resource server.

Your users must register for Fortellis to sign in and get a token to use with your app with this authorization flow.
See [Getting Started](/docs/general/overview/getting-started) for more information on signing up for Fortellis.

> We provide an example application on how to implement the implicit flow [here](https://github.com/Fortellis/ImplictFlow-ExampleApp).

## Retrieving Tokens with the Implicit Flow

1. When you need a user to access your application with the implicit flow,
    redirect the user to the Fortellis authorization server URL `$[authServerUrl]/v1/authorize`.  
    To identify the application to the server,
    you also need to supply the following values to associate the user with your app:
    * `client_id`: Use your API Key for the `client_id`.  
    * `response_type`: Use `token` to get the token back in the response.  
    * `scope`: Use `openid` in the scope.
    * `redirect_uri`: Use the redirect URI that you entered when you were registering your app with a trailing `/`.  
    * `state`: Use a unique identifier for the session that the authorization server returns to prevent cross-site scripting and return your user to the current state of your
        app.  
    * `nonce`: Use `nonce` the ID token to mitigate replay attacks.  
    **Example:**  

    ```URL
    $[authServerUrl]/v1/authorize?client_id=RRTKerBb7tgRT4K6ES2eQdYJ1z9cHFsv&response_type=token&scope=openid&redirect_uri=https://example.com/&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7&nonce=true
    ```

    **Note:** Keep in mind the following when implementing this flow:
    * If the user is signed in,
        the page redirects to the callback URL with the token in the hash fragment.
    * If the user is not signed in,
        the page will ask the user to authorize.
    * Users sign in through Fortellis
        if you choose to use this OAuth flow.  
1. Fortellis returns the user to your application with the token in the URL.  
    **Note:** You can use the token for a maximum of 3600 seconds.  

    ```bash
    https://example.com/#access_token=eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULkxNS1M1czNabmI3RVMwMTBZOFV3bmpJN3N1c0oxM1o0eHpCSGxQajhnUUEiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3IiwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlhdCI6MTU2ODkyNTc3MCwiZXhwIjoxNTY4OTI5MzcwLCJjaWQiOiJSUlRLZXJCYjd0Z1JUNEs2RVMyZVFkWUoxejljSEZzdiIsInVpZCI6IjAwdTNud2pyMmxDdWNXMUVuMnA3Iiwic2NwIjpbIm9wZW5pZCJdLCJzdWIiOiJuYXRoYW4uc2FuZGJlcmdAY2RrLmNvbSJ9.hAI12THE2nxNVTUj3YgPxblJR0aSZFMbL2M4RJvjY5w5ezsI_fR1vcmbsJXC6liuW1EdO2bqbghvTMgL8Nmb87ypFRJBox7Too3dY-owOXexIzZm24JTa0UQSzBFWM2_1gqFbOe7WQZK8-S4w6BrxFBSB8NYQkUSqtsC8m3KbzxfGYhkXADJFTtpuOFQPSYyAFH7xcFSjRIgqxAsNpxEv-nNu2H0vA9VqAxivvClcMTz4dDYWx7myvdkhVJXXwEfArzrv8KqTzfUl4818TrtfHRdylzZ8AKo0TTgTPqICIPKpV05O77w_0tv4ApFVInxM1hBMbZlHZlmrHpKb5BBIg&token_type=Bearer&expires_in=3600&scope=openid&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7
    ```

    **Note:** Your app does the following:
    * Your app extracts the tokens from the URI.
    * Your app uses these tokens to call the resource server for the user.

1. Use the following in the header when calling Fortellis APIs on behalf of the app user:
    * The `token` value
    * The `Subscription-Id`
    * The `Request-Id`

1. Your application navigates the browser to the Fortellis [Sign In]($[accountsServerUrl]/login?key=rHs320VhmrMz1mBRLKtasgSxdDLDJ4cAs26Ue2AlwnU) page to authorize the user.
1. Fortellis redirects the browser back to your redirect URI with access and ID tokens as a hash fragment in the URI.
1. Your app extracts the tokens from the URI.
1. Your app uses these tokens to call the resource server for the user.

## Next Steps

In this tutorial, you should have learned how to retrieve tokens with the implicit flow.
You can use this flow to get tokens and then get a `Subscription-Id` for your users to start making requests to APIs in Fortellis.
