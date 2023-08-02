# Secure Network Analytics
# Why Use these APIs?

Cisco Secure Network Analytics (formerly Stealthwatch) provides easy to use and comprehensive APIs for reporting, making configuration changes, managing users, exporting data, and more. Our APIs are free to allow rapid development of apps, and our open source development community is growing quickly.

The Secure Network Analytics REST APIs consist of a collection of resources for developers, administrators, or partners that enable the functionality of Secure Network Analytics to be accessed programmatically. Since Secure Network Analytics REST APIs are based on open standards, you can use any programming or scripting language you wish as long as it supports Hyper Text Transfer Protocol (HTTP). These instructions assume you have some knowledge of using REST services and a basic understanding of HTTP. Experience using either curl or wget is highly beneficial.

## SOAP Information

You may also be interested in the [Stealthwatch Management Console Web Services Programming Guide]() which documents the Simple Object Access Protocol (SOAP) interface to Secure Network Analytics. With the Web Services API that is compliant with the SOAP, administrators can use Internal applications, such as Security Information and Event Management (SIEM) systems, trouble-ticketing systems, and third-party reporting systems, to access data from the management console.

After the release of Secure Network Analytics (formerly Stealthwatch) v7.2, we began migrating from SOAP APIs to REST APIs. If you use Secure Network Analytics API suites, you should begin using REST API equivalents where available.

We do not support SOAP APIs for Secure Network Analytics with a Data Store.

# Authentication

Before you can use the Secure Network Analytics REST API, you need to authenticate. The same credentials (login/password pair) you use to log in to the user interface for Secure Network Analytics can be used for accessing the Secure Network Analytics REST API. If you do not have credentials, the first step is to contact your Secure Network Analytics administrator.

You authenticate by sending a POST request containing the password to Secure Network Analytics. Assuming your Cisco Secure Network Analytics Manager (formerly Stealthwatch Management Console) is at "smcaddress", the username is "jim" and the password is "password123", an example of using curl to authenticate is shown below:

CODE SNIPPET

``` markup
Copycurl -s -k -c cookies.txt -d "username=jim&password=password123" https://smcaddress/token/v2/authenticate

 ```

Assuming the credentials are good, a user session is created and a cookie (stealthwatch.jwt) is returned in the file cookies.txt. You will need to reference the cookie in subsequent calls. In v7.3.2 and later, a second cookie is returned (XSRF-TOKEN). You will need to set an HTTPS header X-XSRF-TOKEN to the value of this cookie for all HTTPS requests (with the exception of GET requests). For more information, refer to the [Stealthwatch Release Notes v7.3.2]().

**NOTE:** Your user session will expire after a period of 20 minutes. This is no different to user sessions initiated through logging in via the browser. To renew your session, POST the token again.

# HTTP Methods and Responses

The Secure Network Analytics APIs use conventional HTTP request methods and response codes to indicate success or failure of an API request. In general, codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information, and codes in the 5xx range indicate an error with our servers.

## HTTP Requests

Use standard HTTP requests to access the Secure Network Analytics REST API. The methods are applied consistently across all resources and should be fairly familiar to any other REST APIs you may have used:

| Desired Option | HTTP Method |
| --- | --- |
| Read | GET |
| Create | POST |
| Update | PUT |
| Delete | DELETE |

**NOTE:** Not all resources support all HTTP methods. For example, some resources may only support GET.

# Parameters

Several of the REST API calls may require identifiers of various objects in the system, for example a domainId. This section describes how to obtain these identifiers.

You can get parameter information from a CLI. To get a host_id list from a Flow Collector, type this command:

CODE SNIPPET

``` markup
Copygrep id= /lancope/var/sw/today/config/groups.xml | awk -F\" '{print $2, $4}' |awk '$1<60000'|sort -k1,1n |less

 ```

To get the domain number for a Manager (formerly Stealthwatch Management Console), type this command:

CODE SNIPPET

``` markup
Copyls /lancope/var/smc/config/ | grep domain

 ```

Documentation: [https://developer.cisco.com/docs/stealthwatch/enterprise/#!why-use-these-apis]()