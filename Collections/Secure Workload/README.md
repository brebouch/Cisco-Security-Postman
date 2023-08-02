# Secure Workload

The Cisco Secure Workload OpenAPI includes easy-to-use and comprehensive APIs to perform reporting, make configuration changes, export flow data, and more. The APIs come with the service and allow for easier integration with your current security ecosystem.

Cisco Secure Workload OpenAPI capabilities consist of:

- Software agent management (`sensor_management`): Configure and monitor the status of software agents.
- Hardware agent management (`hw_sensor_management`): configure and monitor the status of hardware agents.
- Software download (`software_download`): download software packages for agents and virtual appliances.
- Flow and inventory search (`flow_inventory_query`): query flows and inventory items in a cluster.
- Users, roles, and scope management (`` `user_role_scope_management` ``): able to read, add, modify, or remove users, roles, and scopes.
- User data upload (`user_data_upload`) – lets you upload data for annotating flows and inventory items, or upload good or bad file hashes.
- Applications and policy management (`app_policy_management`): manage applications and enforce policies.
- External system integration – lets you allow integration with external systems like vCenter, Kubernetes, and so on.
- Appliance management: manage the secure workload appliance.
    

Cisco Secure Workload has a Python Software Development Kit (SDK). An SDK, also known as a devkit, is a set of software tools and programs that are used by developers to create applications for specific platforms. The Cisco Secure Workload Python SDK provides a REST client that uses functions for authentication, customized headers, and HTTP requests methods such as GET, POST, PUT, DELETE, and PATCH.

To use a Python SDK, install the SDK using `pip install tetpyclient`, and import the SDK into the script `from tetpyclient import RestClient`.

## OpenAPI Authentication

OpenAPI uses a digest based authentication scheme. The workflow is as follows:

1. Log into the Secure Workload UI Dashboard.
    

1. Generate an API key and an API secret with the desired capabilities.
    

1. Use Secure Workload API sdk to send REST requests in json format.
    

1. To use python sdk, user would install the sdk using `pip install tetpyclient`.
    

1. Once python sdk is installed, here is some boilerplate code for instantiating the RestClient:
    

```
    from tetpyclient import RestClient
    API_ENDPOINT="https://"
    # ``verify`` is an optional param to disable SSL server authentication.
    # By default, cluster dashboard IP uses self signed cert after
    # deployment. Hence, ``verify=False`` might be used to disable server
    # authentication in SSL for API clients. If users upload their own
    # certificate to cluster (from ``Platform > SSL Certificate``)
    # which is signed by their enterprise CA, then server side authentication
    # should be enabled; in such scenarios, in the code below, verify=False
    # should be replaced with verify="path-to-CA-file"
    # credentials.json looks like:
    # {
    #   "api_key": "",
    #   "api_secret": ""
    # }
    restclient = RestClient(API_ENDPOINT,
                    credentials_file='/credentials.json',
                    verify=False)
    # followed by API calls, for example API to retrieve list of agents.
    # API can be passed /openapi/v1/sensors or just /sensors.
    resp = restclient.get('/sensors')

 ```

- [Generate API Key and Secret]()
    

### Generate API Key and Secret

#### Procedure

---

**Step 1:** In the Secure Workload web interface, click the person icon in the upper right corner of the window and choose API Keys.

**Step 2:** Click Create API Key.

**Step 3:** Specify the desired capabilities for the key and secret. User must choose the limited set of capabilities that they intend to use the API Key+Secret pair for. Note, the API capabilities available to the user varies based on user’s roles, e.g. Site Admin users can generate keys to manage software agents but this capability is not available to not non Site Admin users.

**API capabilities include:**

- SW agent management `(sensor_management)`: able to configure and monitor status of SW agents
- Secure Workload software download `(software_download)`: able to download software packages for Secure Workload agents/virtual appliances
- Flow and inventory search `(flow_inventory_query)`: able to query flows and inventory items in Secure Workload cluster
- Users, roles and scope management `(user_role_scope_management)`: able to read/add/modify/remove users, roles and scopes
- User data upload `(user_data_upload)`: allow user to upload data for annotating flows and inventory items or upload good/bad file hashes
- Workspaces and policy management `(app_policy_management)`: able to manage workspaces (“applications”) and enforce policies
- External system integration: able to allow integration with external systems like vCenter, kubernetes etc
- Secure Workload appliance management: able to manage Secure Workload cluster (available only to Site Admin users)
    

**Step 4:** Click Create.

**Step 5:** Copy and paste the key and secret and save it in a safe location. Alternatively, download the API Credentials file.

**Note:**  
If External Auth with LDAP and LDAP Authorization are enabled, access to OpenAPI via API Keys will cease to work seamlessly because Secure Workload Roles derived from LDAP MemberOf groups are reassessed once the user session terminates. Hence to ensure uninterrupted OpenAPI access, we recommend that any user with API Keys have ‘Use Local Authentication’ option enabled in the Edit User Details flow for the user.

Documentation: [https://www.cisco.com/c/en/us/td/docs/security/workload_security/secure_workload/user-guide/3_7/cisco-secure-workload-user-guide/openapi.html#concept_819360]()