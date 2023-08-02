# CNAPP (Panoptica)

# Intro

The Cisco Panoptica REST API has endpoints to perform all activities that can be performed on the console UI, including endpoints to create environments and clusters, create policies, add images and registries.

The REST API uses Escher authentication—described below—which uses a Python library provided by Cisco.

## Prerequisites

- Python, version 3
- Python libraries (pip) - decorator (4.3.0), requests (2.18.4), warrant (0.6.1)
- Cisco Panoptica authentication library, _escherauth.py _from [here]()
- A Panoptica account. See [Getting Started]()
    

# Base URL

The base URL for the REST API is - _appsecurity.cisco.com/api_  
You need to be logged into [the Panoptica server]() to access the APIs.

## API Reference

Click _API _on the Panoptica home page to open the API reference, in Swagger. This shows details of each endpoint and method.

<img src="https://files.readme.io/16e8382-API_link.png">

# Authentication

The REST API uses [Escher]() authentication. This method uses a unique token for each request. The token is a hash generated from fixed Access and Secret keys (obtained from Panoptica), the request URL, and the request time.

## Access & Secret keys

Generate unique Access and Secret keys from the Panoptica console. These will be used for each request.

1. Navigate to the _System _page, and select _MANAGE USERS_.
2. Click _New User _and select _Service User_.
3. Enter a name for the user. Leave the status as 'Active'.
4. Click _FINISH_ to receive the Token window.
5. Copy the values of Access Key and Secret Key, for use in the Escher authentication.
    

<img src="https://files.readme.io/c30cf51-API_Token.png">

## Escher Authentication

To generate the Escher authentication, download the Panoptica authentication module: [escherauth.py]()

It uses these parameters:

- the Access & Secret Keys, copied from the token generated when creating the Service User (above)
- the request URL - see sample below for format
- the current date & time - see sample below for format
    

Format for Request URL

```
response = requests.get(full_url,
                     headers={'X-Escher-Date': date_string,
                              'host': 'securecn.cisco.com',
                              'content-type': 'application/json'},
                     auth=EscherRequestsAuth("global/services/portshift_request",
                                             {'current_time': date},
                                             {'api_key': access_key, 'api_secret': secret_key}))

 ```

Format for Current Date & Time

```
date_format = '%Y%m%dT%H%M%SZ'
  date_string = datetime.datetime.utcnow().strftime(date_format)
  date = datetime.datetime.strptime(date_string, date_format)

 ```

Documentation: [https://docs.panoptica.app/docs/rest-api]()