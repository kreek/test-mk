# Lifecycle

As and API version progresses through its lifecycle, consumers test it, use it, and eventually migrate away from it. 
The lifecycle stages, or states, on Lighthouse are:

| State       | Description                                                                                                                                                                                                                                                                                                  |
|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ONBOARDING  | The API is in the process of onboarding to Lighthouse.                                                                                                                                                                                                                                                       |
| ACTIVE      | This version of the API is available in production and is fully supported. There may be more than one 'active' version of an API at a time.                                                                                                                                                                  |
| DEPRECATED  | This version of the API is available for a fixed period of time. It is fully supported for existing consumers. It is not available to new consumers. [Deprecation and Sunset HTTP headers](deprecation/#headers_1) are set to let consumers know that it is deprecated and when the API will be deactivated. |
| DEACTIVATED | This version of the API is unpublished from production and no longer available to any consumer. The footprint of all deployed applications entering this state must be completely removed from production, sandbox, and lower environments.                                                                  |

This section describes the lifecycle and versioning strategies of APIs on Lighthouse:

- [API Evolution](api-evolution): A strategy for enhancing an API without introducing breaking changes.
- [Versioning](versioning): Conventions for versioning an API for major or breaking changes.
- [Deprecation](deprecation): Retiring a version, endpoint, or a complete API.
