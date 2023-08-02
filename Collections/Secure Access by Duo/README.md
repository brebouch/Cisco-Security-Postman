# Duo

#Admin API

The Duo Admin API provides programmatic access to the administrative functionality of Duo Security's two-factor authentication platform.

## Overview

The Admin API lets developers integrate with Duo Security's platform at a low level. The API has methods for creating, retrieving, updating, and deleting the core objects in Duo's system: [users](), [phones](), [hardware tokens](), [admins](), and [integrations]().

Developers can write applications that programmatically read their Duo account's [authentication logs](), [administrator logs](), and [telephony logs](); read or update account [settings](); and retrieve [reports and other information]().

Review the [API Details]() to see how to construct your first API request.

## About the Admin API

This API is automatically available to paying [Duo Premier, Duo Advantage, and Duo Essentials plan]() customers and new customers with an Advantage or Premier trial.

Documented properties will not be removed within a stable version of the API. Once a given API endpoint is documented to return a given property, a property with that name will always appear (although certain properties may only appear under certain conditions, like if the customer is using a specific [edition]()). When Duo deprecates a property, the API continues to accept that property in requests, although it no longer has any effect.

Properties that enumerate choices may gain new values at any time, e.g. the device `platform` value could return new device platforms that did not previously exist. Duo may cease to return legacy values for properties as well. Duo will update our API documentation with changes to property values in a timely fashion, adding new property values or indicating changes to existing property values.

New, undocumented properties may also appear at any time. For instance, Duo may make available a beta feature involving extra information returned by an API endpoint. Until the property is documented here its format may change or it may even be entirely removed from our API.

We have started implementing v2 handlers for endpoints. In these cases, the API v1 handler remains supported, but will be limited or deprecated in the future. We encourage use of the v2 endpoints where available and recommend migrating existing API implementations to the v2 handlers.

Please note that all Unix timestamps are in seconds, except where noted.

## First Steps

[Role required](): Owner

Note that only administrators with the [Owner]() role can create or modify an Admin API application in the Duo Admin Panel.

1. [Sign up for a Duo account]().
2. Log in to the [Duo Admin Panel]() and navigate to **Applications**.
3. Click Protect an Application and locate the entry for Admin API in the applications list. Click Protect to the far-right to configure the application and get your integration key, secret key, and API hostname. You'll need this information to complete your setup. See [Protecting Applications]() for more information about protecting applications in Duo and additional application options.
    

Treat your secret key like a passwordThe security of your Duo application is tied to the security of your secret key (skey). Secure it as you would any sensitive credential. Don't share it with unauthorized individuals or email it to anyone under any circumstances!

1. PermissionDetailsGrant administratorsThe Admin API application can read information about, create, update, and delete Duo administrators and administrative units.Grant read informationThe Admin API application can read information about the Duo customer account's utilization.Grant applicationsThe Admin API application can read information about, create, update, and delete Duo applications (referred to as "integrations" in the API).Grant settingsThe Admin API application can read and change global Duo account settings.Grant read logThe Admin API application can read authentication, offline access, telephony, and administrator action log information.Grant read resourceThe Admin API application can read information about resource objects such as end users and devices.Grant write resourceThe Admin API application can create, update, and delete resource objects such as end users and devices.
2. Optionally specify which IP addresses or ranges are allowed to use this Admin API application in Networks for API Access. If you do not specify any IP addresses or ranges, this Admin API application may be accessed from any network.  
    The Admin API performs the IP check occurs after verifying the [authentication signature]() in a request. If you restrict the allowed networks for API access and see logged events for blocked Admin API requests from unrecognized IP addresses, this may indicate compromise of your Admin API application's secret key.
    

Connectivity Requirements

This application communicates with Duo's service on SSL TCP port 443.

Firewall configurations that restrict outbound access to Duo's service with rules using destination IP addresses or IP address ranges aren't recommended, since these may change over time to maintain our service's high availability. If your organization requires IP-based rules, please review [Duo Knowledge Base article 1337]().

Effective June 30, 2023, Duo no longer supports TLS 1.0 or 1.1 connections or insecure TLS/SSL cipher suites. See [Duo Knowledge Base article 7546]() for additional guidance.

## API Clients

Duo Security has demonstration clients available on Github to call the Duo API methods. Examples are available in: [Python](), [Java](), [C#](), [Go](), [Node](), [Ruby](), [Perl](), and [PHP]().

## API Details

### Base URL

All API methods use your API hostname, `https://api-XXXXXXXX.duosecurity.com`. Obtain this value from the Duo Admin Panel and use it exactly as shown there.

Methods always use HTTPS. Unsecured HTTP is not supported.

### Request Format

All requests must have "Authorization" and "Date" headers.

If the request method is `GET` or `DELETE`, URL-encode parameters and send them in the URL query string like this: `/admin/v1/users?realname=First%20Last&username=root`. They still go on a separate line when creating the string to sign for an Authorization header.

Send parameters for `POST` requests in the body as URL-encoded key-value pairs (the same request format used by browsers to submit form data). The header "`Content-Type: application/x-www-form-urlencoded`" must also be present.

When URL-encoding, all bytes except ASCII letters, digits, underscore ("_"), period ("."), tilde ("\~"), and hyphen ("-") are replaced by a percent sign ("%") followed by two hexadecimal digits containing the value of the byte. For example, a space is replaced with "%20" and an at-sign ("@") becomes "%40". Use only upper-case A through F for hexadecimal digits.

A request with parameters, as a complete URL, would look like this: `https://api-XXXXXXXX.duosecurity.com/admin/v1/users?realname=First%20Last&username=root`.

### Response Format

Responses are formatted as a JSON object with a top-level `stat` key.

Successful responses will have a `stat` value of "OK" and a `response` key. The `response` will either be a single object or a sequence of other JSON types, depending on which endpoint is called.

``` javascript
{
  "stat": "OK",
  "response": {
    "key": "value"
  }
}

 ```

Values are returned as strings unless otherwise documented.

Unsuccessful responses will have a `stat` value of "FAIL", an integer `code`, and a `message` key that further describes the failure. A `message_detail` key may be present if additional information is available (like the specific parameter that caused the error).

``` javascript
{
  "stat": "FAIL",
  "code": 40002,
  "message": "Invalid request parameters",
  "message_detail": "username"
}

 ```

The HTTP response code will be the first three digits of the more specific `code` found inside the JSON object. Each endpoint's documentation lists HTTP response codes it can return. Additionally, all API endpoints that require a signed request can return the following HTTP response codes:

| Response | Meaning |
| --- | --- |
| 200 | The request completed successfully. |
| 401 | The "Authorization", "Date", and/or "Content-Type" headers were missing or invalid. |
| 403 |   <br>  <br>This integration is not authorized for this endpoint or the ikey was created for a different integration type (for example, using an Auth API ikey with Admin API endpoints).  <br>  <br> |
| 405 | The request's HTTP verb is not valid for this endpoint (for example, POST when only GET is supported). |
| 429 | The account has made too many requests of this type recently. Try again later. |

### Response Paging

Some API endpoints return a paged list of results on `GET`, up to the API endpoint's `limit`, or maximum results per page.

A successful response when the total results exceed the endpoint's default page size will include a metadata section with information about the total number of objects found and the results returned in the paged response. If the request returns no paging metadata, then either the endpoint does not support paged results or the total results do not exceed one page.

Specifying incorrect paging parameters results in a `400` invalid parameters response.

| Metadata Information | Description |
| --- | --- |
| `total_objects` |   <br>  <br>An integer indicating the total number of objects retrieved by the API request across all pages of results.  <br>  <br> |
| `next_offset` |   <br>  <br>An integer indicating The offset from `0` at which to start the next paged set of results.  <br>  <br>If not present in the metadata response, then there are no more pages of results left.  <br>  <br> |
| `prev_offset` |   <br>  <br>An integer indicating the offset from `0` at which the previous paged set of results started. If you did not specify `next_offset` in the request, this defaults to `0` (the beginning of the results).  <br>  <br> |

Use the metadata information returned to change the paging parameters for your request.

| Paging Parameter | Required? | Description |
| --- | --- | --- |
| `limit` | Optional |   <br>  <br>The maximum number of records returned in a paged set of results.  <br>  <br>Each endpoint that supports paged results has its own `limit` settings, specified like "Default: `100`; Max: `300`".  <br>  <br>If a request specifies a value greater than the endpoint's maximum `limit`, max value is used.  <br>  <br> |
| `offset` | Optional |   <br>  <br>The offset from `0` at which to start record retrieval.  <br>  <br>When used with "limit", the handler will return "limit" records starting at the _n-th_ record, where _n_ is the offset.  <br>  <br>Default: `0`  <br>  <br> |

To retrieve the full set of results for a request with paged results, repeat the call, specifying the `offset` parameter value, until there are no more results (indicated by the absence of `next_offset`).

Paging Metadata Examples:

The metadata response will look like these examples except where noted for an individual API endpoint.

This metadata information indicates that there are 951 total objects returned by that endpoint, and no `offset` or `limit` was specified so the response set defaulted to the first 100 objects:

``` javascript
{
    "metadata": {
        "next_offset": 100,
        "prev_offset": 0,
        "total_objects": 951
    }

 ```

This metadata information indicates that the request specified `offset=500 limit=200`, so the response set was objects 500-699:

``` javascript
{
    "metadata": {
        "next_offset": 700,
        "prev_offset": 300,
        "total_objects": 11318
    }

 ```

This metadata information indicates that there are 2342 total objects, and the request specified `offset=2300` and used that endpoint's default `limit` of 100, so the response set was the end of the list (objects 2300-2342):

``` javascript
{
    "metadata": {
        "prev_offset": 2200,
        "total_objects": 2342
    }

 ```

### Authentication

The API uses [HTTP Basic Authentication]() to authenticate requests. Use your Duo application's integration key as the HTTP Username.

Generate the HTTP Password as an HMAC signature of the request. This will be different for each request and must be re-generated each time.

To construct the signature, first build an ASCII string from your request, using the following components:

| Component | Description | Example |
| --- | --- | --- |
| `date` | The current time, formatted as [RFC 2822](). This must be the same string as the "Date" header. | `Tue, 21 Aug 2012 17:29:18 -0000` |
| `method` | The HTTP method (uppercase) | `POST` |
| `host` | Your API hostname (lowercase) | `api-xxxxxxxx.duosecurity.com` |
| `path` |   <br>  <br>The specific API method's path  <br>  <br> | `/admin/v1/users` |
| `params` |   <br>  <br>The URL-encoded list of `key=value` pairs, lexicographically sorted by key. These come from the request parameters (the URL query string for GET and DELETE requests or the request body for POST requests).  <br>  <br>If the request does not have any parameters one must still include a blank line in the string that is signed.  <br>  <br>Do not encode unreserved characters. Use upper-case hexadecimal digits A through F in escape sequences.  <br>  <br> |   <br>  <br>An example `params` list:  <br>  <br>`realname=First%20Last&username=root`  <br>  <br> |

Then concatenate these components with (line feed) newlines. For example:

```
Tue, 21 Aug 2012 17:29:18 -0000
POST
api-xxxxxxxx.duosecurity.com
/admin/v1/users
realname=First Last&username=root

 ```

GET requests also use this five-line format:

```
Tue, 21 Aug 2012 17:29:18 -0000
GET
api-xxxxxxxx.duosecurity.com
/admin/v1/users
username=root

 ```

Lastly, compute the HMAC-SHA1 of this canonical representation, using your Duo Admin API application's secret key as the HMAC key. Send this signature as hexadecimal ASCII (i.e. not raw binary data). Use [HTTP Basic Authentication]() for the request, using your integration key as the username and the HMAC-SHA1 signature as the password. Signature validation is case-insensitive, so the signature may be upper or lowercase.

For example, here are the headers for the above POST request to `api-XXXXXXXX.duosecurity.com/admin/v1/users`, using `DIWJ8X6AEYOR5OMC6TQ1` as the integration key and `Zh5eGmUq9zpfQnyUIu5OL9iWoMMv5ZNmk3zLJ4Ep` as the secret key:

```
Date: Tue, 21 Aug 2012 17:29:18 -0000
Authorization: Basic RElXSjhYNkFFWU9SNU9NQzZUUTE6NjVhZWJhZWZhNDBjOWYyMWI1NmYzYWExMTc1ZDE3ZGM4N2I1ZDM4NQ==
Host: api-XXXXXXXX.duosecurity.com
Content-Length: 35
Content-Type: application/x-www-form-urlencoded

 ```

Separate HTTP request header lines with CRLF newlines.

Documentation: [https://duo.com/docs/adminapi]()

# Auth API

## API Details

### Base URL

All API methods use your API hostname, `https://api-XXXXXXXX.duosecurity.com`. Obtain this value from the Duo Admin Panel and use it exactly as shown there.

Methods always use HTTPS. Unsecured HTTP is not supported.

#### Request Format

All requests must have "Authorization" and "Date" headers.

If the request method is GET or DELETE, URL-encode parameters and send them in the URL query string like this: `?realname=First%20Last&username=root`. They still go on a separate line when creating the string to sign for an Authorization header.

Send parameters for POST requests in the body as URL-encoded key-value pairs (the same request format used by browsers to submit form data). The header "`Content-Type: application/x-www-form-urlencoded`" must also be present.

When URL-encoding, all bytes except ASCII letters, digits, underscore ("_"), period ("."), tilde ("\~"), and hyphen ("-") are replaced by a percent sign ("%") followed by two hexadecimal digits containing the value of the byte. For example, a space is replaced with "%20" and an at-sign ("@") becomes "%40". Use only upper-case A through F for hexadecimal digits.

A request with parameters, as a complete URL, would look something like this: `https://api-XXXXXXXX.duosecurity.com/admin/v1/users?realname=First%20Last&username=root` (substituting the actual API method path and parameters used in your request).

#### Response Format

Responses are formatted as a JSON object with a top-level `stat` key.

Successful responses will have a `stat` value of "OK" and a `response` key. The `response` will either be a single object or a sequence of other JSON types, depending on which endpoint is called.

``` javascript
{
  "stat": "OK",
  "response": {
    "key": "value"
  }
}

 ```

Values are returned as strings unless otherwise documented.

Unsuccessful responses will have a `stat` value of "FAIL", an integer `code`, and a `message` key that further describes the failure. A `message_detail` key may be present if additional information is available (like the specific parameter that caused the error).

``` javascript
{
  "stat": "FAIL",
  "code": 40002,
  "message": "Invalid request parameters",
  "message_detail": "username"
}

 ```

The HTTP response code will be the first three digits of the more specific `code` found inside the JSON object. Each endpoint's documentation lists HTTP response codes it can return. Additionally, all API endpoints that require a signed request can return the following HTTP response codes:

| Response | Meaning |
| --- | --- |
| 200 | The request completed successfully. |
| 401 | The "Authorization", "Date", and/or "Content-Type" headers were missing or invalid. |
| 403 |   <br>  <br>This integration is not authorized for this endpoint or the ikey was created for a different integration type (for example, using an Auth API ikey with Admin API endpoints).  <br>  <br> |
| 405 | The request's HTTP verb is not valid for this endpoint (for example, POST when only GET is supported). |
| 429 | The account has made too many requests of this type recently. Try again later. |

### Authentication

The API uses [HTTP Basic Authentication]() to authenticate requests. Use your Duo application's integration key as the HTTP Username.

Generate the HTTP Password as an HMAC signature of the request. This will be different for each request and must be re-generated each time.

To construct the signature, first build an ASCII string from your request, using the following components:

| Component | Description | Example |
| --- | --- | --- |
| `date` | The current time, formatted as [RFC 2822](). This must be the same string as the "Date" header. | `Tue, 21 Aug 2012 17:29:18 -0000` |
| `method` | The HTTP method (uppercase) | `POST` |
| `host` | Your API hostname (lowercase) | `api-xxxxxxxx.duosecurity.com` |
| `path` |   <br>  <br>The specific API method's path  <br>  <br> |  |
| `params` |   <br>  <br>The URL-encoded list of `key=value` pairs, lexicographically sorted by key. These come from the request parameters (the URL query string for GET and DELETE requests or the request body for POST requests).  <br>  <br>If the request does not have any parameters one must still include a blank line in the string that is signed.  <br>  <br>Do not encode unreserved characters. Use upper-case hexadecimal digits A through F in escape sequences.  <br>  <br> |   <br>  <br>An example `params` list:  <br>  <br>`realname=First%20Last&username=root`  <br>  <br> |

Documentation: [https://duo.com/docs/authapi]()

# Device API

This API documentation is for Trusted Endpoints Generic Integrations which use the Duo Device Health App to establish trust.

## Overview

Duo's **Trusted Endpoints** feature secures your sensitive applications by ensuring that only known devices can access Duo protected services. When a user authenticates via the Duo Prompt, we'll compare device identifiers collected by the [Duo Device Health application]() installed on that endpoint with the identifiers of known Windows and macOS devices stored in Duo. You can monitor access to your applications from trusted and untrusted devices, and optionally block access from devices not trusted by your organization.

Trusted Endpoints and the Device API are part of the [Duo Premier, Duo Advantage, and Duo Essentials plans]().

Before enabling the Trusted Endpoints policy on your applications, you'll need to create a device cache with the identifying information for your known devices in Duo's service using the [Device API](). Be sure to read the [Trusted Endpoints Generic with Device Health management integration instructions]() in full before using Device API.

Review the [API Details]() to see how to construct your Device API request.

## About the Device API

This API is automatically available to paying [Duo Premier, Duo Advantage, and Duo Essentials]() customers who are using Trusted Endpoints Generic management integrations with the Duo Device Health App to establish trust.

The typical usage of this API will be to start by creating a new device cache. At this point, the device cache is in a pending status and devices added to the device cache are not trusted. If an existing active device cache exists, it is not affected. Next, add devices to the pending cache. This may require more than one operation depending on the number of devices to be added. When all devices have been added to the pending device cache, activate the device cache. Activating the pending device cache will replace any existing active device and the device identifiers contained within will be trusted.

Documented properties will not be removed within a stable version of the API. Once a given API endpoint is documented to return a given property, a property with that name will always appear (although certain properties may only appear under certain conditions, like if the customer is using a specific [edition]()). When Duo deprecates a property, the API continues to accept that property in requests, although it no longer has any effect.

Properties that enumerate choices may gain new values at any time. Duo may cease to return legacy values for properties as well. Duo will update our API documentation with changes to property values in a timely fashion, adding new property values or indicating changes to existing property values.

New, undocumented properties may also appear at any time. For instance, Duo may make available a beta feature involving extra information returned by an API endpoint. Until the property is documented here its format may change or it may even be entirely removed from our API.

## First Steps

1. Log in to the [Duo Admin Panel]() and navigate to **Trusted Endpoints**.
2. Create a Trusted Endpoints Generic with Device Health integration that uses the Duo Device Health App to establish trust. Please see the [instructions for creating a Generic Device Health integration in the Generic Device Health Trust documentation]().  
    The API credentials for use with the Device API are provided within the context of that specific Trusted Endpoints integration.
    

Treat your secret key like a passwordThe security of your Duo device trust integration is tied to the security of your API secret key (skey). Secure it as you would any sensitive credential. Don't share it with unauthorized individuals or email it to anyone under any circumstances!

## API Clients

Duo Security has demonstration clients available on Github to call the Duo API methods. Examples are available in: [Python](), [Java](), [C#](), [Ruby](), [Perl](), and [PHP]().

## API Details

### Base URL

All API methods use your API hostname, `https://api-XXXXXXXX.duosecurity.com`. Obtain this value from the Duo Admin Panel and use it exactly as shown there.

Methods always use HTTPS. Unsecured HTTP is not supported.

### Request Format

All requests must have "Authorization" and "Date" headers.

If the request method is `GET` or `DELETE`, URL-encode parameters and send them in the URL query string like this: `/device/v1/management_systems/[mkey]/device_cache?status=active`. They still go on a separate line when creating the string to sign for an Authorization header.

Send parameters for `POST` requests in the body as URL-encoded key-value pairs (the same request format used by browsers to submit form data). The header "`Content-Type: application/x-www-form-urlencoded`" must also be present.

When URL-encoding, all bytes except ASCII letters, digits, underscore ("_"), period ("."), tilde ("\~"), and hyphen ("-") are replaced by a percent sign ("%") followed by two hexadecimal digits containing the value of the byte. For example, a space is replaced with "%20" and an at-sign ("@") becomes "%40". Use only upper-case A through F for hexadecimal digits.

A request with parameters, as a complete URL, would look like this: `https://api-XXXXXXXX.duosecurity.com/device/v1/management_systems/[mkey]/device_cache?status=active`.

### Response Format

Responses are formatted as a JSON object with a top-level `stat` key.

Successful responses will have a `stat` value of "OK" and a `response` key. The `response` will either be a single object or a sequence of other JSON types, depending on which endpoint is called.

``` javascript
{
  "stat": "OK",
  "response": {
    "key": "value"
  }
}

 ```

Values are returned as strings unless otherwise documented.

Unsuccessful responses will have a `stat` value of "FAIL", an integer `code`, and a `message` key that further describes the failure. A `message_detail` key may be present if additional information is available (like the specific parameter that caused the error).

``` javascript
{
  "stat": "FAIL",
  "code": 40002,
  "message": "Invalid request parameters",
  "message_detail": "username"
}

 ```

The HTTP response code will be the first three digits of the more specific `code` found inside the JSON object. Each endpoint's documentation lists HTTP response codes it can return. Additionally, all API endpoints that require a signed request can return the following HTTP response codes:

| Response | Meaning |
| --- | --- |
| 200 | The request completed successfully. |
| 401 | The "Authorization", "Date", and/or "Content-Type" headers were missing or invalid. |
| 403 |   <br>  <br>This integration is not authorized for this endpoint or the ikey was created for a different integration type (for example, using an Auth API ikey with Device API endpoints).  <br>  <br> |
| 405 | The request's HTTP verb is not valid for this endpoint (for example, POST when only GET is supported). |
| 429 | The account has made too many requests of this type recently. Try again later. |

### Authentication

The API uses [HTTP Basic Authentication]() to authenticate requests. Use your Duo application's integration key as the HTTP Username.

Generate the HTTP Password as an HMAC signature of the request. This will be different for each request and must be re-generated each time.

To construct the signature, first build an ASCII string from your request, using the following components:

| Component | Description | Example |
| --- | --- | --- |
| `date` | The current time, formatted as [RFC 2822](). This must be the same string as the "Date" header. | `Tue, 21 Aug 2012 17:29:18 -0000` |
| `method` | The HTTP method (uppercase) | `POST` |
| `host` | Your API hostname (lowercase) | `api-xxxxxxxx.duosecurity.com` |
| `path` |   <br>  <br>The specific API method's path  <br>  <br> | `/device/v1/management_systems/[mkey]/device_cache` |
| `params` |   <br>  <br>The URL-encoded list of `key=value` pairs, lexicographically sorted by key. These come from the request parameters (the URL query string for GET and DELETE requests or the request body for POST requests).  <br>  <br>If the request does not have any parameters one must still include a blank line in the string that is signed.  <br>  <br>Do not encode unreserved characters. Use upper-case hexadecimal digits A through F in escape sequences.  <br>  <br> |   <br>  <br>An example `params` list:  <br>  <br>`status=active`  <br>  <br> |

Then concatenate these components with (line feed) newlines. For example:

```
Tue, 21 Aug 2012 17:29:18 -0000
POST
api-xxxxxxxx.duosecurity.com
/device/v1/management_systems/[mkey]/device_cache
status=active

 ```

GET requests also use this five-line format:

```
Tue, 21 Aug 2012 17:29:18 -0000
GET
api-xxxxxxxx.duosecurity.com
/device/v1/management_systems/[mkey]/device_cache

 ```

Lastly, compute the HMAC-SHA1 of this canonical representation, using your Duo Device API application's secret key as the HMAC key. Send this signature as hexadecimal ASCII (i.e. not raw binary data). Use [HTTP Basic Authentication]() for the request, using your integration key as the username and the HMAC-SHA1 signature as the password. Signature validation is case-insensitive, so the signature may be upper or lowercase.

For example, here are the headers for the above POST request to `api-xxxxxxxx.duosecurity.com/device/v1/management_systems/[mkey]/device_cache`, using `DME0XUC77ATL3J05HSTB` as the mkey, `DIWJ8X6AEYOR5OMC6TQ1` as the integration key, and `Zh5eGmUq9zpfQnyUIu5OL9iWoMMv5ZNmk3zLJ4Ep` as the secret key:

```
Date: Tue, 21 Aug 2012 17:29:18 -0000
Authorization: Basic RElXSjhYNkFFWU9SNU9NQzZUUTE6OTU3YTRhOTJkYWRlOWUyYWYzYmEwNWQ0ZjE4YjI0ZmY1M2MyOTRmZQ==
Host: api-xxxxxxxx.duosecurity.com
Content-Length: 35
Content-Type: application/x-www-form-urlencoded

 ```

Separate HTTP request header lines with CRLF newlines.

Documentation: [https://duo.com/docs/deviceapi]()
