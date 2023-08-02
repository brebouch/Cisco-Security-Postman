# XDR

# About Cisco XDR APIs

The Cisco XDR platform connects the breadth of Cisco’s integrated security portfolio and the customer’s infrastructure for a consistent experience that unifies visibility, enables automation, and strengthens your security across network, endpoint, cloud, and applications. By connecting technology in an integrated platform, Cisco XDR delivers measurable insights, desirable outcomes, and unparalleled cross-team collaboration. Cisco XDR consists of sub-components, such as, Control Center, Incidents, Automation, Devices and Administration.

Cisco XDR is an "API first" application, which means that the API endpoint that performs a task is the first developed component, and the UI component that calls that API endpoint is the second. The entire user interface of Cisco XDR is an API client that uses the same APIs to which you also have direct access. Any actions that you can perform in the UI can also be performed with the API.

Cisco XDR is built upon a collection of various REST APIs which can be used to integrate your Cisco and third-party security products, automate the incident response process, and manage threat intelligence and security context data in a single location. This guide will serve as a single location to find anything related to these APIs.

For most parts of Cisco XDR, the Cisco Threat Intelligence Model (CTIM) is a data model, an abstract model that organizes data and defines data relationships. CTIM is of utmost importance for Cisco XDR because it provides a common representation of threat information, regardless of whether its source is Cisco or a third party. See the [Cisco Threat Intellligence Model Git repository]() to learn more about CTIM and its components.

For more information about Cisco XDR, go to the [Cisco XDR]() product page.

## Use Cases

With Cisco XDR REST APIs, you can:

- Enrich an IP address, file hash, or any observable types with local and global threat intelligence
- Load threat intelligence into your Private Intel Store
- Automate routine tasks using prebuilt workflows that align to common SecOps use cases
- Share playbooks between Security Operations Center (SOC) teams
- Automated triage and prioritization of alerts from other security portfolio solutions
- Create custom response actions to reduce response time
- Automate enrichment from multiple data sources, overlayed with threat intelligence
    

## API Endpoints

There are many available REST APIs that can be used for integrations. These include:

| API Endpoint | Description |
| --- | --- |
| [Automation]() | Managing Automation objects and running Automation workflows. |
| [Dashboard]() | \[Read-Only\] Lists all the Control Center tiles and data. |
| [Enrich]() | Query local and global threat intelligence for observables. |
| [Global-Intel]() | \[Read-Only\] Global instance of Cisco Threat Intelligence API. |
| [Inspect]() | Parses a string of text and extracts supported observables. |
| [Invite]() | Invites new users to your organization and manage invites (batch also available). |
| [OAuth2]() | Retrieves a token. |
| [Private-Intel (Incidents)]() | Manage and query prioritized Incidents and Worklog Notes. |
| [Private-Intel (Legacy)]() | Private instance of Cisco Threat Intelligence API to manage judgements, indicators and more. |
| [Profile]() | Retrieves and manages profile information of your profile, or users within your organization. |
| [Response]() | Used to take action on an observable within a product. |
| [User]() | Retrieves and manages user information for your organization. |

Documentation: [https://developer.cisco.com/docs/cisco-xdr/#]()