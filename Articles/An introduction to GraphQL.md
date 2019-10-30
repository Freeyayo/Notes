# An introduction to GraphQL

## What is GraphQL
- The new in API design
- A query language
   >It’s not tied to a specific technology / language
- A methodology that **directly competes with REST**
	> Much as SOAP and REST
	
## Principles
- **GraphQL exposes a single endpoint**
	>You **send a query to that endpoint** by using a special Query Language syntax. That query is **just a string**
- The server responds to a query by providing a JSON object
- 
## GraphQL vs REST 
- **REST** is a *concept*. **REST** is a de-facto *architecture standard* but it actually has no specification and tons of unofficial definitions.
- **GraphQL** has a [specification](https://graphql.github.io/graphql-spec/) draft, and it’s a [Query Language](http://graphql.org/learn/queries/) instead of an architecture, with a well defined set of tools built around it (and a flourishing ecosystem)
- With a **REST** approach, you create multiple endpoints and use *HTTP verbs* to distinguish read actions (`GET`) and write actions (`POST`, `PUT`, `DELETE`)
- **GraphQL** has only one endpoint, where you send all your queries
- With **REST**, you generally *cannot choose what the server returns back to you*, unless the server implements *partial responses* using [sparse fieldsets](http://jsonapi.org/format/#fetching-sparse-fieldsets),
	> The API will usually return you much more information than what you need, unless you control the API server as well, and you tailor your responses for each different request.
	
- With **GraphQL** you explicitly request just the information you need, *you don’t “opt out” from the full response default, but it’s mandatory to pick the fields you want*.
	> This helps saving resources on the server, since you most probably need less processing, and also network savings, since the payload to transfer is smaller.
-  **GraphQL** makes it easy to monitor for fields usage
-  **GraphQL** Accesses nested data resources
-  A **REST** API is based on *JSON* which cannot provide type control. **GraphQL** has a *Type System*
