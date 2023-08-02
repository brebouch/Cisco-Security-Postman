# pxGrid Cloud

# Cisco Platform Exchange Grid Cloud

Cisco Platform Exchange Grid (pxGrid) Cloud is a new Cisco Cloud offer that enables you to share contextual information between on-prem applications and cloud-based solutions without compromising the security of your network.

Today we rely on multiple technology solutions to meet the security and reliability needs of our network. Cisco pxGrid Cloud bridges the gap created by these siloed applications by providing a unified framework that enables seamless data integration between various cloud applications and on-premise Cisco Identity Services Engine (Cisco ISE). Cisco pxGrid Cloud is customizable, enabling you to share and consume only the data that is relevant to your situation.

Cisco Identity Services Engine (Cisco ISE) Release 3.1 Patch 3 and later support Cisco pxGrid Cloud. Cisco pxGrid Cloud is included as part of the Cisco ISE Advantage license. Cisco and its partners and customers can develop pxGrid Cloud-based applications and register them with the pxGrid Cloud offer. These applications use the External RESTful Services (ERS) and the pxGrid API to exchange information with Cisco ISE. The use cases addressed vary by the application that is onboarded. To learn more about specific application integrations, login to [pxGrid Cloud]().

Cisco pxGrid Cloud offers the following benefits:

- Secure connection to the internet for cloud ecosystem integration.
- Plug and play deployment without requiring infrastructure changes to your network.
- Cisco ISE as a single source of truth for endpoint identity by delivering consistent context exchange with on-prem and cloud partners.
- Enrichment of Software as a Service-based (SaaS-based) security analysis with real-time endpoint context from on-prem security products.
- Threat containment by isolating endpoints from the network through actions initiated from the security SaaS solutions.
    

# Cisco pxGrid Cloud Terminology

The following are some of the common terms that are used in the Cisco pxGrid Cloud solution and their meaning in the Cisco pxGrid Cloud environment:

- **App**: An application service catalogue entry in pxGrid Cloud App Store
- **Tenant**: An organization is represented as a tenant. Sometimes, an organization can have more than one tenant (for different business units or different locations).
- **Offer**: A set of capabilities packaged together and offered as a solution.
- **Subscription**: An instance of an offer being consumed by a tenant is a subscription.
- **Device**: A device (also called as a product) is any on-prem asset (physical or virtual). A Cisco ISE deployment is a device for pxGrid Cloud.
- **Data Exchange Hub (dxHub)**: Bi-directional Data Exchange framework with Publisher/Subscriber semantics.
    

# Overview

The App Registry manages applications and the activation of tenants on those applications. Once an application is created, an application developer is provided with an API key that can be utilized to call the API as an application.  
To use the API key, the application simply provides it in the `X-API-KEY` header.

Once an application is created, tenants may activate the application through a one time password operation. The tenant requests a one time password from the application store and provides that to the application at a URL defined by the application. The application then redeems this one time password with the App Registry in exchange for the credentials, that enables the application to perform further operations under the tenant's identity.

The following sections describe the App Registry API that is used by an application integrating with the pxGrid Cloud subsystems.

## Using an API Key

Using an API key to authorize a request to cloud services is as simple as including it in the `X-API-KEY` header:

CODE SNIPPET

``` markup
CopyGET /api/v1/.... HTTP/1.1
X-API-KEY: unQjc80hiF0W28vH254uGS8tyxPk0VoLxWTo64ONdtXxwFpl4ZlD7mAhpTzBlQDy
Accept: application/json
...

 ```

## Error Responses

App Registry conforms to standard REST interpretations when issuing HTTP status codes in response to errors (non 2xx series status codes).

In the case of a non-2xx response status, a descriptive error object is returned, for example:

CODE SNIPPET

``` markup
CopyHTTP/1.1 404 Not Found
Content-Type: application/json
{ 
  "error": "Application not found"
}

 ```

Documentation: [https://developer.cisco.com/docs/pxgrid-cloud/api/#!overview]()