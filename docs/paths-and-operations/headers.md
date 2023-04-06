# Headers

The purpose of HTTP headers is to provide metadata information about the body or the sender of the message in a uniform, standardized, and isolated way.

!!! warning "Requirement"
    - Header names **must be** case insensitive.
    - Headers **must not** include API or domain-specific values.
    - APIs **must not** use headers in a way that changes the behaviour of HTTP methods.
    - APIs **must not** use headers to communicate business logic or service logic (such as paging response info or PII query parameters)

!!! success "Guidance"
    - HTTP headers **should** only be used for the purpose of handling cross-cutting concerns such as authorization.
    - API implementations **should not** introduce or depend on headers.
    - If available, HTTP standard headers **should** be used instead of creating a custom header.


## Accept

This request header specifies the media types that the API client is capable of handling in the response. These API guidelines assume that APIs accept `application/json`.

!!! success "Guidance"
    - APIs handling the request **should not** assume `Accept` is available.



## Accept-Charset

!!! warning "Requirement"
    - APIs **must not** respond to this header. Browsers omit this header and servers should ignore it.

## Content-Language

This request/response header is used to specify the language of the content.

!!! success "Guidance"
    - APIs **should** provide this header in the response.
    - This value of this header **should** correspond to the language of the data in the response.
    - APIs **should** default to using the locale `en-US`.

Example:

```
Content-Language: en-US
```

## Content-Type

This request/response header indicates the media type of the request or response body.

!!! warning "Requirement"
    - APIs **must** include it with response if there is a response body (it is not used with 204 responses).
    - If the content is a text-based type, such as `application/json`, the `Content-Type` **must** include a character-set parameter.
    - The character-set **must** be `UTF-8`.

In a request:

```
Accept: application/json; Accept-Charset: utf-8
```

In a response:

```
Content-Type: application/json; charset=utf-8
```

## Link

According to Web Linking [RFC 5988](https://tools.ietf.org/html/rfc5988), a link is a typed connection between two resources that are identified by Internationalised Resource Identifiers (IRIs).
The Link `entity-header` field provides a means for serializing one or more links in HTTP headers.

!!! warning "Requirement"
    - The Link header **must not**  be used in responses with status codes `201` or `3xx`.

!!! success "Guidance"
    - APIs **should** prefer returning links within the response body's `meta` object.


## Location

The Location response header indicates the URL to redirect a page to. It only provides a meaning when served with a 3xx (redirection) or 201 (created) status response.

!!! warning "Requirement"
    - The Location header **must** only  be used in responses with redirection status codes `3xx` or `201` created.

## Prefer

The [Prefer](https://tools.ietf.org/html/rfc7240) request header field is used to indicate that a particular server behavior(s) is preferred by the client but is not required for successful completion of the request.
It is an end-to-end field and **must** be forwarded by a proxy if the request is forwarded unless `Prefer` is explicitly identified as being hop-by-hop using the Connection header field.

The following token values are possible to use for APIs as long as an API's documentation explicitly indicates support for `Prefer`.

`Prefer: respond-async`: API client prefers that the API server processes its request asynchronously.

Server returns a `202 Accepted` response and processes the request asynchronously.
API server could use a webhook to inform the client subsequently, or the client may call GET to get the response at a later time.

`Prefer: read-consistent`: API client prefers that the API server returns responses from a durable store with consistent data.
For APIs that are not offering any optimization preferences for their clients, this behavior would be the default and it would not require the client to set this token.

`Prefer: read-eventual-consistent`: API client prefers that the API server returns responses from cache or eventually consistent datastore if applicable.
If there is a miss in finding the data from either of these two types of sources, the API server might return response from a consistent, durable datastore.

`Prefer: read-cache`: API client prefers that the API server returns responses from cache if available.
If the cache hit is a miss, the server could return the response from other sources such as eventual consistent datastore or a consistent, durable datastore.

`Prefer: return=representation`: API client prefers that the API server includes an entity representing the current state of the resource in the response to a successful request.
This preference is intended to provide a means of optimizing communication between the client and server. It eliminates the need for a subsequent GET request to retrieve the current representation of the resource after a creation (POST) modification operation (PUT or PATCH).

`Prefer: return=minimal`: API client indicates that the server returns only a minimal response to a successful request.
The determination of what constitutes an appropriate “minimal” response is solely at the discretion of the server.

## ETag

An [entity tag (ETag) response header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) is a good approach to make update requests idempotent.
ETags are generated by the server based on the current resource representation.

## If-Match

Using the `If-Match` header with the current `ETag` value representing the current resource state allows the server to provide idempotent operations and avoid race conditions.
The server would only execute the update if the `If-Match` value matches current `ETag` for the resource.
