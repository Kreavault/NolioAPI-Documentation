# API Authentication & Authorization
Nolio API uses the OAuth 2.0 protocol for authentication and authorization. Each application is assigned a `client_id` and `client_secret` and scopes that are authorized on the platform. Your application can then request authorization for a user. Once the user has granted authorization, the system will issue _short lived per user access and refresh tokens. The access token is used by your application to make requests until it expires and the refresh token is used to get an un updated access token.

![](https://developer.withings.com/oauth2/img/OauthDiagram.jpg)

There are many [OAuth2 client libraries](http://oauth.net/2/) available for you to use in your application.

**IMPORTANT**: The Nolio platform calls (OAuth and API) are resricted to _**HTTPS**_ requiring all all communication must be done over a secure SSL channel. HTTP requests made over a non-secure will return a HTTP 302 redirect to HTTPS.

## OAuth URLs

| System | Type | URL | Purpose |
| :-- | :-- | :- | -- |
| Production | Authorize | https://www.nolio.io/api/authorize/ | Being the authorize flow |
| | Token Exchange | https://www.nolio.io/api/token/ | Exchange a code for a token |
| | Deauthorize | https://www.nolio.io/api/deauthorize/ | Remove user authorizations |


## Authenticating Your Application on Behalf of a User
To access resources on behalf of a user, your application needs to get explicit permission from the user via a 3-legged authentication flow:

1. Make GET request to authorize endpoint with following parameters.  Each of the parameter values should be urlencoded.  The user should be redirected to the Nolio authentication flow.

    | Parameter | Value |
    | :-- | -- |
    | `response_type` | Must have the value `code` to obtain the authorization code |
    | `client_id` | Unique application identifier obtained from Nolio |
    | `redirect_uri` | URI where to redirect the user after permission is granted. It must match the callback you setup on Nolio API| 

Please note that 'scope' is not a parameter we ask for currently, all routes will be accessible by default.
 
``` http
For example, if your oauth information is below:  
client_id: my_client_identifier    
redirect_uri: https://myplatform.com/callback  

GET request to the authorize endpoint should look like:  
    
GET https://www.nolio.io/api/authorize/?response_type=code&client_id=my_client_identifier&redirect_uri=https%3A%2F%2Fmyplatform.com%2Fcallback 
```
    
2. If the user is not logged in, they will be asked to enter their username and password and upon successful login the workflow will continue.

3. The user will be prompted with a screen to approve data access to your application

4. Once scopes are granted, the request will be redirected back to the `redirect_uri` passed with the authorization `code` as a URL query parameter. For example: http://myplatform.com/?code=ABC123. The authorization code returned expires in 10 minutes or the next step will fail. **Important** the code passed to your application will be _url encoded_ and typically must be _url decoded_ before being passed in the next step.

5. Last step is for your application to exchange the authorization code for access and refresh tokens.
The application must make an HTTPS POST to the token URL passing the following `application/x-www-form-urlencoded` parameters:

    | Parameter | Value |
    | :-- | -- |
    | `grant_type` | must have the value `authorization_code` to exchange the token |
    | `redirect_uri` | URI must match the redirect_uri used in the initial step |
    | `code` | The authorization code previously returned |

You must as well add `Authorization` header as `Basic` and `client_id:client_secret` encoded as base64

Example: for client_id=toto, client_secret=tata

``` http
POST /api/token/ HTTP/1.1
Authorization: Basic dG90bzp0YXRh
Host: www.nolio.io
Content-Type: application/x-www-form-urlencoded
Content-Length: 112

grant_type=authorization_code&code=ABC123&redirect_uri=https%3A%2F%myplatform.com%3A8080%2Fcallback
```

`dG90bzp0YXRh = toto:tata encoded as base64`

6. The Nolio OAuth server will return a JSON object with the access token, refresh token, access token expiration in seconds, and the scope (`read write` always for now). The response will looks similar to the following:

    ```json
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    {
        "access_token" : "gAAAAMYien...",
        "token_type" : "bearer",
        "expires_in" : 86400,
        "refresh_token" : "i7ne!IAAA...",
        "scope": "read write"
    }
    ```

## Refreshing an Access Token
Access tokens are short lived and will expire after a brief period (24h currently). When the token is granted the `expires_in` value contains the number of seconds the token is valid from issue. Using an expired access token will result in the API call returning an HTTP 401 Unauthorized response. Once expired your application will need to make a request using the access token and refresh token to get a new valid token.

To refresh an access token make an HTTP POST call to the token endpoint passing the following `application/x-www-form-urlencoded` parameters:

| Parameter | Value |
| :-- | -- |
| `grant_type` | Grant type must have the value `refresh_token`. i.e `grant_type=refresh_token` |
| `refresh_token` | The refresh token received when the token was issued |

You must as well add `Authorization` header as `Basic` and `client_id:client_secret` encoded as base64 (same as of the first time to get a token)

Example: for client_id=toto, client_secret=tata

``` http
POST /api/token/ HTTP/1.1
Authorization: Basic dG90bzp0YXRh
Host: www.nolio.io
Content-Type: application/x-www-form-urlencoded
Content-Length: 56

grant_type=refresh_token&refresh_token=9OjmAE7BSj1Cqsv6u
```

`dG90bzp0YXRh = toto:tata encoded as base64`

Successful execution of this call is a HTTP 200 response containing the new access token in the same format as the previous token. A HTTP 400 response may be returned if the user revokes authorization for your application, which will require re-authentication of your application for the user using the previously described flow.  

### Deauthorization
If you need to deauthorize a user you can POST a request to the deauthorize endpoint. Include the authorization header with the bearer token as you would with any other authenticated api request. 