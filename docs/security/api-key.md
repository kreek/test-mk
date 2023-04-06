# API key

APIs that don't involve user authentication, PII, or PHI can use API keys for access control. Otherwise, your API will use [OAuth 2.0](../oauth).

External consumers will register for access to an API and obtain an API key via the [developer portal](https://developer.va.gov/apply).

API keys are passed in using a request header and validated at the gateway. An API will not receive a request through the gateway without having a valid API key. 

You can identify the consumer using the `X-Consumer-Username` header in the request.

!!! warning "Requirement"
    - Lighthouse does not apply resource or role-based access control with API keys. Your API is responsible for any fine-grained authorization.
