# Versioning
!!! warning "Requirement"
    - External APIs **must** increment the major version within their URI if they introduce a breaking change. 
    - Internal APIs with over one consumer **must** increment the major version within their URI if they introduce a breaking change. 
    - Release notes **must** accompany each major and minor release.

!!! success "Guidance"
    - It is **optional** to increment the major version within their URI when introducing a breaking change if it is an internal API with only one consumer **AND** if the consumer’s clients can be updated at the same time as the API. An example of this would be a web app pushed out to all clients vs. a mobile app where updates roll out more slowly.
    - It's **optional** but recommended to publish release notes for patch versions.

## Breaking change definition
At a high level, a breaking change is any change to an API that would cause an error in a consuming application. The following are all examples of breaking changes:

- Removing an endpoint
- Renaming an endpoint’s path
- Removing an HTTP verb (GET, POST, and so on) for an endpoint
- Removing a property 
- Renaming a property
- Changing a properties-level or tier within an object’s hierarchy
- Making a mandatory property optional 
- Changing a property’s type 
- Changing a property’s format 
- Adding or removing values from an enum

## API versioning scheme
!!! warning "Requirement"
    - APIs **must** only use URI (non-header)-based major versioning.
    - APIs **must** provide a major version number in the URI path after the namespace and before any resources/operations.
    - The versioning scheme **must** start with the lowercase character `v` followed by an integer, the combination of which produces an ordinal number, e.g. `v0`, `v1`, `v2`.
    - APIs **must** NOT expose minor or patch version numbers in the URI path.
    - A minor API version **must** maintain backward compatibility with all previous minor versions within the same major version.
    - For non-major changes, providers **must** still update the minor or patch versions in the OAS documentation.

!!! success "Guidance"
    - Versioning **should** start at `v0`.
    - Versioning **may** start with another ordinal number if the API is being ported to Lighthouse.

*The major version after the namespace.*
```
https://api.va.gov/benefits/v0
```
*Resources or operations specific to the endpoints within the API then show up after the version number.*
```
https://api.va.gov/benefits/v0/claims
```

## Incrementing minor and patch versions

!!! warning "Requirement"
    - Backward-compatible changes that introduce new endpoints or new fields within existing endpoints **must** be released within a new minor version.
    - Changes to the API's underlying service, or upstream services, that do not update its interface or cause any new side effects to the underlying system's data **must** be released as a patch version.
    - Minor version releases **must not** change the major version within the URI.

!!! success "Guidance"
    - Creating a new OAS doc for each minor version can result in many files that are virtually the same. Use '[reference definitions](https://swagger.io/docs/specification/using-ref/)', `$ref`, to reduce duplication and share definitions across OAS docs.


As minor and patch version information is not in the URI path, minor changes must only be documented in the OAS doc for the API.

*Before*

```yaml
info:
  title: Benefits API
  ...
  version: 1.0.0
```

*After*

```yaml
	
info:
  title: Benefits API
  ...
  version: 1.1.0
```

## Release Notes

When should API teams publish release notes? Whenever an API changes in a way that could affect a consumer's implementation. This is regardless of whether the change is within a major, minor, or patch release (Major.Minor.Patch).

The first criterion for publishing release notes is when the OpenAPI specification for the API changes accompanying release notes must be published. In semantic versioning, this would be a major or minor change. API teams may optionally publish releases for changes internal to the API and are abstracted away from the end user. This is almost always a patch change. For example, iPhone security updates do not affect the UI and are released - with a one-sentence release note - as a patch version, e.g., iOS 14.8.1.

The second is when a release causes a change in behavior in the underlying system. i.e., an endpoint with the same contract as before, when now called, has a different effect on its system or upstream systems.

In summary, API teams must publish release notes when the API's interface changes or the system's behavior changes. API teams may optionally publish release notes when a change is internal and does not update the interface or cause any new side effects to the underlying system's data.
