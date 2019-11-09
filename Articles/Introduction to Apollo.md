# Introduction to Apollo  
**The GraphQL toolkit**  
- - - -  
  
## What is Apollo?  
Apollo is a _team_ and _community_ that build on top of GraphQL, and provides tools that help you build your projects.  
  
Tools provided by Apollo are mainly 3:  
* **Client**  
> It helps you **consume a GraphQL API**, with popular frameworks. 
* **Server**  
> It’s the **server part of GraphQL**, which _interfaces with backend and sends responses back to the client requests_. 
* **Engine**  
> A hosted infrastructure (SAAS) that serves **as a middle man between the client and server**, _providing caching, performance reporting, load measurement, error tracking and many goodies_.   
**You can use just Apollo Client to interface with a 3rd party API, or serve an API using Apollo Server without having a client at all.**  
Apollo is backed by the  [Meteor Development Group](https://www.meteor.io/) , the company behind  [Meteor](https://flaviocopes.com/meteor/)

## Apollo Client

[Apollo Client](https://www.apollographql.com/client)  is the leading **JavaScript client for GraphQL.** Community-driven, it’s designed to let you *build UI components that interface with GraphQL data,* either in *displaying data*, or in *performing mutations when certain actions happen*.

### Get started with Apollo Boost

*Apollo Boost* is the easiest way to start using Apollo Client on a new project. We’ll install that in addition to  `react-apollo`  and  `graphql`.
```bash
npm install apollo-boost react-apollo graphql
```
### Create an ApolloClient object

You start by importing ApolloClient from  `apollo-client`
```javascript
import { ApolloClient } from 'apollo-client'

const client = new ApolloClient()
```

### Apollo Links

An Apollo Link is represented by an  `HttpLink`  object, which we import from  `apollo-link-http`.

Apollo Link provides us a way to **describe how we want to get the result of a GraphQL operation, and what we want to do with the response**.

```javascript
import { ApolloClient } from 'apollo-client'
import { createHttpLink } from 'apollo-link-http'

const client = new ApolloClient({
  link: createHttpLink({ uri: 'https://api.github.com/graphql' })
})
```

### Caching

We’re not done yet. Before having a working example we must also tell  `ApolloClient`  which  [caching strategy](https://www.apollographql.com/docs/react/advanced/caching/)  to use:  `InMemoryCache`  is the default and it’s a good one to start.

```js
import { ApolloClient } from 'apollo-client'
import { createHttpLink } from 'apollo-link-http'
import { InMemoryCache } from 'apollo-cache-inmemory'

const client = new ApolloClient({
  link: createHttpLink({ uri: 'https://api.github.com/graphql' }),
  cache: new InMemoryCache()
})
```

### Use  `ApolloProvider`

Wrapping our application component in the main React file to **connect the Apollo Client to our component tree**
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { ApolloClient } from 'apollo-client'
import { createHttpLink } from 'apollo-link-http'
import { InMemoryCache } from 'apollo-cache-inmemory'
import { ApolloProvider } from 'react-apollo'

import App from './App'

const client = new ApolloClient({
  link: createHttpLink({ uri: 'https://api.github.com/graphql' }),
  cache: new InMemoryCache()
})

ReactDOM.render(
  <ApolloProvider client={client}>
    <App />
  </ApolloProvider>,
  document.getElementById('root')
)
```
### The  `gql`  template tag

GraphQL query will be built using this template tag

```javascript
import gql from 'graphql-tag'

//...

const query = gql`
  query {
    ...
  }
`
```

### Render a GraphQL query result set in a component

```javascript
import React from 'react'
import { graphql } from 'react-apollo'
import { gql } from 'apollo-boost'

const POPULAR_REPOSITORIES_LIST = gql`
{
  search(query: "stars:>50000", type: REPOSITORY, first: 10) {
    repositoryCount
    edges {
      node {
        ... on Repository {
          name
          owner {
            login
          }
          stargazers {
            totalCount
          }
        }
      }
    }
  }
}
`

const App = graphql(POPULAR_REPOSITORIES_LIST)(props =>
  <ul>
    {props.data.loading ? '' : props.data.search.edges.map((row, i) =>
      <li key={row.node.owner.login + '-' + row.node.name}>
        {row.node.owner.login} / {row.node.name}: {' '}
        <strong>
          {row.node.stargazers.totalCount}
        </strong>
      </li>
    )}
  </ul>
)

export default App
```

## Apollo Server

A GraphQL server has the job of **accepting incoming requests on an endpoint, interpreting the request and looking up any data that’s necessary to fulfill the client needs**.
> **Apollo Server is a GraphQL server implementation for JavaScript, in particular for the  [Node.js](https://flaviocopes.com/nodejs/)  platform**


Apollo Server gives us 3 things basically:

-   gives us a way to describe our data with a  **schema**.
-   provides the framework for  **resolvers**, which are functions we write to fetch the data needed to fulfill a request.
-   facilitates handling  **authentication**  for our API.

### Code
```javascript
const { ApolloServer, gql } = require('apollo-server');

// Construct a schema, using GraphQL schema language
const typeDefs = gql`
  type Query {
    hello: String
  }
`

// Provide resolver functions for your schema fields
const resolvers = {
  Query: {
    hello: (root, args, context) => {
      return 'Hello world!'
    }
  }
}

const server = new ApolloServer({ typeDefs, resolvers })

server.listen().then(({ url }) => {
  console.log(`🚀 Server ready at ${url}`)
})
```

#### Details :
We create a **schema definition** using the  `gql`  tag. A schema definition is an template literal string containing the description of our query and the types associated with each field:

```javascript
const { ApolloServer, gql } = require('apollo-server');

const typeDefs = gql`
  type Query {
    hello: String
  }
`
```

A **resolver** is an object that maps fields in the schema to resolver functions, able to lookup data to respond to a query.

```javascript
const resolvers = {
  Query: {
    hello: (root, args, context) => {
      return 'Hello world!'
    }
  }
}
```

We then call the `listen()` method on the sever object, and we listen for the promise to resolve, **which indicates the server is ready**.

If you don't use client, still can type this in terminal:
```bash
$ curl \
  -X POST \
  -H "Content-Type: application/json" \
  --data '{ "query": "{ hello }" }' \
  http://localhost:4000/graphql
```

#### Client Code
```javascript
import React from 'react'
import { gql } from 'apollo-boost'
import { Query } from 'react-apollo'

const App = () => (
  <Query
    query={gql`
      {
        hello
      }
    `}
  >
    {({ loading, error, data }) => {
      if (loading) return <p>Loading...</p>
      if (error) return <p>Error :(</p>

      return data.hello
    }}
  </Query>
)

export default App
```
**./index.js:**

```javascript
const httpLink = createHttpLink({ uri: 'http://localhost:4000/graphql' })

```
