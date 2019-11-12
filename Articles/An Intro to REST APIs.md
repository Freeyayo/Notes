# An Intro to REST APIs

-  **RE**presentational **S**tate **T**ransfer (REST) is an architectural style that handle the client-server relationship, with the purpose of aiming for **speed and performance by using re-usable components**.
- REST was introduced into the world in 2000

## What is REST API
- A way of easily accessing web service **without having excess processing**. The server will transfer representation of the state of requested resource.
- **API**(application programming interface). It's a set of **rules that allow programs to communicate with each other**. Developers create APIs on the server and allows the client to talk to it. 
- REST is what determined how API looks. **It is the rules that developers follow when the create an API**.
- Each URL is called a **request** while the data sent back to you is called **response**.

## RESTful Architecture
- **Stateless**: **Client data is not stored on the server**, the session is stored client-side.
- **Client< - > Server**: They **operate independently** of each other and both are replaceable.
- **Cache**: Data on the server can be cached on the client.
- **URL Composition**: Use a standardized approach to the composition of base URLs like `GET`,`PUT`,`DELETE`and`POST`.

## REST in Action
- Request is sent from the client to the server **via HTTP in the form of web URL**
- Response is in the form of a resource, which could be anything like HTML, XML, Image or JSON.
- HTTP has 5 methods which are commonly used in a REST based architecture: **POST**, **GET**, **PUT**, **PATCH**, **DELETE**,OPTIONS,HEAD.
- **CRUD** :Create, read, update and delete
- **GET**:**Read**(or retrieve) a representation of a resource
  >- 200 OK
  >- 404 NOT FOUND
  >- 400 BAD REQUEST
 
- **POST**:**Create**  a new resource, used to create subordinate resource
  >- 201 return a location header with a link  to the newly-create resource with 201 HTTP status
 
- **PUT**: Used for **updating** and **creating** . PUT is to a URL that contains the value of a non-existent resource ID
  >- 200(204 if not returning any content of body)
  >- If for creating, it returns 201 on successful creation

- **PATCH**: PATCH request only needs to **contain the changes to the resource**, not the complete resource.
- **DELETE**: Delete a resource identified by a URL
  >- 200 along with a response body 