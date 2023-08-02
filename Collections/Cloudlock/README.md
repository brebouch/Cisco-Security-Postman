# Cloudlock
# Getting Started

The Cisco Cloudlock API is a REST API and uses JSON for all requests and responses. To get started, contact [Cloudlock Support]() to obtain your Cloudlock API URL.

## Overview

The Cloudlock API provides a detailed view of applications, entities, and network incidents in your organization. You can manage lists of destinations (IP addresses), and integrate the Cloudlock detection and response information into your security workflows.

For more information about Cisco Cloudlock, see [Cloudlock Documentation Hub]().

## Authentication

To get started, create your Cloudlock API access token.

Log into Cloudlock using the following URL:

[https://app.cloudlock.com]()

### Generate Your API Token

Create a Cloudlock API access token.

1. Navigate to the **Settings** page.
2. Click the **Authentication & API** tab.
3. Under **API**, click **Generate** to create your access token.
    

> Note: API keys, passwords, secrets, and tokens allow access to your private customer data. You should never share your credentials with another user or organization. 
  

## Base URI

Sample Cloudlock API Base URI:

`https://{YourCloudlockAPIServer}/api/v2`

## Authorization

Every Cloudlock API request requires an `Authorization` header with a valid Bearer access token.

For example:

SHELL

``` shell
Copycurl -i GET 'https://{YourCloudlockAPIServer}/api/v2/activities' \
-H 'Authorization: Bearer %YourAccessToken%' \
-H 'Content-Type: application/json'

 ```

## Pagination

The Cloudlock API collection endpoints support pagination with the `limit` and `offset` query parameters.

| Name | Type | Description | Default Value |
| --- | --- | --- | --- |
| limit | integer | The number of records from the collection to return in a response. The maximum value is 100. | 20 |
| offset | integer | The number that represents the index in the collection. | 0 |

## Rate Limits

The Cloudlock API limits the number and rate of requests for each endpoint. If you are unable to access an endpoint, check your Cloudlock license.

## Response Codes

The Cloudlock API uses HTTP response codes to indicate success or failure of an API request. Codes in the 2xx range indicate success, codes in the 4xx range indicate an error and include an error response object, and codes in the 5xx range indicate an error with Cloudlock's servers.

| Status Code | Message | Description |
| --- | --- | --- |
| **200** | **OK** | Success. Everything worked as expected. |
| **400** | **Bad Request** | Likely missing a required parameter or malformed JSON. The syntax of your query may need to be revised. Check for any spaces preceding, trailing, or spaces in the domain name of the query string. |
| **401** | **Unauthorized** | The authorization header is missing or the access token is invalid. |
| **403** | **Forbidden** | The client is unauthorized to access the content. |
| **404** | **Not Found** | The requested item doesn't exist. Check the syntax of your query or ensure the IP and domain are valid. |
| **429** | **Too Many Requests** | Too many requests received in a given amount of time. You may have exceeded the rate limits for your organization or package. |
| **500** | **Server errors** | This request could not be processed by the Cloudlock server. |

Documentation: [https://developer.cisco.com/docs/cloud-security/#!cloudlock-api-getting-started/response-codes]()