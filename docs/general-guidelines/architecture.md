# Architecture

!!! success "Guidance"
    - Lighthouse APIs **should** use a RESTful architecture.
    - SOAP, RPC, or GraphQL, are **not recommended** as they are unsupported by our tooling.


## REST

REST is the most widely-adopted architectural style for distributed systems. Much of its popularity comes from its consistency and the fact that its underlying interface is HTTP, the foundation of data communication for the web which nearly all clients and servers understand.

REST provides a second layer of uniformity by placing 'Resources' and the manipulation of their state via identifiers at the center of client/server interactions. This reduces friction for those developing and implementing the API.

Endpoint paths for resources are one example of REST convention that delineates a solution that would be ambiguous with other architectures. So, for example:

_What's the path for a list of resources?_

REST prescribes using the HTTP GET method and the plural name of the resource:

``` http title="request"
GET ../rx/v0/prescriptions
```

_What if we then want to update a specific resource from that list?_

Use the HTTP PUT method and append its identifier:

``` http title="request"
PUT ../rx/v0/prescriptions/e526c85d-29fc-432c-a16d-df5cdfce2a62
```

### Constraints of a RESTful system
Roy Fielding designed REST to facilitate building distributed systems that are efficient, scalable, and reliable. His PhD dissertation ["Architectural Styles
and the Design of Network-based Software Architectures"](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm) explains this in great detail. This guide does not reproduce that dissertation here, however an API is 'RESTful' if it meets the following constraints[^1]:

- **Client/server separation**: There must be a clear separation of concerns when components like a web or mobile application and its back-end API server work together and communicate.
- **Statelessness**: A request contains all the information needed for the server to execute it. The server does not store client context in a session between requests.
- **Cacheability**: A response to a request must disclose if the client can store and reuse it and for how long.
- **Layered system**: The client is only aware of the details of the server. It does not know or need to know about systems two or more layers down supporting the server.
- **Uniform interface**: Resources and the manipulation of their state via standard HTTP methods guide the development of a consistent interface.

[^1]: An optional sixth constraint, 'Code on demand', in which the server transfers executable code to the client **must not** be implemented in Lighthouse APIs due to security concerns. 
