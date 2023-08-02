# Kenna
# Getting Started with Kenna Platform

This page will help you get started with Kenna Platform. You'll be up and running in a jiffy!

This page will help you quickly get started with the Kenna Platform.

## Code Samples

Here are some code examples to get you started.

Please see the [API Authentication]() section for tips on obtaining the `X-Risk-Token`.

### Python

```
import json
import requests
url = "https://api.kennasecurity.com:443/assets"
token = "123abc"
headers = {'content-type': 'application/json', 'X-Risk-Token': token}
api = requests.get(url, headers=headers)
response = api.json()
print(json.dumps(response, indent=4, sort_keys=True))`

 ```

### Ruby

```
require 'net/https'
require 'uri'
require 'json'
token = "123abcd"
url = URI.parse("https://api.kennasecurity.com")
http = Net::HTTP.new(url.host, 443)
http.use_ssl = true
http.start do |http|
  request  =
    Net::HTTP::Get.new("/assets", {'X-Risk-Token' => token, 'Accept' => 'application/json'})
  response = http.request(request)
  puts JSON.pretty_generate JSON.parse(response.body)["assets"]
end

 ```

### Java

```
import java.net.*;
import java.io.*;
public class SimpleRequest {
  public static void main(String[] args) throws Exception {
    try {
      URL risk = new URL("https://api.kennasecurity.com/assets");
      HttpURLConnection conn = (HttpURLConnection) risk.openConnection();
      /* insert account token below */
      conn.addRequestProperty("X-Risk-Token", "123abc");
      conn.addRequestProperty("Accept", "application/json");
      if (conn.getResponseCode() != 200) {
        throw new RuntimeException("Failed : HTTP error code : " + conn.getResponseCode());
      }
      BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
      String inputLine;
      while ((inputLine = in.readLine()) != null) {
        System.out.println(inputLine);
      }
      in.close();
      conn.disconnect();
    } catch (MalformedURLException e) {
      e.printStackTrace();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}

 ```

# API Authentication

The first step to interfacing with Kenna's API is to identify the base API URL which your Kenna subscription is hosted on. You can determine this by looking at the format of the subdomain for your subscription. The format of your URL for the user interface will match that of the API base URL. If you're unsure of which API base URL is right for you, please reach out to your Kenna administrator or account team. The examples use `api.kennasecurity.com`.

Access to the API is controlled using a key, also referred to as a token. Only administrators can manage and retrieve keys for other users and only users with system roles (administrators, normal and read-only) are allowed API access at this time (see [Role Permissions]() and [API Key Generation]() help pages for details).

Administrators may locate and change API keys by logging in and clicking the settings menu in the upper right hand corner. In the dropdown that appears, choose 'API Keys'. Administrators may create, change or revoke API keys from this menu. API keys can be copied one time immediately after they are generated. If a user loses their key, an admin will need to generate a new key in order to copy and distribute the key.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. You must authenticate for all requests. In order to authenticate you must include a `X-Risk-Token` header with its value set to your API token discussed in the previous paragraph. In order to do this in a `curl` request you would use the flag `--header 'X-Risk-Token: SoL310X108mps'` with an API key.

In ReadMe, the API key can be add in the Authentication section on the right side. That way, it will appear in all the API examples when using the "Try It!" button.

In bash or zsh shells, make sure you use single quotes to set environment varaibles; for example,

`export KENNA_API_KEY='7apikey-with2dashes-and0a1period.'`

In Windows, using DOS,

`set KENNA_API_KEY=7apikey-with2dashes-and0a1period.`

# Parameters

Many API methods take optional parameters. For GET requests, ID parameters follow REST conventions and are specified as a segment in the path. For example if you were requesting data regarding a vulnerability with an ID of 100:

```
curl -H "X-Risk-Token: " "https://api.kennasecurity.com/vulnerabilities/100" -X GET",

 ```

With the response:

```
"vulnerability":
  {
    "id": 100,
    "created_at": "August 01, 2013 19:19",
    "closed":true,
    "score":15,
    "priority":0,
    "threat":10,
    "urls":
      {
        "asset": "https://api.kennasecurity.com/asset/98"
      },
    "notes": null,
    "patch": true,
    "trending": false,
    "prioritized": false,
    "top_exploit": false,
    "wasc_id": null,
    "cve_id": "CVE-2002-0510",
    "description" :null,
    "solution": null,
    "asset_id": 98
  }

 ```

_Note_: The `Accept` HTTP header parameter is usually 'application/json'; however, it some cases it is `application/gzip`. Each API in the example area shows the recommended `Accept` value.

For most POST requests, parameters are encoded in the HTTP body as JSON, with a Content-Type of `application/json`:

```
curl -H "X-Risk-Token: " -H "Content-Type: application/json"
  https://api.kennasecurity.com/vulnerabilities
  -X POST
  -d '{
        "vulnerability":
          {
            "wasc_id" : "WASC-01",
            "primary_locator" : "url",
            "url" : "http://www.example.com"
          }
      }',

 ```

With the response:

```
"vulnerability":
  {
    "id":20238,
    "created_at": "August 10, 2013 15:50",
    "closed": false,
    "score": 18,
    "priority": 0,
    "threat": 10,
    "urls":
      {
        "asset": "https://api.kennasecurity.com/asset/183"
      },
    "notes": null,
    "patch": false,
    "trending": null,
    "prioritized": false,
    "top_exploit": null,
    "wasc_id": "WASC-01",
    "cve_id": CVE-2022-1211,
    "description": "Example CVE.",
    "solution": "There is no solution",
    "asset_id": 183
  }

 ```

_Note_: The `Content-Type` HTTP header parameter should be omitted for specific GET request endpoints, such as the "[Retrieve Data Export]()" endpoint.

Larger record sets are paginated. For example, when requesting a page from the list of vulnerabilities, a `page` query parameter is used. Each paginated response includes meta data containing the current page and the total number of pages. Page limit is currently set to 20. Pages are 1-indexed based.

```
      "code": "curl -H "X-Risk-Token: " "https://api.kennasecurity.com/vulnerabilities/?page=3" -X GET",
      "language": "curl",
      "name": "Request"
    },

 ```

With the response:

```
  "vulnerabilities":
    [{
      "id": 100,
      "created_at": "2013-09-24 15:50:21 UTC",
      "closed": true,
      "score": 15,
      "priority": 0,
      "threat": 10,
      "urls":
        {
          "asset":"https://api.kennasecurity.com/asset/98"
        },
      "notes": null,
      "patch": true,
      "trending": false,
      "prioritized": false,
      "top_exploit": false,
      "wasc_id": null,
      "cve_id":" CVE-2002-0510",
      "description": null,
      "solution": null,
      "asset_id": 98
    },
    {
      ...
    }],
  "meta":
    {
      "page": 3,
      "pages": 310
    }
}

 ```

See the [Pagination]() section for more details."

# Data Types

## Data Exchange

The request body format for HTTP POSTS and PUTS is [JSON](). The response body format is either JSON or gzip JSON.

## Data Types used in the Schema

Kenna APIs use data types specified in the [Open API Specification]() which includes:

- string
- number
- integer
- boolean
- array
- object
    

Strings can be generic or be in the `date-time` format which follows the [ISO-8601]() format. Integers are specificed as `int32` or `int64`. If the format of the integer is not specified, assume `int32`. Numbers utilize the `float` format. Assume `float` if format is not specified.

# Errors

In the case of an API error, the appropriate HTTP status code will be returned in the response header. In addition, the following response body will be returned:

```
error: 
message: 
success: false

 ```

[The&nbsp;<code>success</code>&nbsp;attribute will always be&nbsp;<code>false</code>&nbsp;when there is an API error. An&nbsp;]()  [HTTP status code]() description is in the `error` attribute, while a `message` attribute contains an API specific error message.

Here is `curl` example with an error response:

```
curl -H "X-Risk-Token: " "https://api.kennasecurity.com/assets/100" \
-X PUT -d {"priority":"-1"}`
HTTP/1.1 422 Unprocessable Entity
Date: Tue, 04 Dec 2012 16:31:27 GMT
Server: Apache
X-Powered-By: Phusion Passenger (mod_rails/mod_rack) 3.0.17
X-Runtime: 1.384526
Cache-Control: no-cache
X-Request-Id: 088038cd5e4d20f33fa5756fa8791676
X-UA-Compatible: IE=Edge
Status: 422
Content-Length: 133
Content-Type: application/json; charset=utf-8
{
  "error": "unprocessable_entity",
  "message": "priority":"must be an integer between 0 and 10",
  "success": "false"
}

 ```

## HTTP Status Codes

The following HTTP status codes are returned by the API:

| Code | Meaning |
| --- | --- |
| 102 | Processing |
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 206 | Partial Content |
| 400 | Bad Request |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 412 | Precondition Failed |
| 422 | Unprocessable Entity |
| 429 | Too Many Requests (see "Rate Limit" below) |
| 500 | Internal Server Error |

## Error Processing Guidelines

- Error handling in client code should be based on the HTTP status code, not of the message returned.
- Messages are returned for informational purposes.
    

Over time, new error messages may be added and current error messages may be modified. Error message changes are considered compatible.

## Rate Limit

The rate limit per IP address is a maximum of five API requests per second. If this rate is exceeded, an HTTP error 429 will be returned. An example of handling a rate limit error is located in a [code example]().

# Resources

## Code Samples

- [From our blogs]()
- [Various API samples including uploading KDI, and a postman collection]()
    

## Blogs

| Title | Date Published | APIs Discussed |
| --- | --- | --- |
| [Counting Closed Vulnerabilities]() | 2023-03-15 | [Search Vulnerabilities]() |
| [Using Kenna Security APIs to Query Connector Run Details]() Connector run details using `curl`. | 2023-02-01 | [List Connectors]()  <br>[List Connector Runs]()  <br>[Show Connector Run]() |
| [Uploading a File to a Connector]() | 2022-12-16 | [Upload Data File]() |
| [Extracting Talos Zero Day Information]() | 2022-12-08 | [Search Vulnerabilities]()  <br>[Show Vulnerability]() |
| [API Support for CVSS v3]() | 2022-09-23 | [List Vulnerabilities]()  <br>[Show Vulnerabilities]()  <br>[Search Vulnerabilities]() |
| [Kenna API Documentation Updates]() | 2022-08-23 | [Search Vulnerabilities]()  <br>[Request Data Exports]()  <br>[Check Data Exports]()  <br>[Create User]()  <br>[Update User]() |
| [How to Work with Custom Fields: Part Two]() | 2022-06-30 | [Search Vulnerabilities]()  <br>[Update Vulnerabilities]()  <br>[Bulk Update Vulnerabilities]() |
| [How to Create a CISA Risk Meter]()  <br>Create a [CISA]() risk meter. | 2022-04-05 | [List Asset Group]()  <br>[Create Asset Group]()  <br>[Search Vulnerabilities]()  <br>[Bulk Update Vulnerabilities]() |
| [Risk Meter Reports - PowerShell]()  <br>Learn about risk accept from risk meter reports. | 2022-02-24 | [List Asset Groups]()  <br>[Historical Vulnerability Risk Category Counts]()  <br>[Past Due Vulnerabilities by Risk Level Over Time]() |
| [Risk Meter Reports - Python]()  <br>Learn about historical risk meter reports. | 2021-12-23 | [List Asset Groups]()  <br>[Historical Vulnerability Risk Category Counts]()  <br>[Risk Accepted by Risk Level Over Time]() |
| [Risk Meter Vulnerabilities]()  <br>Learn how to customize risk meter reports. | 2021-11-03 | [List Asset Groups]()  <br>[Search Assets]()  <br>[Show Vulnerability]() |
| [Automating Connector Runs]()  <br>Automate your connector runs so that DevOps doesn't have to. | 2021-08-26 | [List Connector Runs]()  <br>[Run Connector]()  <br>[List Connectors]() |
| [How to Turn A Data Overload into Actionable Intelligence]()  <br>Turn your vulnerability data into a results with `curl`. | 2021-08-09 | [Request Data Exports]()  <br>[Retrieve Data Exports]()  <br>[Check Data Exports]() |
| [Automating Risk Meter Creation]()  <br>Automatically create risk meters from asset tags. | 2021-07-29 | [List Asset Groups]()  <br>[Create Asset Group]()  <br>[Search Assets]() |
| [Kenna Security Postman Collection]()  <br>Check-out Kenna Security's Postman Collection. | 2021-06-24 |  |
| [Acquiring Vulnerabilities per Asset]()  <br>Acquire vulnerabilities per each asset. | 2021-05-10 | [Request Data Exports]()  <br>[Retrieve Data Exports]()  <br>[Check Data Exports]()  <br>[List Vulnerabilities]()  <br>[List Assets]() |
| [Inactivate An Asset]()  <br>Inactivate an asset instead of deletion. | 2021-04-06 | [List Assets]()  <br>[Uupdate Assets]() |

## RSS

Sign-up for Changelog RSS feeds at: apidocs.kennasecurity.com/changelog.rss

## Videos

- [Data Exports -UI &amp; API]() - 2021-11-10
- [Best Practices for Data Exports]() - 2022-04-20
    

## Webinars

- [How to Start Using APIs in Your Vulnerability Management]() - 2021-07-22
- [Power User Webinar: Uncovering Kenna's API (Part 1)]()
- [Power User Webinar: Getting Our Hands Dirty with Kenna's Super Clean APIs (Part 2)]()"