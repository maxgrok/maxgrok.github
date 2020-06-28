---
template: post
title: Add Query Variables from Props to <Query>
slug: eighty-fourth-post
draft: false
date: 2019-02-24T14:12:00.000Z
description: This is a post on how to make a query take variables passed as
  props from a parent component in a React/Apollo/GraphQL front end
  applications.
category: GraphQL
tags:
  - GraphQL
  - React
  - Apollo
  - react-apollo
---

In this post, we will explore how to make query variables available to GraphQL `<Query>` components in React. This tutorial assumes a basic knowledge of React and GraphQL. 

Please `npm install` or `yarn add` the following dependencies after running `npx create-react-app myappnamehere`
- react-apollo
- graphql-tag

Be sure to import them in the top of your targeted React file like so: 

` import gql from 'graphql-tag'` and 
`import {graphql, Query} from 'react-apollo`

We will jump right in and assume that you have the ApolloClient uri setup for your GraphQL endpoint and ApolloProvider setup in the `<App />` component. This way we can jump right in to adding the variables to the Query component in our `render()` method in our chosen React class component. 

Let's take a look at how to use a basic query that we define for the GraphQL component first, then take a look at the `<Query>` component. 

Outside the React class component and at the bottom of your file before the `export default classcomponenthere`, define the graphQL query like this sample query. 

```javascript
const QUERY_HERE = gql`{
    somepropertyhere{ //any property you want to reference 
        property //specific data returned from the POST query request to the GraphQL endpoint
    }
}`
```

If you are having trouble defining your query, then please see the following resource: [Queries and Mutations](https://graphql.org/learn/queries/), [Basic Queries](https://blog.apollographql.com/the-anatomy-of-a-graphql-query-6dffa9e9e747), and download [GraphiQL](https://electronjs.org/apps/graphiql) to test your query with your GraphQL endpoint before continuing to ensure it grabs the proper data 

(Pro tip: make sure you include any HTTP Headers that are required if you are POSTing to a 3rd party GraphQL endpoint)

Let's go into our React class component `render()` method where our `<Query>` component lives. Here is a sample `<Query>` component usage with the above GraphQL query called "QUERY_HERE": 

```javascript
render(){
  return(
    <Query 
        query={QUERY_HERE} //the query we defined
    >
    {{ loading, error, data }}{
        //if loading the query, display this "Loading..." to the user
        if (loading) return <p>Loading...</p>;
        //if an error occurs in the query, then display to the user "Error: "
        if (error) return <p>Error :</p>;
        
        //if query is successful, return the following
        return (
            /**the result of the .map -ing of this query responses object */
            {data.somepropertyhere.map((property, index)=>{
                //display the following to the user
                return (
                    <div>
                        <p>{index}</p>
                        <p>{property}</p>
                    </div>
                    )
                }}
            )
    }
    </Query>
    )
}```

This is relatively simple use of the `<Query>` component. It returns to the user "Loading..." if it is still loading, an "Error: " notice, if there is an error, and returns the data that is mapped in the form of one `<div>` with the `index` variable and the `property` variable as well to display to the user the results of the query, QUERY_HERE. 

Let's say we want to edit our query to include a variable.

Let's quickly modify our query. Note: This modification will depend largely on your own GraphQL endpoint. Please see the Schema of your endpoint to see what is allowed and what is not. This example will work if the graphQL endpoint is configured properly for it to work. 

```javascript 
const QUERY_HERE = gql`{
    somepropertyhere(where: $name){ //any property you want to reference 
        property //specific data returned from the POST query request to the GraphQL endpoint
    }
}` 
```

This query requires a variable $name. We can add the $name query variable in two ways.  
(1) export the query using graphql, passing props in as an option and then returning the variable to the query. 
OR 
(2) simply add the variables to the `<Query>` component itself as a prop. 

Let's do the first options first. 

At the bottom of the file, with our `export default INSERTYOURCLASSCOMPONENTHERE`, we need to do the following: 

```javascript 
export default graphql(QUERY_HERE, {
    options: (props) => {return {variables: { number: props.name }}} /**defines the number variable as the props passed to the class component from the parent component*/
})(CLASSCOMPONENTHERE);
```

The other way to pass variables to the query component is a lot easier. 

Let's re-examine our `<Query>` component inside our `render()` method.

```javascript
render(){
    return(
    <Query 
        query={QUERY_HERE} //the query we defined
        variables={{name: this.props.name}} /** Passing variables here that were passed as props to the class component */
        >
        {{ loading, error, data }}{
            ...
        }
        </Query>
        )
}
```

This passes the query variables to the `QUERY_HERE` query that are required for it to run. This is easier than the `export default` method and easier to read codewise. 

Now, you know how to add query variables when they are passed as props from a parent component to your class component in React!

Congratulations!