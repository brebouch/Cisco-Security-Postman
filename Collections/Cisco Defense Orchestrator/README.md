# Cisco Defense Orchestrator

## Welcome to the CDO public API[​]()

The goal of our public API is to provide you with a simple and effective way to perform a lot of what you would normally be able to do in the CDO UI, but in code.

To use this API, you will need to know GraphQL. It is very easy to learn, and their [official guide]() provides a thorough, light read. We chose GraphQL because it is flexible, strongly typed, and auto-documenting.

To find the full schema documentation, simply go to the [GraphQL Playground](), and click on the `docs` tab on the right hand side of the page.

---

## Samples[​]()

To help you get started, we have prepared a bunch of [sample GraphQL queries and mutations]() that you can experiment with right away!

##### IMPORTANT

Before running the samples, you must first provide your API token to the playground.  
Click on `HTTP Headers` in the bottom left corner, and replace with your token. This will fix any errors being displayed, and should perform the query successfully.

When you reload the `Samples` page, all of your changes, including the API token that you provided, will be reset. If you want a persistent playground session to work with, use the [empty playground]() . However, as a current limitation, do be warned that if you visit the `Samples` page again, your empty playground data will reset.

Some of the queries will need modification before running them, so you don't end up with bogus data in your account. The required changes are mostly in mutation quries, where it is creating or updating data. Make sure you change any boilerplate, and ensure all the values being set are correct. (You can always delete incorrect data manually if you accidentally run the query before making these changes)

---

## Empty Playground[​]()

For your convenience, we are also providing a raw [empty playground]() with no preset settings. This means you will need to provide your API token by clicking `HTTP Headers` in the bottom left corner of the screen, and pasting this in (with the correct API token):

```
{
  "Authorization": "Bearer "
}

 ```

Copy

Once you have done this, you can start writing queries! Have a look at the auto-generated `docs` tab on the right hand side of the playground to see the kind of queries you can write. The playground's intellisense should help too.

---

## Endpoints[​]()

#### How do I decide what endpoint URL to use?[​]()

We have prepared this table to help you determine what endpoint URL you should hit when making API requests. This is the same URL you need to enter in the Playground.

| Region | Endpoint you should use | Description |
| --- | --- | --- |
| US | [https://edge.us.cdo.cisco.com/api/public]() | Use if you normally access CDO from [www.defenseorchestrator.com]() |
| EU | [https://edge.eu.cdo.cisco.com/api/public]() | Use if you normally access CDO from [www.defenseorchestrator.eu]() |
| APJ | [https://edge.apj.cdo.cisco.com/api/public]() | Use if you normally access CDO from [www.apj.cdo.cisco.com]() |

---

## How do I start coding with the CDO Public API?[​]()

While you are able use any workflow you want with the GraphQL API, here is, in our opinion, the best way to start programming in TypeScript:

First, set up the [GraphQL code generator]()

Once you have followed the setup wizard, make sure your `codegen.yml` config has the correct authorization headers, something like this:

```
schema:
  - https://edge..cdo.cisco.com/api/public:
      headers:
        Authorization: Bearer 

 ```

We also recommend the [Apollo client library]() to help you write code to interface with our GraphQL API.

Documentation: [https://edge.us.cdo.cisco.com/api-explorer/docs/]()