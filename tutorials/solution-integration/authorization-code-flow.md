---
title: Authorization Code Flow
author: Nathaniel Sandberg
last updated: 2/24/2022
---

> **Warning:**
> You must get your user's `Subscription-Id` and send it and the token in the header for the API to respond.
> We recommend having your users log in using your authorization.
> You can then retrieve your user's `Subscription-Id` with your backend and retrieve the token with your credentials using the client credentials flow.
> Fortellis' authorization code flow is not secure in the context of JavaScript apps and single pages apps.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Get the authorization code.
* Use the code to get tokens to access the API.  
    **Note:** Fortellis only redirects users back to the app that App Developers enter in the `redirect_uri` for the **Callback** when you are registering the app.  
* Refresh tokens to get new tokens without having users reenter their credentials.  

### Things to Remember in This Tutorial

You should remember the following:

* Your users must register for Fortellis to sign in and get a token to use with your app with this authorization flow.  
    See [Getting Started](/docs/general/overview/getting-started) for more information on signing up for Fortellis.
* Only use the Authorization Code flow if you have a server-side web application that can securely store your `client_id`.

## Retrieving a Token with the Authorization Code Flow

1. When you need users to access your application with their authorization credentials,
    redirect the user to the Fortellis identity server URL `$[authServerUrl]/v1/authorize`.  
    You also need to supply the following values to associate the user with your app:  
    * `client_id`: Use your API Key for the `client_id`.  
    * `response_type`: Use `code` to get the code back in the response.  
    * `scope`: Use `openid` in the scope.  
    * `redirect_uri`: Use the redirect URI that you entered when you were registering your app with a trailing `/`.  
    * `state`: Use a unique identifier for the session that the authorization server returns to prevent cross-site scripting and return your user to the current state of your
        app.  
    **Example:**  

    ```URL
    $[authServerUrl]/v1/authorize?client_id=RRTKerBb7tgRT4K6ES2eQdYJ1z9cHFsv&response_type=code&scope=openid&redirect_uri=https://example.com/&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7
    ```

    **Note:** Keep in mind the following when implementing this flow:
    * If the user is logged in, the page redirects to the callback URL with the `code` in the URL.
    * If the user is not logged in, the page will ask the user to authorize.
    * Users sign in through the Fortellis Platform login screen if you choose to use this OAuth flow.  
1. Fortellis returns the user to your application with the authorization code in the URL.  
    **Note:** You can exchange the authorization code for a token for 60 seconds.  
    **Example:**

    ```json
    https://yourRedirectURL/code=yJKlFgWW8OGv8a1Ei3BH&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7
    ```

1. Use the authorization `code` to get a token from the authorization server using a `POST` request with the following values:  
    * URL
        * `$[authServerUrl]/v1/token`
    * Headers
        * `Authorization`  
        **Note:** To create your basic authorization, [Base64 encode](https://www.base64encode.org/) your `API Secret:API Key` for that app.  
        * `accept`: `application/json`
        * `content-type`: `application/x-www-form-urlencoded`
    * Query Parameters  
        * `grant_type`: `authorization_code`  
        * `redirect_uri`: {YOUR_REDIRECT_URI}  
        **Note:** You must enter the exact same redirect URI
        that you used in the `redirect_uri` with the trailing `/`
        when you got the authorization code.  
        * `code`: `{your authorization code}`  

    **Example:**

    ```bash
    curl --request POST \
    --url $[authServerUrl]/v1/token \
    --header 'accept: application/json' \
    --header 'authorization: Basic MG9hY...' \
    --header 'content-type: application/x-www-form-urlencoded' \
    --data 'grant_type=authorization_code&redirect_uri=https://example.com/&code=P59yPm1_X1gxtdEOEZjn'
    ```

    **Example Token:**

    ```json
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULkhhSm13bklsQ3QzVDJibDVBSDhrVUNlQ2tzMjZ5U1V2eEJsb0JvWkZFNUEiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3IiwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlhdCI6MTU2ODkwNDY3OSwiZXhwIjoxNTY4OTA4Mjc5LCJjaWQiOiJSUlRLZXJCYjd0Z1JUNEs2RVMyZVFkWUoxejljSEZzdiIsInVpZCI6IjAwdTNud2pyMmxDdWNXMUVuMnA3Iiwic2NwIjpbIm9wZW5pZCJdLCJzdWIiOiJuYXRoYW4uc2FuZGJlcmdAY2RrLmNvbSJ9.FgwkizyN1AMJsUvU5hsnQtSSLHs1pBV8cVUVI4tfknNJdelR-oXOQXsCI4xNpAgOWzn07UNxZwu-4wPtknzxPZ8a3D-xEhoIV_Imp6xUU3x5pLkxJS5b9re0eoHwA8eyp0aitI5lC8-iIjKJAhrNt8HOR4rteCFDxbu38OTL5tRrfPxBc6w4TCzg-LgRJ1dWvg7RCHgsuNyBJOucQSyjWqQKsqR68GDc3DAOQCCR6qpKtsIfwnem1ELy2KnNTLyyQTTQI7ihtm7aXUWgA8haL3GifOmBzJKgnoQQJ6hRSC4Th7DtBwemoiWKVobpXbD6g1DER6Z64piuClfF8MIjxA",
        "scope": "openid",
        "id_token": "eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ.eyJzdWIiOiIwMHUzbndqcjJsQ3VjVzFFbjJwNyIsInZlciI6MSwiaXNzIjoiaHR0cHM6Ly9pZGVudGl0eS1kZXYuZm9ydGVsbGlzLmlvL29hdXRoMi9hdXMxbmk1aTluOVdremNZYTJwNyIsImF1ZCI6IlJSVEtlckJiN3RnUlQ0SzZFUzJlUWRZSjF6OWNIRnN2IiwiaWF0IjoxNTY4OTA0Njc5LCJleHAiOjE1Njg5MDgyNzksImp0aSI6IklELjdlN0ZVSWpfNWR2akxsamh5WnZIeWk0djFxYkN6dTdyejVkaWdlLS1XaE0iLCJhbXIiOlsicHdkIl0sImlkcCI6IjAwbzFhbjQ4cTFTdTFSQ0ZoMnA3IiwiYXV0aF90aW1lIjoxNTY4OTAzMDQ0LCJhdF9oYXNoIjoiaE5ab3lkUUdNalZ2YXNhMXlxZXF5USJ9.VRhom11-GAQPKQ_r1dXfieq3d_9mch_GfsDKiYvXtpyxGXw-uDmm1bm-nBhHVKh2Ule74YDH5LFXzeQopP-_Rof5dCBVATlOyHENo_LID1c6s2hhRKaj3KaI1HTn9FmK2KMl2xCuC9LqQ59-WnkizCVe2MFmo74qz9RHjuztdoRixo5v4lipjZf-qY5nAf3nk6ISKpF9UiB23BNitxL5-8p37DJRsIETkJEkX4BTixetZzaMzBxnuE1GoRPRsr2FHm5EA2C_Co4qEYB_CxdEbcgkDWBH-H658InKWNFYfu_aval8T1JdqINFVdYuEPnOVCXcqLRyZWUQ6Kc6wjlbnQ"
    }
    ```

1. Use the following HTTP headers when calling Fortellis APIs on behalf of the app user:
    * The `token` value
    * The `Subscription-Id`
    * The `Request-Id`

> Read more about [OAuth 2.0](https://oauth.net/2/).

## Refresh Tokens

Remember the following when implementing this authorization flow:

* Generate the refresh token on the initial call.
* The refresh token allows you to request a new token before the old token expires after an hour.
* Users must sign in to Fortellis a second time if they don't get a refresh token.

### Using Refresh Tokens

1. To get a refresh token, follow the authorization code flow
    and set the scope to `offline_access` and any other scopes
    you need later.  
    **Note:** When you refresh a token, you cannot include a scope that is not in the original call to get the access code.  
    **Authorization Code URL Example**

    ```bash
    $[authServerUrl]/v1/authorize?client_id=RRTKerBb7tgRT4K6ES2eQdYJ1z9cHFsv&response_type=code&scope=offline_access&redirect_uri=https://example.com/&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7
    ```

1. Get the new code in the URL.  
    **Example Redirect URL:**

    ```bash
    https://example.com/?code=YqCyB3IS9xCAYJSCsd6F&state=f8f5df39-cac3-41ea-a56c-6f6cf18f1be7
    ```

1. Call the token endpoint to get the new token.  
    **Example Curl:**

    ```bash
    curl --verbose -X POST "$[authServerUrl]/v1/token?grant_type=authorization_code&redirect_uri=https://example.com/&code=YqCyB3IS9xCAYJSCsd6F" -H "Authorization: Basic Ul...I3WA==" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json"
    ```

1. You receive the token with a `refresh_token` key.  
    **Example Token:**

    ```bash
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULk8zVmJWUmNHdnBHMjdtcWphWnBFX2ZNTWFOWXBTLXEyNjVqT3hReWs3SmcuUmlReDR2c1ZvVjBUTUhOMlJ2NlJSblI5M21EdXp0VmhzWVRPcy9FSE1lZz0iLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3IiwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlhdCI6MTU2ODk5MDE3NiwiZXhwIjoxNTY4OTkzNzc2LCJjaWQiOiJSUlRLZXJCYjd0Z1JUNEs2RVMyZVFkWUoxejljSEZzdiIsInVpZCI6IjAwdTNud2pyMmxDdWNXMUVuMnA3Iiwic2NwIjpbIm9mZmxpbmVfYWNjZXNzIl0sInN1YiI6Im5hdGhhbi5zYW5kYmVyZ0BjZGsuY29tIn0.C7V5usu4m58t6ez1kYq-61GBM0P2_iS2kSsq6HwvgIndACAVRcn4EKrkXIT1ZnRrWrCqaqX6WqZ7GHdh__ay6ACHUXHBK6RnAdbRcBFcZpKlRouP69s5TupPwuobj4QMB9JokJkUmcgNsbOv6_RFBvfjSOHprLGbfCKNPALvI5OXI0bOzBx3Z_-Ljpa_Z3KlX9TJLSW0X-M-RZFnrTdRAau71wIRUkZAZVTWBFLApAY2KM94b1lsQtgU9AzT4oqe0doh3gfCf6pSN7NVMHLG5LE1KBV8KUNx5sBtJNNKUQKJEFDlgJxlL3rwU3XBNIG7EH1Trd3aESryqpO6KKsOzA",
        "scope": "offline_access",
        "refresh_token": "iztTKKkT5L7V8DeJhiGIAtvR4jVKlHBt1pHCek69o7M"
    }
    ```

1. Change the `grant_type` to `refresh_token` to get a new token
    that you can continue to use without having the user redirect to sign in again.  
    **Example Refresh Request:**

    ```bash
    curl --verbose -X POST "$[authServerUrl]/v1/token?grant_type=refresh_token&redirect_uri=https://example.com/&code=YqCyB3IS9xCAYJSCsd6F&scope=offline_access&refresh_token=iztTKKkT5L7V8DeJhiGIAtvR4jVKlHBt1pHCek69o7M" -H "Authorization: Basic Ul...I3WA==" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json"
    ```

1. Use the new token you receive with the same `refresh_token`.  
    **Example Refreshed Token:**

    ```bash
    {
        "token_type": "Bearer",
        "expires_in": 3600,
        "access_token": "eyJraWQiOiJ6Z3cyUV9jRDJ2N0tCVENINUgzM3hQeXNUN2pLRVEtR3pQdFNLUi1vRXZRIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULnUzaDV1bjRVc2lrRWZWNFViX1JfQlJ5QmUybGVXMU5jZEtUNkFYNHRXWk0uUmlReDR2c1ZvVjBUTUhOMlJ2NlJSblI5M21EdXp0VmhzWVRPcy9FSE1lZz0iLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3IiwiYXVkIjoiYXBpX3Byb3ZpZGVycyIsImlhdCI6MTU2ODk5MTU0MiwiZXhwIjoxNTY4OTk1MTQyLCJjaWQiOiJSUlRLZXJCYjd0Z1JUNEs2RVMyZVFkWUoxejljSEZzdiIsInVpZCI6IjAwdTNud2pyMmxDdWNXMUVuMnA3Iiwic2NwIjpbIm9mZmxpbmVfYWNjZXNzIl0sInN1YiI6Im5hdGhhbi5zYW5kYmVyZ0BjZGsuY29tIn0.C4H9cxUHFWzTDPopBgxmXGdS5jidpYZ0DKr4F2KGmcDikGZA6_68lQ7twF_rJa8JrFqjLKa2KnC2lo-IdYtWWnjU5hLY-aARu3FJZdgspYqNKVZ5SkQRwKE483bZOyVyAVJGpDWkZy-GdN3XQDV5QN0fzh_ql22ipJ3Vr0hiVpxyB2nl8I_DdGWvlkSVZ1M7gy3krkE1IIu3IoFEH2D2hzFdQmdbl2DXqy7xjqAO499Cm8yyzTHBTIAUh35x1Q94DdW3hOggdtrX2_97AWTlEz5GI3PT4VbtDeIlVVk8SzzPH166WUHQXwoleDBb3V16UzpgYJSvRujVCg2y5DzZeg",
        "scope": "offline_access",
        "refresh_token": "iztTKKkT5L7V8DeJhiGIAtvR4jVKlHBt1pHCek69o7M"
    }
    ```

## Next Steps

In this tutorial, you should have learned how to do the following:

* Get the authorization code.
* Use the code to get tokens to access the API.  
* Refresh tokens to get new tokens without having users reenter their credentials.  

You can now use a server to store your `client_id` to retrieve a token and to get a `Subscription-Id` for your users.
