# Nolio Public API

## Intro

Nolio API was created to allow authenticated external third parties access or push data in Nolio. To use the API you will need the application credentials available on Nolio API Portal (requests can be made at www.nolio.io/api).

All requests to the API require a valid OAuth token, which identifies the application and the user account that the application is accessing data on behalf of.

* [OAuth Documentation](https://github.com/NolioApp/NolioAPI-Documentation/wiki/OAuth-2)
* [API Documentation](https://github.com/NolioApp/NolioAPI-Documentation/wiki/API-Routes)
* [Nolio Media Kit](https://www.nolio.io/media_kit/)

A valid access token is requires for every API request. Requests must be made over an HTTPS connection and include an Authorization header of type Bearer with your access token value.

## API calls

A valid access token is requires for every API request. Requests must be made over an HTTPS connection and include an Authorization header of type Bearer with your access token value.

```
POST /api/upload/file/ HTTP/1.1
Host: www.nolio.io
Content-Type: application/json
Authorization: Bearer xBBBBvkia...
Accept: application/json
```

**Note the space between Bearer and the token.**

API requests are made using JSON data passing a `Content-Type` of `application/json` and `Accept header` of `application/json`. By default all API requests will return JSON data.

## Production system

When access to Nolio API is granted, you will be able to directly call our production server with your development app. 
Ask for more informations by [contacting us](https://github.com/NolioApp/NolioAPI-Documentation/wiki/ContactUs)

## Rate limits

Nolio API usage is limited to avoid too many calls on our servers.

There is an hour limit and a daily limit.

### Development app (and new apps) :

* 200 requests per hour
* 2000 requests per day

### Production app

* 500 requests per hour + 20 * <number_of_users_synced_with_your_app>

* 5000 requests per day + 100 * <number_of_users_synced_with_your_app>

Production application is disabled by default, you need to contact us when you want to enable it after testing with development apps.

Note that requests violating the short term limit will still count toward the long term limit.

Please contact us if you think the rate limits do not suits your usage, we can increase them.

### Retry logic

You must implements your own logic if a request failed for any reason (429 too many requests, …)


# API response status code

The API will respond to requests using HTTP status codes. Below are some common status codes your application may receive:

* 2XX - Status codes in the 200 range denote success.
  * A status of 200 (OK) is typical, but the documentation for each endpoint may specify other successful status codes in this range.
* 302 - Object moved.
  * Typically returned by the API if your request is made over an insecure HTTP connection.
* 4XX - Status codes in the 400 range denote client errors. Common responses include:
* 400 - Bad request - bad or missing data was passed to the endpoint.
* 401 - Unauthorized - bad, expired or missing authorization header.
* 403 - Forbidden - wrong token or other non allowed access
* 405 - Method not allowed
* 429 - Too Many Requests - Will trigger if you do more request than your rate limit, please read [Rate limits](https://github.com/NolioApp/NolioAPI-Documentation/wiki/Home/_edit#rate-limits)

* 500 - Server Error.
  * If a status code 500 is returned there was an error on the server processing your request. Server errors are recorded and the servers are monitored for errors.
* 502 - Bad gateway - Can happen if a severe crash on our end, you should retry your request later.
* 503 - Service Unavailable.
  * If you receive a status code of 503 our system is temporarily unavailable, most likely for system maintenance. You should retry your request later.

For more information see the [HTTP RFC for HTTP status codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
