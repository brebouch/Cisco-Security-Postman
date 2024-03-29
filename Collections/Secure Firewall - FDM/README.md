# Secure Firewall - FDM


# Introduction to Firepower Threat Defense REST API

---

You can use a REST API to programmatically interact with a Firepower Threat Defense device that you are managing locally through Firepower Device Manager.

**NOTE**: _Cisco makes no guarantee that the API version included on this Firepower Threat Device (the “API”) will be compatible with future releases. Cisco, at any time in its sole discretion, may modify, enhance or otherwise improve the API based on user feedback._

## Audience for This Programming Guide

This guide is written on the assumption that you have a general knowledge of programming and a specific understanding of REST APIs and JSON. If you are new to these technologies, please first read a general guide on REST APIs.

## About the Firepower Threat Defense REST API

You can use the Firepower Threat Defense REpresentational State Transfer (REST) Application Programming Interface (API), over HTTPS, to interact with a Firepower Threat Defense device through a client program. The REST API uses JavaScript Object Notation (JSON) format to represent objects.

Firepower Device Manager includes an API Explorer that explains all of the resources and JSON objects available for your programmatic use. The Explorer provides detailed information about the attribute-value pairs in each object, and you can experiment with the various HTTP methods to ensure you understand the coding required to use each resource. The API Explorer also provides examples of the URLs required for each resource.

The API is has its own version number. There is no guarantee that a client designed for one version of the API will work for a future version without error or without requiring changes to your program.

### General Process for Using the REST API

In general, your client interacts with a Firepower Threat Defense device using the following iterative process:

1. Obtain an access token to authenticate your API calls. See [Overview of the API Client Authentication Process]().
2. Except when simply reading data, build a JSON payload.
3. Transmit the JSON payload using an HTTPS call for the Universal Resource Locator (URL) for the resource.
4. Consume the returned JSON response.
5. If you make configuration changes, deploy the changes. See [Deploying Configuration Changes]().
    

### Supported HTTP Methods

You can use the following HTTP methods only. The other methods are not supported.

- **GET**—To read data from the system.
- **POST**—To create new objects.
- **PUT**—To modify existing objects. When using PUT, you must include the entire JSON object. You cannot selectively update individual attributes within an object.
- **DELETE**—To remove a user-defined object.
    

### The Base URL for the API

The easiest way to determine the base URL for a given Firepower Threat Defense device is to try out a GET method in the API Explorer, and simply delete the object part of the URL from the result.

For example, you can do a GET /object/networks, and see something similar to the following in the returned output under Request URL:

URL

``` url
Copyhttps://ftd.example.com/api/fdm/v6/object/networks

 ```

The server name part of the URL is the hostname or IP address of the Firepower Threat Defense device, and will be different for your device in place of “ftd.example.com.” In this example, you delete /object/networks from the path to get the base URL:

URL

``` url
Copyhttps://ftd.example.com/api/fdm/v6/

 ```

All resource calls use this URL as the base for the request URL.

In the API Explorer, if you scroll to the bottom of the page, you can see information on the base URL (without the server name) and API version.

Documentation: [https://developer.cisco.com/docs/ftd-api-reference/latest/#]()