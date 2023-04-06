# Deprecation
APIs that teams have replaced with newer versions or are no longer meeting technical, business, or security objectives should be deprecated. However, it’s important to remember that teams at the VA often have different priorities, budgets, and release schedules and may have a long runway to migrate away from a deprecated API. Therefore, we recommend that API teams keep an API in a deprecated state for at least six months before deactivating it. Also, note that some clients, such as mobile applications, may be unable to control when users update a consuming application. Therefore, it may take up to a year for the user base to transition to a version of the app that no longer uses the deprecated API.
The goal of deprecation is to progress to a state where no consumers use the API. Getting to this state depends on clear, timely, and detailed documentation, communication, and monitoring.

!!! warning "Requirement"
    - Providers **must** return [deprecation and an optional sunset header](#headers) in deprecated endpoint's responses.

!!! success "Guidance"
    - Providers **should** define a sunset period for the API they are deprecating. This period is up to the provider, but we recommend at least six months.
    - Providers **should** have a plan for communicating the deprecation and the sunset period to **all consumers** of the API.
    - Providers **should** monitor the deprecated API to ensure its use is declining over the sunset period.

## Headers
The Internet Engineering Task Force has specified two headers for deprecation. ‘Deprecation’ and ‘Sunset.’ Both headers seem similar, and that the Deprecation header can take a boolean value or a timestamp can cause further confusion. 

Deprecation in the context of an API means active but no longer enhanced, e.g., the API is not the latest version, and the development team is no longer adding new features or fixing bugs, but consumers can still access it. 

Sunset means deactivated; the API is no longer accessible at all.

### Deprecation header
This header is used to mark an API as deprecated or slated for deprecation at a future date. Use a boolean `true` value to set the API as actively deprecated.
```
Deprecation: true
```

If deprecation starts at a future date, use an HTTP-date timestamp.
```
Deprecation: Thu, 11 Nov 2048 23:59:59 UTC
```

### Sunset header
The sunset header always takes an HTTP-date timestamp and represents the date that a provider will remove the API.
```
Deprecation: true
Sunset: Thu, 11 Nov 2049 23:59:59 UTC
```

## Documenting deprecations
Between versions, an API team may want to deprecate an endpoint or an element of an API rather than the entire thing. The OpenAPI Specification uses a `deprecated` field to mark complete endpoints or their specific elements as deprecated.

!!! warning "Requirement"
    - Providers **must** document deprecated endpoints and elements in the OAS.
    - Deprecated API endpoints and elements **must** remain supported for the life of the major version.

### HTTP methods
```yaml
paths:
  /appeals:
    get:
      description: Deprecated endpoint to retrieve appeals
      deprecated: true
```
### Query parameters
```yaml
paths:
  /appeals:
    get:
      description: Retrieve appeals status
      parameters:
        - name: fromDate
          in: query
          description: Deprecated start date of appeal
          deprecated: true
```
### Headers
```yaml
paths:
  /appeals:
    get:
      description: Retrieve appeals status
      parameters:
        - name: ORG-Authorization-Token
          in: header
          description: Deprecated ORG-Authorization-Token header
          deprecated: true
```
### Properties in a resource
```yaml
paths:
  /appeals:
    get:
      description: Retrieve appeals status
      responses:
        200:
          description: Successful appeal
          content:
            application/json:
              schema:
                $ref: “#/components/schemas/Appeal”
components:
  schemas:
    Appeal:
      description: Appeal status schema
      properties:
        appealType:
          type: string
          description: The decision review option chosen by the appellant
          deprecated: true
```
