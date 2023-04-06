# Token validation

!!! warning "Requirement"
    - APIs MUST confirm the presence and validity of the access token.

Lighthouse does not perform token validation for OAuth 2.0 APIs at the gateway. The gateway acts as a pass-through. You are responsible for ensuring that requests contain a valid token and any further authorization decisions; however, Lighthouse provides a token validation service that we encourage you to use.

The format and structure of the token may vary depending on the issuer and audience.

Requests sent to your APIs should include an access token as a [bearer authorization header](https://datatracker.ietf.org/doc/html/rfc6750#section-2.1) for services that require authorization.

```
Authorization: Bearer xyz
```

## Token-validation service

Lighthouse provides a RESTful service for API providers to validate a token. Using this service requires an API key, which will be issued during onboarding. If you use this service, you should implement a caching strategy for performance.

This service abstracts away the token format, varying issuers, and signing keys and ensures consistent validation for all APIs. It verifies:

- Token was minted by a trusted Lighthouse issuer
- Token has not been modified (signature)
- Token is live `nbf` and not expired `exp`
- Audience matches expected value(s)
The response will include the token's validity, claims, scopes, and more. The response is described in detail in the service's OpenAPI specification.

Lighthouse provides a guided walk-through of the token-validation service.

An example request and response for a patient’s health API token is shown below.

!!! info
    - Request values such as aud and response values will vary for your API and use cases.

```bash title="Sample Request"
curl \
  --request POST \
  --header "apikey: $KEY" \
  --header "Authorization: Bearer $ACCESS_TOKEN" \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'aud=https://sandbox-api.va.gov/services/fhir' \
  'https://sandbox-api.va.gov/internal/auth/v2/validation'
```

```json title="Sample Response"
{
  "data": {
    "id": "AT.ixr9-_pfifmbktxnx1POTokNhLEDgUuyfA3gkt7zkIM.oars1eevwquopfhnn2p6",
    "type": "validated_token",
    "attributes": {
      "ver": 1,
      "jti": "AT.ixr9-_pfifmbktxnx1POTokNhLEDgUuyfA3gkt7zkIM.oars1eevwquopfhnn2p6",
      "iss": "https://deptva-eval.okta.com/oauth2/default",
      "aud": "api://default",
      "iat": 1617636934,
      "exp": 1617640534,
      "cid": "0oa5l33ab7tj62Zpv2p7",
      "uid": "0oa5l33ab7tj62Zpv2p7",
      "scp": [
        "offline_access",
        "launch/patient",
        "patient/Patient.read",
        "openid",
        "profile"
      ],
      "sub": "b24346a788c04dfea5048d44ad071181",
      "act": {
        "icn": "1013657145V762143",
        "type": "patient",
      },
      "launch": {
        "patient": "1013657145V762143"
      }
    }
  }
}
```

## Self-verification

If the bearer token format is a signed JWT, you may opt to validate the token yourself. If you choose to do this, you should implement a key-caching strategy and are responsible for sufficient validation that the:

- Token was minted by a trusted Lighthouse issuer
- Token has not been modified (signature)
- Token is live (nbf) and not expired (exp)
- Audience matches expected value(s)

The signing keys and expected issuer can be retrieved from the provided metadata.
