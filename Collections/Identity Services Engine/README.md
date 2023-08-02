# Identity Services Engine
# Cisco ISE

The External RESTful Services (ERS) APIs are based on HTTPS protocol and REST methodology and uses port 9060. ERS is designed to allow external clients to perform CRUD (Create, Read, Update, Delete) operations on Cisco ISE resources. ERS is based on the HTTP protocol and REST methodology. The External RESTful Services APIs support basic authentication. The authentication credentials are encrypted and are part of the request header. The Cisco ISE administrator must assign special privileges to a user to perform operations using the External RESTful Services APIs.

To perform operations using the External RESTful Services APIs (except for the Guest API), the users must be assigned to one of the following Admin Groups and must be authenticated against the credentials stored in the Cisco ISE internal database (internal admin users):

- External RESTful Services Admin: Full access to all ERS APIs (GET, POST, DELETE, PUT). This user can Create, Read, Update, and Delete ERS API requests.
- External RESTful Services Operator: Read Only access (GET request only).
    

If you do not have the required permissions and still try to perform operations using the External RESTful Services APIs, an error response is received.  
Note: The ERS is limited to Active Directory operations only for Cisco ISE-PIC nodes.  
Note: The ERS APIs support TLS 1.1 and TLS 1.2. ERS APIs do not support TLS 1.0 regardless of enabling TLS 1.0 in the Security Settings window of the Cisco ISE GUI (Administration > System > Settings > Security Settings). Enabling TLS 1.0 in the Security Settings window is related to the EAP protocol only and does not impact ERS APIs.

In this section you will learn how to easily enable ERS and to start building your own applications.

## Enable ERS (port 9060)

ERS uses on HTTPS port 9060 which is by default closed. Clients trying to access this port without enabling ERS first, will face a timeout from the server. Therefore, the first requirement is to enable ERS from the Cisco ISE admin UI. Go to Administration > Settings > API Settings and enable the ERS (Read/Write) toggle button.

## Creating ERS Admin

The second requirement is to create a Cisco ISE Administrator with the admin group as ERS-Admin.

## Setting up ERS for Sponsor Access

To access Sponsor API to perform operation on GuestUsers, it is required to enable ERS as shown above but instead of using ISE Admin, it is required to create an internal user who belongs to the ALL_ACCOUNTS(default) group, and edit the ALL_ACCOUNTS(default) sponsor group setting to enable the REST API for Sponsor. These steps are mentioned below:

Creating internal user belonging to the ALL_ACCOUNTS(default) group

Edit the ALL_ACCOUNTS(default) sponsor group

Allow access for guest users through the REST API

## Using The API

To use the API, a HTTPS client is required. You can use a curl command to post requests to the server, write your own Python scripts or Java clients, or use any HTTP posting tool such as the Poster plugin of Chrome browser. Now your ERS is enabled and ready, you can start using it.

## Deployment

After enabling ERS, it is available for full CRUD operations on a Password Authentication Protocol (PAP), and for readonly access (GET requests) on any Policy Decision Point (PDP).

## Upgrade

After Cisco ISE upgrade procedure, ERS state will be reset to default to disabled state. To continue using the API a manual enable will be required.

# Request Headers

## Request Method Types

The following table describes REST mapping of method types to operations:

| Method Type | Operations |
| --- | --- |
| GET | Get all resources (Search), Get a resource by its ID, Get a resource version information |
| POST | Create a new resource |
| PUT | Update a resource, Other non CRUD operations |
| DELETE | Delete a resource |
| PATCH | Update any attribute subset. Only the attributes sent in the request are affected. |

## Request Headers

The following table describes the request headers:

| Header | Values | Description |
| --- | --- | --- |
| Accept | Application/XML or Application/JSON | Indicates to the server what media type(s) this client is willing to accept |
| Authorization | Basic authorization using username and password (per RFC 2617) | Identifies the authorized user making this request |
| Content-Type | Application/XML or Application/JSON | Describes the representation and syntax of the request message body. |
| ERS-Media-Type | Consists Of: resource-namespace.resource-name.resource-version | This Header is not mandatory. It describes ERS resource version. If not sent from client, the server will assume latest version. |

## Resource Version and Media Type

In the ERS, resource representations and request bodies are normally encoded in XML(as specified in RFC4267). Each type of resource has its own media-type, which matches the pattern xxx.yyy.version , where "xxx" represents the namespace, "yyy" the resource and version specifies the resource version used by the client. (RFC 3023) Example: MediaType for Internal User resource with schema version 1.0:

| identity.internaluser.1.0 |
| --- |

The ERS MUST provide representations of all resources available in XML or JSON. Whenever the requested media type is not supported by the Cisco ISE Server, status 415 will be returned with a list of causes such as "Resource version is no longer supported".

The following example demonstrates a request sent using Linux curl command to fetch an internal user resource by its ID. The ACCEPT header is required by any request type and is added to the curl command using the -H option.

| curl --tlsv1.1 -v -k -H 'ACCEPT: application/json' -H 'ERS-Media-Type: identity.internaluser.1.2' '[https://ers:password@10.56.52.187:9060/ers/config/internaluser']() |
| --- |

# Response Headers

The following table describes the response mandatory headers:

| Header | Values | Description |
| --- | --- | --- |
| LOCATION | new born resource ID | for POST only - holds the new resource ID (as a URI representation). |
| CONTENT-TYPE | Application/XML or Application/JSON | Describes the representation and syntax of the response message body. |

The following example describes a request for getting an Internal User by id sent to ISE and its response using 'postman' google-chrome REST client

<img src="https://pubhub.devnetcloud.com/media/identity-services-engine-api-v1/docs/images/img8.jpg#developer.cisco.com">

# Create a Resource

Creating a resource is achieved by sending a POST request to a resource URI. The request content holds XML/JSON representation of the resource, and must be well formatted according the schema. The request header holds the encrypted authorization and the content-type which defines the resource content. The client can also send the resource version using the ERS-Media-Type header. Create flow main characteristics:

| Description Create | The specified resource |
| --- | --- |
| Synopsis | POST /ers/config/{resource type}/ |
| Request Headers | CONTENT-TYPE, AUTHORIZATION, ERS-MEDIA-TYPE (not mandatory) |
| Request Message Body | XML/JSON representation of the resource |
| Response Headers | Content-Length, Content-Type, Location |
| Response Message Body | success - No Content / fail - ersResponse with Error messages |
| Response Status | 201, 400, 401, 403, 415, 500 |

The example below shows the request and response for creating resource type - endpoint: \* Note the newborn ID, in the LOCATION header.

<img src="https://pubhub.devnetcloud.com/media/identity-services-engine-api-v1/docs/images/img12.jpg#developer.cisco.com">

# Read a Resource

Searching a resource is achieved by sending a GET request to a resource URI. By default, the result would be the first page (page index = 0) with default size of 20. By adding filter, sorting and paging parameters to the URI, the client can control this search. The result is of type SearchResult. The resulting resources are in a collection of Base Resources that contains the name, ID, description, and link for each resource to its full representation to allow the client to easily drill in.  
The following example shows a simple search for the internal user resource:

<img src="https://pubhub.devnetcloud.com/media/identity-services-engine-api-v1/docs/images/search_example.jpeg#developer.cisco.com">

## Adding Filter

Simple filtering should be available through the filter 'query string parameter'. The structure of a filter is a triplet of field operator and value separated with dots. More than one filter can be sent. The logical operator common to ALL filter criteria will be by default AND, and can be changed by using the "filtertype=or" query string parameter. Each resource data model description should specify if an attribute is a filtered field.

Example: get internal users with first name starting with 'a' belonging to identity group 'Finance'

| GET /ers/config/internaluser?filter=name.STARTW.a&filter=identityGroup.EQ.Finance |
| --- |

The following table describes the available filter operators:

| Operator | Description |
| --- | --- |
| EQ | Equals |
| NEQ | Not Equals |
| GT | Greater Than |
| LT | Less Than |
| STARTSW | Starts With |
| NSTARTSW | Not Starts With |
| ENDSW | Ends With |
| NENDSW | Not Ends With |
| CONTAINS | Contains |
| NCONTAINS | Not Contains |

## Paging

The search result is by default paged to 20 resources per page. Page numbering starts at page '1'. Maximum resources per page cannot be more than 100 resources. The paging parameters can override the defaults by using the paging parameters as described in example below. In this example the page size is changed to 50 resources:

| GET /ers/config/internaluser?filter=name.STARTW.a&filter=identityGroup.EQ.Finance&?size=50&page=1 |
| --- |

## Sorting

By default, the sort column is 'name' and sort direction is 'ascendingnot'. Overriding the defaults is done by using the 'sortasc' (ascending order) or 'sortdsc' (descending order) as described in the example below:

| GET /ers/config/internaluser?filter=name.STARTW.a&filter=identityGroup.EQ.Finance&?size=50&page=1&sortdsc=name |
| --- |

# Update a Resource

Performing non CRUD operations is achieved by sending a PUT request to a resource URI. The operation name will appear at the end of the URI. The request content depends on the operation. The request header holds the encrypted authorization and the content-type which defines the resource content and its version. For example, 'register' or 'deregister' of an endpoint, and 'resetPassword' , 'email', 'sms' of a guest user.  
Operation flow main charasteristics:

| Description | Execute an operation on a resource or resources |
| --- | --- |
| Synopsis | PUT /ers/config/{resource type}/{resource-id **optional}/{operation-name} |
| Request Headers | CONTENT-TYPE, AUTHORIZATION |
| Request Message Body | depends on the operation |
| Response Headers | Content-Length, Content-Type |
| Response Message Body | Depends on the operation |
| Response Status | 200, 204, 400, 401, 403, 415, 500 |

# Delete a Resource

Deleting a resource is achieved by sending a DELETE request to a resource URI. The request header holds the encrypted authorization and the content-type which defines the resource content and its version.

| Description Delete | The specified resource |
| --- | --- |
| Synopsis | DELETE /ers/config/{resource type}/{resource-id} |
| Request Headers | CONTENT-TYPE, AUTHORIZATION |
| Request Message | Body No Content |
| Response Headers | Content-Length, Content-Type, ERS-MEDIA-TYPE(not mandatory) |
| Response Message Body | success - No Content / fail - ersResponse with Error messages |
| Response Status | 204, 400, 401, 403, 415, 500 |

The example below shows the request and response for deleting resource type - internaluser:

<img src="https://pubhub.devnetcloud.com/media/identity-services-engine-api-v1/docs/images/img14.jpg#developer.cisco.com">

# Bulk Requests

Bulk requests allow clients to send up to 500 (or 5000 for 'id type') CRUD operations in a single request. All operations in a request would be of the same type and version which means a client cannot send a request with mixed resources. The order of execution is not guaranteed as it is multithreaded execution. The server parses the request and validates its structure and size. If the request is valid, and no other bulk request is already in progress, the execution starts, and the server returns a status code of 202 (ACCEPTED) to the client and a bulk unique Identifier in the 'Location' response header. This ID will allow the client to track the bulk status later on. The status report will be available at least two hours from its start time. If a failure occurs while executing on a resource, the failure will be logged in the status report and the execution will proceed to the next resource meaning that each resource has its own transaction. Only one bulk is allowed to run at a time. If a bulk request was posted while another bulk is still running, the server will return with a response status 503 (Service Unavailable) with a corresponding descriptive message asking the client to try again later.

Two types of bulk requests available:

1) Operation that requires the resource XML itself like creating or updating a resource.  
2) Operation that requires the only resource id like delete, register endpoint, email guestuser etc.  
The bulk request supports 500 resources of the first type or 5000 of the second per a single request.

Bulk Request Charasteristics:

| Description | Bulk Request |
| --- | --- |
| Synopsis | PUT /ers/config/{resource type}/bulk |
| Request Headers | CONTENT-TYPE, AUTHORIZATION |
| Request Message Body | XML representation of the bulk request |
| Response Headers | Content-Length, Content-Type |
| Response : | 202 with Bulkid in the Location Header |
| Response Status | 200, 400, 401, 403, 415, 500 |

The example below shows the request and response for endpoint create bulk request:

<img src="https://pubhub.devnetcloud.com/media/identity-services-engine-api-v1/docs/images/img15.jpg#developer.cisco.com">

# Monitoring bulk execution status

After a successful bulk request, if the server responds with a 202+ bulk id in the Location header, this bulkid can be used to fetch a Bulk Execution Status. The BulkStatus object holds a status entry for each item in the bulk request which describes the execution status. The status is kept in the server for two hours and after that its deleted.  
Bulk Status Request Charasteristics:

| Description | Bulk Status Request |
| --- | --- |
| Synopsis | GET /ers/config/{resource type}/bulk/{bulkid} |
| Request Headers | CONTENT-TYPE, AUTHORIZATION |
| Request Message Body | No Content |
| Response Headers | Content-Length, Content-Type |
| Response | Content: BulkStatus |
| Response Status | 200, 400, 401, 403, 415, 500 |

The example below shows the request and response for endpoint create bulk request status:

<img src="https://pubhub.devnetcloud.com/media/identity-services-engine-api-v1/docs/images/img16.jpg#developer.cisco.com">