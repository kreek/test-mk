# Paths
!!! warning "Requirement"
    - Full URIs **must** follow [RFC 3986](https://datatracker.ietf.org/doc/html/rfc3986).
    - Base (server) URLs **must not** have a trailing slash.
    - Base (server) URLs **must** be relative to their public facing URLs on Lighthouse.
    - Paths **must** use dashes rather than underscores for spaces in words.

An example URI for a fictional 'Rx' API that follows the RFC 3986 standard is:

```
GET https://api.va.gov/services/rx/v1/prescriptions/42ceca25-e9d0-466f-84a8-8ce554d70953
```

How this guide, and the OpenAPI spec, refers to parts of a URI differs slightly from that RFC. Following OpenAPI naming conventions the URI above is split into the following parts:

- **`'https://api.va.gov/rx/v1'`** is the base URL found in the "servers" section of the OAS. 
- **`'https'`** is the scheme.
- **`'api.va.gov'`** is the authority.
- **`'rx'`** is the namespace.
- **`'v1'`** is the version.
- **`'prescriptions'`** is the operation path.
- **`'42ceca25-e9d0-466f-84a8-8ce554d70953'`** is the resource identifier.
- **`'GET'`** is the operation or method.

## Namespaces

!!! success "Guidance"
    - Namespaces **should** match the product name of the API as closely as possible.
    - Namespaces **should** remain consistent between versions.
    - Namespaces **should not** reflect internal VA organizational and communication structures.
    - Namespaces **should not** be more than 2 levels deep.
    - Namespaces **should not** include application names ('WebSphere') or environments ('PROD').


Take care in choosing a namespace. This is the name of your API and should not change version to version. The namespace of your API is part of your product branding and should be user, rather than provider, centered. Choose a namespace that matches as closely as possible the product name of the API in the OAS documentation.

## Operation paths

While everything after the namespace is technically still part of the path in URI terms, for APIs--and specifically those that follow the OpenAPI spec--paths are pointers to resource operations within a version.

The [OpenAPI spec](https://swagger.io/docs/specification/paths-and-operations/) says:

> In OpenAPI terms, paths are endpoints (resources), such as /users or /reports/summary/, that your API exposes, and operations are the HTTP methods used to manipulate these paths, such as GET, POST or DELETE.

Resources will inform how you construct your paths. Most of your paths will revolve around returning a [collection of resources](#collection-resources), and performing Create, Read, Update, and Destroy (CRUD) operations on [singular resources](#singular-resources).

Note that REST and CRUD are not synonymous. An API can be RESTful without CRUD operations. If an operation does not fall under the CRUD resource operations, we consider it ‘[Non-Resourceful](#non-resourceful-endpoint-paths)’ and the naming of its path requires special care.

## Collection resources

!!! success "Guidance"
    - Providers **should** use plural nouns for collections.

Collections represent a list of resources. We prefer plural nouns with no identifier in the URL path. The [Read section](../read) of this guide has details on crafting requests and responses for collections.
```
GET ../rx/v1/prescriptions
```
Use query parameters to customize the returned collection, such as for pagination. An example is:
```
GET ../rx/v1/prescriptions?page=2&pageSize=10
```

## Singular resources

!!! success "Guidance"
    - Resources **should** NOT be identified using easily guessable sequential numbers.
    - APIs **should** use a UUID as a resource identifier, as outlined in [RFC 4122](https://www.ietf.org/rfc/rfc4122.txt).
    - Identifiers **should** be in the form 8-4-4-4-12 with 36 total characters (32 hexadecimal characters and 4 hyphens).

These represent a single instance of a resource in a collection. They should be completely and uniquely identified on the URL path.

*The UUID ‘42ceca25-e9d0-466f-84a8-8ce554d70953’ uniquely identifying a claim*
```
GET ../rx/v1/prescriptions/42ceca25-e9d0-466f-84a8-8ce554d70953
```

### Sub-resources

!!! success "Guidance"
    - Sub-resources **should** be appended after the parent's identifier.
    - When called with no sub-resource identifier, an API **should** return all the sub-resources.
    - Sub-resource in paths more than 2 levels deep are **not recommended**.

Below is an example of a one-to-many resource relationship, showing a prescription with many refill sub-resources. To return all the refills for the prescription with id ‘42ceca25-e9d0-466f-84a8-8ce554d70953’, use:
```
GET ../rx/v1/prescriptions/42ceca25-e9d0-466f-84a8-8ce554d70953/documents
```
Then to return a single sub-resource, in this case the refill with id ‘2b0f2b18-a476-4e13-a4e0-b2fb8f499ae4’, use:
```
GET ../rx/v1/prescriptions/42ceca25-e9d0-466f-84a8-8ce554d70953/refills/2b0f2b18-a476-4e13-a4e0-b2fb8f499ae4
```

## Paths for custom operations

!!! success "Guidance"
    - Providers **should** show that a custom operation was intentional by ending the path with a verb.

There may be considerations that force us into a non-standard path.

For example, you may decide to support searching of resources using POST rather than GET. This may be because a lengthy query would exceed the server’s limit (often 1024 and 2048 characters, for query strings, and URLs respectively), or for security reasons to avoid PII or PHI showing up in logged URLs.

In cases like these, use a verb rather than a noun for the [custom operation](../custom-operations).

```json
POST .../rx/v1/prescriptions/search
```
