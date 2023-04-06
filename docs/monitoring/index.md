# Monitoring

## Health Checks

!!! warning "Requirement"
    - Each version of an API **must** have a unique health check endpoint.
    - APIs **must not** repurpose application endpoints as health checks.

Lighthouse’s internal monitoring tools require that each version of an API has a health check endpoint. This is because some APIs deploy versions on different infrastructure, and versions that share infrastructure now may not always do so in the future. However, if two versions of an API do share infrastructure, you may route each version's health check endpoints to the same controller.

Health checks vary in complexity between APIs. At a minimum, the health check should confirm that requests are successfully being routed, through the gateway and VA network, to your application. More holistic health checks may validate that the application’s internal dependencies, such as data stores and upstream services, are up and operating without error.

Health checks are required to be standalone endpoints rather than repurposed endpoints that return application resources. This ensures that the health check is free of caching, auth, and business logic.

### Example URI and Responses

!!! warning "Requirement"
    - Health check endpoints **must not** require authentication.
    - Health check endpoints **must** return a 200 response code if the API is up.
    - Health check endpoints **must** return a 5xx level response code if the API is down.

Below is an example health check URI and response for a fictional 'Rx' API on Lighthouse.

```
GET https://api.va.gov/services/rx/v1/healthcheck
```

There is not a finalized RFC for healthcheck responses. A [draft proposal](https://inadarei.github.io/rfc-healthcheck) outlines a potential response of which only `status` is required with one of the following values:

- `pass` with 200 response code, the API is available and functioning as expected.
- `fail` with 5xx response code, the API is unavailable or throwing errors. If the health check response itself is down the Lighthouse gateway will return a 503 response.
- `warn` with 2xx-3xx response code range. The API is up but having intermittent issues.

```json title="200 OK"
{
    "status": "pass",
    "version": "1",
    "releaseId": "1.2.2",
}
```

```json title="503 Service Unavailable"
{
    "status": "fail",
    "version": "1",
    "releaseId": "1.2.2",
}
```

### Accessibility

APIs must provide a health check endpoint that Lighthouse monitoring tools can access so they can monitor the status of the API. API consumers can then take appropriate action if an API cannot handle requests or is unhealthy. The health check endpoint or endpoints made available to consumers of an API must not provide internal or sensitive information within the response. In addition, the health check must reflect the state of the interface, not the service behind that interface.
