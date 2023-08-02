# Secure Email Appliance

The AsyncOS API for Cisco Secure Email Gateway (or AsyncOS API) is a representational state transfer (REST) based set of operations that provide secure and authenticated access to the email gateway reports, report counters, and tracking. You can retrieve the email gateway reporting and tracking data using the API. In this release you can query for configuration information. Posting configuration changes is not supported in this release.

For more information, refer to the Swagger API help. To view the API help, access the new web interface of the email gateway, click the help icon on the top right corner of the page and select API Help: Swagger.

This chapter contains the following sections:

- [Prerequisites for Using AsyncOS API]()
- [Enabling AsyncOS API]()
- [Securely Communicating with AsyncOS API]()
- [AsyncOS API Authentication and Authorization]()
- [AsyncOS API Requests and Responses]()
- [AsyncOS API Capabilities]()
    

## Prerequisites for Using AsyncOS API

To use AsyncOS API, you need the knowledge of:

- A client or programming library that initiates requests and receives responses from the AsyncOS API using HTTP or HTTPS, for example, cURL. The client or programming library must support JSON to interpret the response from the API.
- Authorization to access the AsyncOS API. See [Authorization]().
- AsyncOS API enabled using web interface or CLI. See [Enabling AsyncOS API]().
    

Note

Version 1.0 APIs are not supported from Cisco Email Security 13.0 release and later. Instead version 2.0 APIs are used.

## Enabling AsyncOS API

**Before You Begin**

Make sure that you are authorized to access the IP Interfaces page on the web interface or the `interfaceconfig` command on CLI. Only administrators, email administrators, cloud administrators, and operators are authorized.

You can also enable the AsyncOS API using the `interfaceconfig` command in CLI.

### Procedure

---

| Step 1 |

Log in to the web interface.

| Step 2 |

Choose Network > IP Interfaces.

| Step 3 |

Edit the Management interface.

| Step 4 |

Under the AsyncOS API (Monitoring) section, depending on your requirements, select HTTP, HTTPS, or both and the ports to use.

| Step 5 |

Submit and commit your changes.

---

## Securely Communicating with AsyncOS API

You can communicate with AsyncOS API over secure HTTP using your own certificate.

Note  
Do not perform this procedure if you are already running the web interface over HTTPS and using your own certificate for secure communication. AsyncOS API uses the same certificate as web interface, for communicating over HTTPS.

### Procedure

---

| Step 1 |

Set up a certificate using the `certconfig` command in the CLI. For instructions, refer the User Guide or Online Help.

| Step 2 |

Change the HTTPS certificate used by the IP interface to your certificate using the `interfaceconfig` command in CLI. For instructions, refer the User Guide or Online Help.

| Step 3 |

Submit and commit your changes.

---

## AsyncOS API Authentication and Authorization

This section explains about the authentication methods, the user roles which can access APIs, and how to query for APIs accessible to a user.

- [Authentication]()
- [Authorization]()
- [Retrieving APIs Accessible to a User Role]()
    

### Authentication

Submit the email gateway's username and password with all the requests to the API, in the Base64-encoded format or with a JSON Web Token. The user inactivity timeout settings in the email gateway apply to the validity of a JWT. If a request does not include valid credentials in the Authorization header, the API sends a 401 error message. You can use any base64 library to convert your credentials into base64-encoded format.

Note

The email gateway allows you to invoke AsyncOS APIs by including access tokens that are retrieved from Identity Providers (IDPs) that support OpenID Connect 1.0. For more details on how to use AsyncOS APIs with external IDPs, see the "System Administration" chapter of the _User Guide for AsyncOS 14.0 for Cisco Secure Email Gateway_.

#### Authenticating API Queries with JSON Web Token

You can generate a JSON Web Token (JWT) and use it with your API queries.

Note

The user inactivity timeout settings in the email gateway applies to the validity of a JWT. The email gateway checks every API query with a JWT, for its time validity. If a JWT is found to be within 5 minutes of time validity, after which it will time out, a new refresh JWT is sent with the response header. You must use this new refresh JWT with API queries, or generate a new one.

| Synopsis |

`POST /esa/api/v2.0/login`

Use the syntax below for two factor authentications:

`POST /esa/api/v2.0/login/two_factor` |

| Body Parameters |

Use Base64 encoded credentials.

```
{
    "data": 
    {
        "userName":"YWRtaW4=",
        "passphrase":"aXJvbnBvcnQ="
    }
}

 ```

| Request Headers |

Host, Accept, Authorization

| Response Headers |

Content-Type, Content-Length, Connection

Documentation: [https://www.cisco.com/c/en/us/td/docs/security/esa/esa14-0/api/b_ESA_API_Guide_14-0/b_ESA_API_Guide_chapter_01.html]()