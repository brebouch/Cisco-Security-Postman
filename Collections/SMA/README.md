# Security Management Appliance

The AsyncOS API for Cisco Security Management appliances (or AsyncOS API) is a representational state transfer (REST) based set of operations that provide secure and authenticated access to the Security Management appliance reports, report counters, tracking, quarantine, and configuration. You can retrieve the Security Management appliance reporting, tracking, and quarantine data (for Email Security appliances) using the API. In this release you can query for configuration information. Posting configuration changes is not supported in this release.

This chapter contains the following sections:

- [Prerequisites for Using AsyncOS API]()
- [Enabling AsyncOS API]()
- [Securely Communicating with AsyncOS API]()
- [AsyncOS API Authentication and Authorization]()
- [AsyncOS API Requests and Responses]()
- [AsyncOS API Capabilities]()
    

## Prerequisites for Using AsyncOS API

To use AsyncOS API, you need:

- A Security Management appliance with AsyncOS 12.0.
- Knowledge of:
    - HTTP, which is the protocol used for API transactions. Secure communication over TLS.
    - JavaScript Object Notation (JSON), which the API uses to construct resource representations.
    - JSON Web Token (JWT)
- A client or programming library that initiates requests and receives responses from the AsyncOS API using HTTP or HTTPS, for example, cURL. The client or programming library must support JSON to interpret the response from the API.
- Authorization to access the AsyncOS API. See [Authorization]().
- AsyncOS API enabled using web interface or CLI. See [Enabling AsyncOS API]().
    

## Enabling AsyncOS API

**Before You Begin**

Make sure that you are authorized to access the IP Interfaces page on the web interface or the `interfaceconfig` command on CLI. Only administrators, email administrators, cloud administrators, and operators are authorized.

You can also enable the AsyncOS API using the `interfaceconfig` command in CLI.

### Procedure

---

| Step 1 |  
Log in to the web interface.

| Step 2 |  
\[New web interface only\] Click the gear icon to load the legacy web interface.

| Step 3 |  
Choose Management Appliance Network > IP Interfaces.

| Step 4 |  
Edit the Management interface.

Note  
You can enable AsyncOS API on any IP interface. However, Cisco recommends that you enable AsyncOS API on the Management interface.

| Step 5 |  
Under the AsyncOS API (Monitoring) section, depending on your requirements, select HTTP, HTTPS, or both and the ports to use.

Note  
AsyncOS API communicates using HTTP / 1.1.

If you have selected HTTPS and you want to use your own certificate for secure communication, see [Securely Communicating with AsyncOS API]().

Cisco recommends that you always use HTTPS in the production environment. Use HTTP only for troubleshooting and testing the API.

| Step 6 |  
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

Submit the Security Management appliance's username and password with all the requests to the API, in the Base64-encoded format or with a JSON Web Token. The user inactivity timeout settings in the appliance apply to the validity of a JWT. If a request does not include valid credentials in the Authorization header, the API sends a 401 error message. You can use any base64 library to convert your credentials into base64-encoded format. You can authenticate queries to the API using either of the following two methods:

#### Authenticating API Queries with JSON Web Token

You can generate a JSON Web Token (JWT) and use it with your API queries.

Documentation: [https://www.cisco.com/c/en/us/td/docs/security/security_management/sma/sma12-0/api/b_SMA_API_12/test_chapter_01.html]()