# Meraki

# Meraki Dashboard API

A RESTful API to programmatically manage and monitor Meraki networks at scale.

<img src="https://pubhub.devnetcloud.com/media/Meraki-Dashboard-API-v1-Documentation/docs/images/cloud-code.png#developer.cisco.com">

## What can you do with it?

- Add new organizations, admins, networks, devices, VLANs, and more
- Configure thousands of networks in minutes
- On-board and off-board new employees’ teleworker setup automatically
- Build your own dashboard for store managers, field techs, or unique use cases
    

Checkout out the [Explore]() section for open source projects, or browse the [Marketplace]() for partner solutions.

## What's New in v1

The Dashboard API has evolved significantly, providing hundreds of endpoints to manage your Meraki networks!

We want to do so much more. But in order for us to include many of these new features or improvements, we need to break a few things.

The focus of this **major** version is on **Simplicity** and **Scale**, by providing an enjoyable developer experience.

The API documentation, Postman collection, and Python library will remain synced and up-to-date with improved navigation and features.

In addition, several improvements and new endpoints have been included with this major release.

### API Documentation

The API Endpoint documentation and complimenting Postman Collection have a new folder structure for navigating the API.

#### Categories

The services are grouped into categories, providing a collection of endpoints that behave in a similar way.

**CONFIGURE** endpoints are for managing cloud configurations

**MONITOR** endpoints will return status and history information

**LIVE TOOL** endpoints will directly interact with the device

### Resource Path changes

The endpoint URL paths will always contain the Meraki product if required, reducing ambiguity when working with resources that have similar yet unique functionality.

**Examples of a product and service**

`/appliance/ports`

`/switch/ports`

### Base URI

In most parts of the world, every API request will begin with the following **base URI**:

`https://api.meraki.com/api/v1`

For organizations hosted in the China dashboard, please specify the following base URI instead:

`https://api.meraki.cn/api/v1`

### See all the changes

Visit the [Changelog]() for all the details.

### SDKs

Going forward, the custom Meraki [Python library]() will be the recommended SDK for simplified API scripting. The previously auto-generated Python, Node.js, and Ruby SDKs for **v0** will remain in the Meraki GitHub but will no longer be maintained.

#### Python

The Meraki [Python Library]() has been updated to take advantage of all the new API enhancements plus many custom features to help both beginners and experienced programmers.

## Authorization

Dashboard API v1 supports Bearer Auth using the standard `Authorization` header parameter. The value will be a string that begins with the word `Bearer`, followed by your Meraki API key.

**Note:** When developing scripts, it's a best practice to create a local environment variable `MERAKI_DASHBOARD_API_KEY` and set it to your API key, so that you can omit it from your source code. Instructions vary by operating system, so please consult your OS vendor for more information.

JSONCURLPYTHON

``` json
Copy{
 "Authorization": "Bearer "
}

 ```

## Obtaining your Meraki API key

In order to interact with the Dashboard API, you must first obtain an API key.

- Open your Meraki dashboard: [https://dashboard.meraki.com]()
- Once logged in, navigate to the _Organization > Settings_ page.
- Ensure that the API Access is set to “Enable access to the Cisco Meraki Dashboard API”
    

<img src="https://pubhub.devnetcloud.com/media/Meraki-Dashboard-API-v1-Documentation/docs/images/dashEnableOrgAPI.png#developer.cisco.com">

Then go to your profile by clicking on your account email address (on the upper-right) _\> My profile_ to generate the API key.

> &lt;p &gt;save this key in a secure location, as it represents your admin credentials&lt;/p&gt; 
  

<img src="https://pubhub.devnetcloud.com/media/Meraki-Dashboard-API-v1-Documentation/docs/images/dashGenerateAPIkey.png#developer.cisco.com">

### Troubleshooting

If you get a 401 Unauthorized error (with message _"Missing API key"_) when using dashboard API v1 with Bearer token, check the following to troubleshoot:

1. As an example, when using your API key to retrieve the [GET /organizations]() endpoint, you should see the same data as shown when navigating to [https://api.meraki.com/api/v1/organizations]() in your browser, using an authenticated session with the credentials that generated the API key.
2. Next, check that your API call has the correct header with the following (and not v0's `X-Cisco-Meraki-API-Key`):
    

JSON

``` json
CopyAuthorization: Bearer {API_KEY}

 ```

1. If making the API call in cURL, check that the **\--location-trusted** flag is included.
2. If you really want to write your own functions in Python, then you will need to define a new instance of the **requests.Session** class that does not _rebuild_auth_ upon a redirect. For example:
    

PYTHON

``` python
Copyfrom requests import Session
class NoRebuildAuthSession(Session):
    def rebuild_auth(self, prepared_request, response):
      '''
      No code here means requests will always preserve the Authorization header when redirected.
      Be careful not to leak your credentials to untrusted hosts!
      '''
session = NoRebuildAuthSession()
API_KEY = '6bec40cf957de430a6f1f2baf056b99a4fac9ea0'
response = session.get('https://api.meraki.com/api/v1/organizations/', headers={'Authorization': f'Bearer {API_KEY}'})
print(response.JSON())

 ```

1. If using PowerShell with **Invoke-RestMethod**, make sure that the _\-PreserveAuthorizationOnRedirect_ flag is included.
2. The behavior here is standard and due to API clients like cURL and Postman stripping the Authorization header, for security purposes, when following an HTTP redirect.