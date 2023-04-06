# OAuth 2.0

OAuth 2.0 lets an API consumer get an access token on behalf of a user (through our authorization code flow or authorization code flow with proof key for code exchange) or system (through client credentials grant). The API will verify the access token and authorize the request.

If your API will use OAuth 2.0, you need to provide the following as part of onboarding:

- Supported scopes
- Intended audience (for identity providers)
- Token profile (defaults are provided below, but Lighthouse can change these if you need)
    - Token lifespan (default is 1 hour for users, 5 minutes for systems)
    - Offline access using refresh tokens (default is enabled for users)
    - Refresh token lifespan (default is 7 days for users)

Lighthouse uses this information to establish a dedicated OAuth server authorization for your API or set of related APIs and consumer documentation on the developer portal, if applicable.

The OAuth URLs will typically mirror your service URLs and will follow this format:

```url
https://sandbox-api.va.gov/oauth2/{service-name}/{system}/v1/*
```

Lighthouse will give you:

- OAuth 2.0/OpenID Connect (OIDC) issuer metadata ([See an example of OIDC issuer metadata](https://sandbox-api.va.gov/oauth2/clinical-health/v1/.well-known/openid-configuration)), which:
    - Defines a valid token issuer
    - Defines the token signing keys
    - Acts as a quick-start guide for consumers using OAuth 2.0/OIDC libraries (more information on building OAuth 2.0/OIDC applications is available on the Lighthouse Authorization page)
- A token-validation key

Consumers will register for access to your API and obtain OAuth credentials via the [developer portal](https://developer.va.gov/onboarding).
