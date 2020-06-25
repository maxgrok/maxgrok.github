---
template: post
title: What are typeDefs in GraphQL?
slug: sixty-sixth-post
draft: false
date: 2018-11-16T10:15:00.000Z
description: This is a post about typeDefs and how to define a very simple
  schema in GraphQL.
category: GraphQL
tags:
  - GraphQL
  - GraphQL-yoga
---

typeDefs are what defines the schema of your GraphQL API. 

typeDefs are usually a string or an array of GraphQL domain specific language strings or a function that takes no arguments and returns an array of graphQL schema language strings. The order of the strings is not important, but there must be a schema definition, except in this example below where we are introducing ourselves to the `typeDefs` via a simple query for information on the graphQL API. [source][https://www.apollographql.com/docs/graphql-tools/generate-schema.html#schema-language]

```js
//index.js
const { GraphQLServer } = require('graphql-yoga')

const typeDefs = `
   type Query{ 
        info: String!
    }`
```

Let's break down what's happening in this code. 

First we are making our file require `graphql-yoga`, which has the following set of features:
>-GraphQL spec-compliant
>-Supports file upload
>-Realtime functionality with GraphQL subscriptions
>-Works with TypeScript typings
>-Out-of-the-box support for GraphQL Playground
>-Extensible via Express middlewares
>-Resolves custom directives in your GraphQL schema
>-Query performance tracing
>-Accepts both application/json and application/graphql content-types
>-Runs everywhere: Can be deployed via now, up, AWS Lambda, Heroku etc.

With this, we will be able to instantiate a new const called `server` in the file, but we will only be covering the first part of the introductory code in this file, the typeDefs (spoiler alert: there are three types of `const`s in t `index.js` file)

Second, we are defining the typeDefs with the `const typeDefs` code.
We do this by first defining the `const typeDefs = ` and by adding a back tick to indicate the schema of the graphQL API. graphQL APIs have three different types of special root types `query`, `mutation`, and `subscription`. 

In this example, we are using the `query` special root type in our root field where we define what type of operations are available to us in the graphQL API. So, we define this with `type Query`, then specify that it has one field called `info`, which is a string with `String!` and the `!` indicates that it cannot be `null`.

With only one root field, only one type of query is accepted by the API, right now, that is the 
```
    query {
        info
    }
```

entered into the graphQL playground command that corresponds to the typeDef set in the graphQL `index.js` file. 

For a full stack tutorial on building your own Hackernews Clone GraphQL API, [visit here][https://www.howtographql.com/graphql-js/0-introduction/]

For more information on GraphQL as a whole,[visit here][https://www.howtographql.com/graphql-js/1-getting-started/]