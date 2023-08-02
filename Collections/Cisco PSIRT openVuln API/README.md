# Cisco PSIRT openVuln API

# Overview

The Cisco Product Security Incident Response Team (PSIRT) openVuln API is a RESTful API that allows customers to obtain Cisco Security Vulnerability information in different machine-consumable formats. APIs are important for customers because they allow their technical staff and programmers to build tools that help them do their job more effectively (in this case, to keep up with security vulnerability information).

## Data Supported

The Cisco PSIRT openVuln API can retrieve the Cisco advisory information in either .json or .xml format.

The advisory information can be queried by multiple identifiers; including advisory_id, CVE-ID, Cisco Bug ID, based on product type or software version or based on timeframes.

The Cisco PSIRT openVuln API integrates with ["Cisco Software Checker"]() to support to searching for Cisco Security Advisories that apply to specific software releases of the following products: Cisco ASA, FMC, FTD, FXOS, IOS, IOS XE, NX-OS and NX-OS in ACI Mode.

Additionally the data returned provide location data to retrieve the advisory in either CVRF or CSAF formats. Cisco recommends focusing on CSAF as CVRF will be phased our for Cisco advisories.

- **CSAF**: Common Security Advisory Framework (CSAF) is a specification for structured machine-readable vulnerability-related advisories and further refine those standards over time. CSAF is the new name and replacement for the Common Vulnerability Reporting Framework (CVRF). Cisco will support CVRF until December 31, 2023. More information at: [https://csaf.io]()
    

# Authentication

The Cisco PSIRT openVuln API applications require an API "access token" in order to authenticate each individual API request. Only authorized accounts are able to submit requests to API operations. All operations must communicate over a secure HTTPS connection.

Prior to making calls to the Cisco PSIRT openVuln API; a user must register their application using the Cisco API Console.

## Registering a new Application

1. Login to the api console [developer portal]() using your Cisco.com ID.a. If you do not have an account please register first by clicking **Register** next to the **Sign In** button.
2. Click on **My Apps & Keys**
3. Click on **Register a New App**
4. Fill in the respective fields.a. Application Type should be serviceb. Grant Type should be Client Credentialsc. Select the **Cisco PSIRT openVuln API**d. Agree to the terms of service.
5. Click on **Register**
    

Under **My Apps & Keys** you will now have a **Key** and a **Client Secret**. These will be used to obtain an access token required, which is required in every API call made.

**IMPORTANT**: Current registered applications will be deprecated in coming months. Please migrate your applications to continue using API’s. Detailed instructions: [Migrating Applications]().

## Accessing the API

1. Register your application as per above; which generates all the details needed to successfully complete the authentication sequence. The registration process creates the client credentials along with name assignment, description, and subscribes the client application to one or more of the OAuth v2.0 grant types requested for their client application.
2. Copy`curl -s -k -H "Content-Type: application/x-www-form-urlencoded" -X POST -d "client_id=deadbabebeef" -d "client_secret=deadbabebeef" -d "grant_type=client_credentials" https://id.cisco.com/oauth2/default/v1/token`Copy`{"token_type":"Bearer","expires_in":3600,"access_token":"eyJraWQiOiI4MUN0dlNFZzFwYzRHNmNBRjFTU0hYNVVaWk5kZ3hzMG1lOVFLZjVocGtzIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULjMwYnF0QjJKdEpmUHpra1BGUGVZUGtRNXpocWtTbXJ5NjJqWW5Cb09oTDQiLCJpc3MiOiJodHRwczovL2lkLmNpc2NvLmNvbS9vYXV0aDIvZGVmYXVsdCIsImF1ZCI6ImFwaTovL2RlZmF1bHQiLCJpYXQiOjE2ODMwMDc0OTAsImV4cCI6MTY4MzAxMTA5MCwiY2lkIjoiNGVwd3U1ajJzc3k5cGg4OHlxNnBrZTc4Iiwic2NwIjpbImN1c3RvbXNjb3BlIl0sImFjY2Vzc19sZXZlbCI6MSwic3ViIjoiNGVwd3U1ajJzc3k5cGg4OHlxNnBrZTc4IiwiZnVsbF9uYW1lIjoibnVsbCBudWxsIiwiYXpwIjoiNGVwd3U1ajJzc3k5cGg4OHlxNnBrZTc4In0.dOEPzpnK9P-xF0--DaDl-Rub-ZlHlebv4Ai4GbaFevZReB4gzfNQljIuhqsHsTDSHIIc8G-M0iQxWHT9mx6TK5WARDrEmJA0qE9n6rV1cTxiMe8SapzT_iqbowIFA5jbfgWpApFwqnGh8tJDWFLClkT1xxQAoIhWrnmMBpZx3MhjV4MnQmSXsJOcFnyj_iSCKhR-6nQWa3qCcYb6JkgiT0nyfHWQfCUcL4QZzo5LX_ilOrGndE6Pc3IPAl6XFVqqsxmWbs2wFEsuqPP-a4sopHAV_FyEeqPTi9uI9CtzPOW0Ya_GGjJ8BX_yJZ0A_zdN2zj9v36hXXfcZuz2JoXsAw","scope":"customscope"}`a. Shown below is an example using CURLCODE SNIPPETThe response will be a json data; with an example shown below:JSONb. Shown below is an example using Postman  
    If you don't have Postman already installed, you can download it from [here](). Once you install it, you can follow the steps below:  
    Under the **Authorization** tab of the reuquest set the following:
    1. Type **OAuth 2.0**
    2. Add authorization data to **Request Headers**  
        Under the **Configure New Token**
    3. Grant Type **Client Credentials**
    4. Access Token URL [<b>https://id.cisco.com/oauth2/default/v1/token</b>]()
    5. Client ID **{{client_id}}** NOTE: Use variables here; but this is obtained from **key** value when you registered the application.
    6. Client Secret **{{client_secret}}** NOTE: Use variables here; but this is obtained from **Client Secret** value when you registered the application.
    7. Client Authentication **Send as Basic Auth Header**
    8. Click on **Get New Access Token**
    9. Click on **Use Token**
3. Copy`curl -X GET -s -k -H "Accept: application/json" -H "Authorization: Bearer eyJraWQiOiI4MUN0dlNFZzFwYzRHNmNBRjFTU0hYNVVaWk5kZ3hzMG1lOVFLZjVocGtzIiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULjMwYnF0QjJKdEpmUHpra1BGUGVZUGtRNXpocWtTbXJ5NjJqWW5Cb09oTDQiLCJpc3MiOiJodHRwczovL2lkLmNpc2NvLmNvbS9vYXV0aDIvZGVmYXVsdCIsImF1ZCI6ImFwaTovL2RlZmF1bHQiLCJpYXQiOjE2ODMwMDc0OTAsImV4cCI6MTY4MzAxMTA5MCwiY2lkIjoiNGVwd3U1ajJzc3k5cGg4OHlxNnBrZTc4Iiwic2NwIjpbImN1c3RvbXNjb3BlIl0sImFjY2Vzc19sZXZlbCI6MSwic3ViIjoiNGVwd3U1ajJzc3k5cGg4OHlxNnBrZTc4IiwiZnVsbF9uYW1lIjoibnVsbCBudWxsIiwiYXpwIjoiNGVwd3U1ajJzc3k5cGg4OHlxNnBrZTc4In0.dOEPzpnK9P-xF0--DaDl-Rub-ZlHlebv4Ai4GbaFevZReB4gzfNQljIuhqsHsTDSHIIc8G-M0iQxWHT9mx6TK5WARDrEmJA0qE9n6rV1cTxiMe8SapzT_iqbowIFA5jbfgWpApFwqnGh8tJDWFLClkT1xxQAoIhWrnmMBpZx3MhjV4MnQmSXsJOcFnyj_iSCKhR-6nQWa3qCcYb6JkgiT0nyfHWQfCUcL4QZzo5LX_ilOrGndE6Pc3IPAl6XFVqqsxmWbs2wFEsuqPP-a4sopHAV_FyEeqPTi9uI9CtzPOW0Ya_GGjJ8BX_yJZ0A_zdN2zj9v36hXXfcZuz2JoXsAw" 'https://apix.cisco.com/security/advisories/v2/cve/CVE-2018-0124'`Copy`{"advisories":[{"advisoryId":"cisco-sa-20180221-ucdm","advisoryTitle":"Cisco Unified Communications Domain Manager Remote Code Execution Vulnerability","bugIDs":["CSCuv67964","CSCvi10692"],"ipsSignatures":["NA"],"cves":["CVE-2018-0124"],"cvrfUrl":"https://sec.cloudapps.cisco.com/security/center/contentxml/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm/cvrf/cisco-sa-20180221-ucdm_cvrf.xml","csafUrl":"https://sec.cloudapps.cisco.com/security/center/contentjson/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm/csaf/cisco-sa-20180221-ucdm.json","cvssBaseScore":"9.8","cwe":["CWE-320"],"firstPublished":"2018-02-21T16:00:00","lastUpdated":"2018-03-09T14:47:00","status":"Final","version":"1.1","productNames":["Cisco Unified Communications Domain Manager "],"publicationUrl":"https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm","sir":"Critical","summary":"A vulnerability in Cisco Unified Communications Domain Manager could allow an unauthenticated, remote attacker to bypass security protections, gain elevated privileges, and execute arbitrary code. \r\n \r\nThe vulnerability is due to insecure key generation during application configuration. An attacker could exploit this vulnerability by using a known insecure key value to bypass security protections by sending arbitrary requests using the insecure key to a targeted application. An exploit could allow the attacker to execute arbitrary code. \r\n \r\nCisco has released software updates that address this vulnerability. There are no workarounds that address this vulnerability. \r\n \r\nThis advisory is available at the following link: \r\n[https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm](\"https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm\")"}]}`Copy`GET https://apix.cisco.com/security/advisories/v2/cve/CVE-2018-0124`Copy`{ "advisories": [ { "advisoryId": "cisco-sa-20180221-ucdm", "advisoryTitle": "Cisco Unified Communications Domain Manager Remote Code Execution Vulnerability", "bugIDs": [ "CSCuv67964", "CSCvi10692" ], "ipsSignatures": [ "NA" ], "cves": [ "CVE-2018-0124" ], "cvrfUrl": "https://sec.cloudapps.cisco.com/security/center/contentxml/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm/cvrf/cisco-sa-20180221-ucdm_cvrf.xml", "csafUrl": "https://sec.cloudapps.cisco.com/security/center/contentjson/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm/csaf/cisco-sa-20180221-ucdm.json", "cvssBaseScore": "9.8", "cwe": [ "CWE-320" ], "firstPublished": "2018-02-21T16:00:00", "lastUpdated": "2018-03-09T14:47:00", "status": "Final", "version": "1.1", "productNames": [ "Cisco Unified Communications Domain Manager " ], "publicationUrl": "https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm", "sir": "Critical", "summary": "A vulnerability in Cisco Unified Communications Domain Manager could allow an unauthenticated, remote attacker to bypass security protections, gain elevated privileges, and execute arbitrary code. \r\n \r\nThe vulnerability is due to insecure key generation during application configuration. An attacker could exploit this vulnerability by using a known insecure key value to bypass security protections by sending arbitrary requests using the insecure key to a targeted application. An exploit could allow the attacker to execute arbitrary code. \r\n \r\nCisco has released software updates that address this vulnerability. There are no workarounds that address this vulnerability. \r\n \r\nThis advisory is available at the following link: \r\n[https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm](\"https://sec.cloudapps.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180221-ucdm\")" } ] }`  
    a. Show below is an example using CURL  
    NOTE: The Bearer string here is what you obtained from step 2a.  
    CODE SNIPPET  
    The response will be json data; with an example shown below:  
    JSON  
    b. Shown below is an example using Postman  
    For the request just use:  
    CODE SNIPPET  
    The Authorization headers will be added as per step 2b above.  
    The reponse will be json data; with an example shown below:  
    CODE SNIPPET
    

## API Client Code

Cisco PSIRT also published a community maintained client code to interact with the API. The openVulnQuery is an open-source community-supported tool created to query the Cisco PSIRT openVuln API. You can obtain the source code at the [GitHub repository]().

Documentation: [https://developer.cisco.com/docs/psirt/#!authentication/api-client-code]()