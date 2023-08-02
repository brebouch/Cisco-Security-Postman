# Attack Surface Management (Secure Cloud Insights)

The JupiterOne platform exposes a number of public GraphQL endpoints.

**Base URL**: `https://graphql.us.jupiterone.io`

- For query and graph operations
- For alerts and rules operations
    

**Rate Limits**: Rate limiting is enforced based on your account tier:

- Free: 10/min, no burst
- Freemium: 30/min, no burst
- Enterprise: 30-60/min with burst
    

A `429` HTTP response code indicates the limit has been reached.

**Authentication**: The JupiterOne APIs use a Bearer Token to authenticate. Include the API key in the header as a Bearer Token. You also need to include `JupiterOne-Account` as a header parameter. You can find the `Jupiterone-Account` value in your account by running the following J1QL query:

```
FIND jupiterone_account as a return a._accountId

 ```

**Example cURL command with authentication**

```
curl --location --request POST 'https://graphql.us.jupiterone.io' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer apiToken' \
--header 'Jupiterone-Account: accountID' \
--data-binary @- << EOF
{
  "query": "query J1QL(\$query: String!, \$cursor: String) {queryV1(query: \$query, cursor: \$cursor) { type data cursor } }",
  "variables": {
    "query": "find Domain"
  }
}
EOF

 ```

**Example cURL command using Bash**

```
#!/bin/bash
GRAPHQL_QUERY='
  query J1QL(
    $query: String!
    $cursor: String
    $variables: JSON
    $remember: Boolean
    $includeDeleted: Boolean
    $flags: QueryV1Flags
  ) {
    queryV1(
      query: $query
      variables: $variables
      remember: $remember
      includeDeleted: $includeDeleted
      flags: $flags
      cursor: $cursor
    ) {
      type
      data
      cursor
    }
  }
'
FLATTENED_GRAPHQL_QUERY=$(echo ${GRAPHQL_QUERY} | tr -d '\n')
curl --location --request POST 'https://graphql.api.us.jupiterone.io' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer apiToken' \
--header 'Jupiterone-Account: accountID' \
--data-binary @- << EOF
{
  "query": "${FLATTENED_GRAPHQL_QUERY}",
  "variables": {
    "query": "find Domain"
  }
}
EOF

 ```

An experimental [node.js client and CLI]() is available on Github.

Documentation: [https://community.askj1.com/kb/articles/794-jupiterone-api]()