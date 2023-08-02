# Secure Firewall


## Firepower Management Center

This collection contains examples for Cisco Secure Firewall (Firepower Management Center)  REST API 7.0.0. It includes 138 examples for the various api functions, including managing devices, objects and policies.

The following variables must be set to use the Postman collection:

| Name     | Description                                       | Mandatory | Default         |
| -------- | ------------------------------------------------- | --------- | --------------- |
| fmc_hostname | FQDN or ip address of Firepower Management Center | True      | fmc.example.com |
| fmc_username | Username                                          | True      | api             |
| fmc_password | Password                                          | True      | Cisco123        |
| fmc_domain   | FMC Domain                                        | True      | Global          |

Note: It is recommended to use a dedicated api user, since a user can only be authenticated once!

Documentation for the API can be found at https://www.cisco.com/c/en/us/td/docs/security/firepower/670/api/REST/firepower_management_center_rest_api_quick_start_guide_670.html

## Structure

The structure of this collection is based on FMCs API Explorer.

## Usage

Various requests have dependencies on pre-existing resources hence "Pre-request Scripts" are being used to determine if Postman is already aware of the required UUIDs.

If you encounter an error during execution telling you that a required resource cannot be found you can navigate to the resource folder and find a "Find $ResourceName ID" request. By executing this request Postman will search all existing resources (limited to a page size of 1000) for the name of the resource specified in the collection variables. If a resource with a matching name is found the UUID variable is automatically populated.

## Getting Started

Within the postman collection navigate to "Variables" and edit the required variables hostname, username and password.

![Import Collection](Docs/img/Postman Collection Import.png)

After setting the required variables you will need to retrieve an authorization token. Within the "Authentication" folder you find two calls:

- Retrieve Authorization Token (used for initial authentication)
- Refresh Authorization Token (tokens are valid for 30 minutes and can be refreshed up to 3 times)

![02-authentication](img/02-authentication.png)

## Troubleshooting

Most requests include console logging. Open up the Console to get additional information about a requests result
