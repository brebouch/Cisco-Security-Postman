# Umbrella

# Introduction

You can interact with the Umbrella API to protect, analyze, and manage your network elements and traffic, monitor threat intelligence, create rich reports and dashboard integrations, and observe the applications accessible from your networks.

The Umbrella API provides a standard REST interface and OAuth 2.0 client authentication and authorization. Integrate your systems with Umbrella through the Umbrella API and automate your security workflows.

## Advantages of Integrating with the Umbrella API

The Umbrella API features a number of improvements over the Umbrella v1 APIs and the Umbrella Reporting v2 API.

- Intuitive base URI
- API paths defined by top-level scopes
- Intent-based, granular API key scopes
- API key expiration
- Updated API key administration dashboard views
- Programmatic API key administration
- API authentication and authorization supported by OAuth 2.0 client credentials flow
- Portable, programmable API interface for client integrations
    

Before you send a request to the Umbrella API, you must create new Umbrella API credentials and generate an API access token. For more information, see [Umbrella API Authentication]().

## Umbrella API Use Cases

You can view and create Umbrella API requests in the interactive API Reference.

| Use Case | API |
| --- | --- |
| Authentication |   <br>  <br>\- [Token Authorization]()  <br>  <br> |
| Admin |   <br>  <br>\- [KeyAdmin]()  <br>\- [Managed Organizations]()  <br>\- [Users and Roles]()  <br>  <br> |
| Deployments |   <br>  <br>\- [Networks]()  <br>\- [Internal Networks]()  <br>\- [Internal Domains]()  <br>\- [Roaming Computers]()  <br>\- [Sites]()  <br>\- [Virtual Appliances]()  <br>\- [Network Tunnels]()  <br>\- [Network Devices]()  <br>\- [Policies]()  <br>  <br> |
| Policies |   <br>  <br>\- [Destination Lists]()  <br>  <br> |
| Reports |   <br>  <br>\- [Reporting]()  <br>\- [Providers Consoles]()  <br>\- [App Discovery]()  <br>  <br> |

# Authentication

The Umbrella API provides a standard REST interface and supports the OAuth 2.0 client credentials flow. To get started, log in to Umbrella and create an Umbrella API key. Then, use your API credentials to generate an API access token.

> Note: API keys, passwords, secrets, and tokens allow access to your private customer data. You should never share your credentials with another user or organization. 
  

## Prerequisites

- You must have **Full Admin** access to Umbrella to create and manage Umbrella API keys or Umbrella KeyAdmin API keys.
    

## Log in to Umbrella

Log in to Umbrella with the following URL:

[https://dashboard.umbrella.com]()

You can find your username after **Admin** in the navigation tree. Confirm that your organization appears under your username.

## API Key Use Cases

You can create various types of API key. The Umbrella KeyAdmin API enables you to provision and manage Umbrella API keys. Use your Umbrella API keys or legacy Umbrella API keys to create and manage network entities and users, access your reports, manage policies, and integrate your systems and devices with the Cisco Cloud Security platform.

| Key Type | Description | Use Cases |
| --- | --- | --- |
| **Umbrella API key** | Secure, intent-based API key with configured scopes and expiration date. | View and manage your deployments, users, and policies. View reports and logs. |
| **Legacy Umbrella API key** | API key that supports access to multiple API resources using Basic authentication. For more information, see [Authentication](). | View and manage your deployments, users, and policies. View reports and logs. |
| **Umbrella KeyAdmin API key** | Secure API key with configured permissions and expiration date. | View and manage the Umbrella API keys in your organization. |

## Manage API Keys

Create and manage Umbrella API keys.

### Create Umbrella API Key

Create an Umbrella API key ID and key secret.

> Note: You have only one opportunity to copy your API secret. Umbrella does not save your API secret and you cannot retrieve the secret after its initial creation. 
  

1. Navigate to **Admin > API Keys** or in a Multi-org, Managed Service Provider (MSP), or Managed Secure Service Provider (MSSP) console navigate to **Console Settings > API Keys**.
2. - The number of expired API keys appears next to the red triangle.
        - The number of API keys that expire within 30 days appears next to the yellow triangle.
3. Click **Create Key**.
4. Copy and save your **API Key** and **Key Secret**.
    

### Refresh Umbrella API Key

Refresh an Umbrella API key ID and key secret.

> Note: You have only one opportunity to copy your API secret. Umbrella does not save your API secret and you cannot retrieve the secret after its initial creation. 
  

1. Navigate to **Admin > API Keys** or in a Multi-org, Managed Service Provider (MSP), or Managed Secure Service Provider (MSSP) console, navigate to **Console Settings > API Keys**.
2. Click **API Keys**, and then expand an API key.
3. Copy and save your **API Key** and **Key Secret**.
    

### Update Umbrella API Key

Update an Umbrella API key.

1. Navigate to **Admin > API Keys** or in a Multi-org, Managed Service Provider (MSP), or Managed Secure Service Provider (MSSP) console, navigate to **Console Settings > API Keys**.
2. Click **Save**.
    

### Delete Umbrella API Key

Delete an Umbrella API key.

1. Navigate to **Admin > API Keys** or in a Multi-org, Managed Service Provider (MSP), or Managed Secure Service Provider (MSSP) console, navigate to **Console Settings > API Keys**.
2. Click **API Keys**, and then expand an API key.
    

## Manage KeyAdmin API Keys

Create and manage Umbrella KeyAdmin API keys.

### Create KeyAdmin Key

Create a KeyAdmin API key and secret.

> Note: You have only one opportunity to copy your API secret. Umbrella does not save your API secret and you cannot retrieve the secret after its initial creation. 
  

1. Navigate to **Admin > API Keys** or in a Multi-org, Managed Service Provider (MSP), or Managed Secure Service Provider (MSSP) console navigate to **Console Settings > API Keys**.
2. - The number of expired API keys appears next to the red triangle.
        - The number of API keys that expire within 30 days appears next to the yellow triangle.
3. Click **Create Key**.
4. Copy and save your **KeyAdmin Key** and **Key Secret**.
    

### Refresh KeyAdmin Key

Refresh a KeyAdmin API key and secret.

> Note: You have only one opportunity to copy your API secret. Umbrella does not save your API secret and you cannot retrieve the secret after its initial creation. 
  

1. Navigate to **Admin > API Keys** or in a Multi-org, Managed Service Provider (MSP), or Managed Secure Service Provider (MSSP) console navigate to **Console Settings > API Keys**.
2. Copy and save your **KeyAdmin Key** and **Key Secret**.
    

### Update KeyAdmin Key

Update a KeyAdmin API key.

1. Click **Save**.
    

### Delete KeyAdmin Key

Delete a KeyAdmin API key.

1. Click **KeyAdmin Keys**, and then expand an API key.
    

## Generate an API Access Token

You can use any standards-based OAuth 2.0 client library to create an Umbrella API access token. When you provide your Umbrella API key and secret to the Umbrella Token Authorization API, Umbrella authenticates your request and generates an authorization token. Then, include your access token in the `Authorization` request header of each Umbrella API operation.

Umbrella Token Authorization API endpoint:

POST `https://api.umbrella.com/auth/v2/token`

### Best Practices

The Umbrella Token Authorization API endpoint supports the [OAuth 2.0 Client Credentials Flow](). Umbrella only accepts API credentials (key and secret) created by a valid Umbrella Admin user account. Umbrella cannot authenticate requests for deactivated or deleted Admin user accounts.

> Note: An Umbrella OAuth 2.0 access token expires in one hour (3600 seconds). We recommend that you do not refresh an access token until the token is nearly expired. 
  

## Troubleshooting

For information about error conditions that may occur when you generate an access token or authorize an Umbrella API request, see [Errors and Troubleshooting]().

## Token Authorization Request

Use the Umbrella API credentials that you created for your organization to generate an API access token.

### Request

In the `curl` sample, substitute your Umbrella API key and secret for the value of the `user` option.

SHELLPYTHON

``` shell
Copycurl --user ':' --request POST --url 'https://api.umbrella.com/auth/v2/token' \
-H 'Content-Type: application/x-www-form-urlencoded' \
-d 'grant_type=client_credentials'

 ```

### Response Schema

| Name | Type | Description |
| --- | --- | --- |
| token_type | string | The type of access token. |
| access_token | string | The OAuth 2.0 access token. |
| expires_in | integer | The number of seconds after the token is created in which the token expires. |

### Response

Sample response (`200`, OK):

JSON

``` json
Copy{
   "token_type": "bearer",
   "access_token": "xxxxxx",
   "expires_in": 3600
}

 ```