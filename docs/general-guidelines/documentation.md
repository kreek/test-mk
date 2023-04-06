# Documentation

!!! warning "Requirement"
    - Providers **must** document their API using the OpenAPI Specification version 3.0.x
    - OpenAPI docs **must** be valid YAML or JSON so they are machine-readable.

## OpenAPI Specification
OpenAPI Specification (OAS) is the predominant API documentation standard in the industry. 

All providers on Lighthouse must document their APIs using the OpenAPI Specification as a requirement of onboarding. Version 3.0.3 of the OpenAPI Specification can be found [here](https://swagger.io/specification/).

### Documentation for humans and computers

An OAS serves as documentation for human consumers to read when evaluating or using an API and as a machine-readable input for Lighthouse tooling that verifies consistency between the API’s specification and its behavior.
                                          
OAS docs must be valid in [Swagger Editor](https://swagger.io/tools/swagger-editor/).

#### Lighthouse OAS Subjective Rules
OAS docs also undergo a subjective review for items we can't automate or that are open to interpretation. Below is a list of the subjective rules found within this guide:

???+ success "Security"

    - Endpoints that return PII or PHI **must** use OAuth.
    - Endpoints secured with OAuth **must** list the required scopes in the security section of each endpoint.

???+ success "Headers"

    - APIs **must not** use headers to communicate business logic or service logic (such as paging response info or PII query parameters).
    - HTTP headers **should** only be used for the purpose of handling cross-cutting concerns such as authorization.

???+ success "Resource Operations"

    - `GET` Endpoints must use the POST method when creating resources without a consumer-supplied ID.
    - `GET` operations should return a 201 with the created resource reflected in the response body.
    - `POST` **should not** be idempotent; meaning, if the request is sent again, a second resource should be created.
    - `PUT` **should** be idempotent, meaning that calling it several times should return the same result.
    - Operations **should** return the created resource inside a `data` object.
    - Operations **should** return any errors inside an `errors` object.
    - Providers **should** show that a custom operation was intentional by ending the path with a verb.

???+ success "Naming & Formatting"

    - Fields **should** use camelCase.
    - Acronyms **should** be camelCase rather than uppercase.
    - Providers **should** avoid abbreviations.
    - Booleans **should** be prefixed with an auxiliary verb (such as is, has, or can).



## Example OAS Document

Below is an example OAS doc for a fictitious 'Rx' API on Lighthouse. Click on the circular buttons labeled with a '+'  to view code annotations.

```yaml
openapi: 3.0.0 # (1)
info: # (15)
  title: Rx
  description: An example 'Rx' API that follows the [Lighthouse API Standards](https://department-of-veterans-affairs.github.io/lighthouse-api-standards).
  contact:
    name: VA.gov
  version: 1.0.0 
servers: # (3)
  - url: 'https://sandbox-api.va.gov/services/rx/{version}'
    description: Lighthouse API sandbox environment
    variables:
      version:
        default: v1
  - url: 'https://api.va.gov/services/rx/{version}'
    description: Lighthouse API production environment
    variables:
      version:
        default: v1
paths: 
  /pharmacies:
    get:
      tags:
        - pharmacy
      summary: Returns a list of facilities with pharmacies. # (16)
      description: Returns a paginated list of all VA facilities that provide pharmacological services.
      operationId: getPharmacies
      responses:
        '200':
          description: The veteran's prescriptions were successfully found and returned as an array.
          content:
            application/json:
              schema: # (4)
                $ref: '#/components/schemas/PharmacyList' 
        '500': # (5)
          $ref: '#/components/responses/ErrorInternalServerError'
        '502': # (6)
          $ref: '#/components/responses/ErrorInternalServerError'
        '503': # (7)
          $ref: '#/components/responses/ErrorServiceUnavailable'
      security: # (8)
        - {}
  /prescriptions:
    get:
      tags:
        - prescription
      summary: Returns a list of a veteran's prescriptions
      description: Given a veteran's ICN, the endpoint returns a list of their prescriptions.
      operationId: getPrescriptions
      parameters: # (17)
        - in: query
          name: icn
          description: MPI ICN
          required: true
          schema:
            maxLength: 17
            minLength: 17
            pattern: '^\d{10}V\d{6}$'
            type: string
            example: 1012667145V762142
      responses:
        '200':
          description: The veteran's prescriptions were successfully found and returned as an array.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/PrescriptionList'
        '401': # (18)
          $ref: '#/components/responses/ErrorUnauthorized'
        '403':
          $ref: '#/components/responses/ErrorForbidden'
        '404':
          $ref: '#/components/responses/ErrorNotFound'
        '422':
          description: Unprocessable Entity
          content:
            application/json:
              schema:
                type: object
                example:
                  errors:
                    - status: 422
                      title: Invalid ICN
                      detail: 'ICN must match \"^\\d{10}V\\d{6}$\", size must be exactly 17.'
        '500':
          $ref: '#/components/responses/ErrorInternalServerError'
        '502':
          $ref: '#/components/responses/ErrorBadGateway'
        '503':
          $ref: '#/components/responses/ErrorServiceUnavailable'
      security:
        - bearerToken: # (12)
          - prescription.read # (19)
components: # (9)
  securitySchemes: 
    bearerToken:
      type: http
      scheme: bearer
    production:
      type: oauth2
      description: This API uses OAuth 2 with the authorization code grant flow.
      flows:
        authorizationCode:
          authorizationUrl: https://api.va.gov/oauth2/authorization
          tokenUrl: https://api.va.gov/oauth2/token
          scopes:
            prescription.read: Retrieve prescription data
    sandbox:
      type: oauth2
      description: This API uses OAuth 2 with the authorization code grant flow.
      flows:
        authorizationCode:
          authorizationUrl: https://sandbox-api.va.gov/oauth2/authorization
          tokenUrl: https://sandbox-api.va.gov/oauth2/token
          scopes:
            prescription.read: Retrieve prescription data
  responses: # (10)
    ErrorUnauthorized:
      description: Unauthorized
      content:
        application/json:
          schema:
            type: object
            example:
              errors:
                - status: 401
                  title: Unauthorized
                  detail: Invalid credentials. The access token has expired.
    ErrorForbidden:
      description: Forbidden
      content:
        application/json:
          schema:
            type: object
            example:
              errors:
                - status: 403
                  title: Forbidden
                  detail: You do not have access to the requested resource.
    ErrorNotFound:
      description: Not Found
      content:
        application/json:
          schema:
            type: object
            example:
              errors:
                - status: 404
                  title: Not Found
                  detail: The requested resource could not be found.
    ErrorInternalServerError:
      description: Internal Server Error
      content:
        application/json:
          schema:
            type: object
            example:
              errors:
                - status: 500
                  title: Internal Server Error
                  detail: An internal API error occurred.
    ErrorBadGateway:
      description: Bad Gateway
      content:
        application/json:
          schema:
            type: object
            example:
              errors:
                - status: 502
                  title: Bad Gateway
                  detail: An upstream service the API depends on returned an error.
    ErrorServiceUnavailable:
      description: Service Unavailable
      content:
        application/json:
          schema:
            type: object
            example:
              errors:
                - status: 503
                  detail: An upstream service is unavailable.
  schemas:
    PharmacyList:
      type: object
      required:
        - data
      properties:
        data:
          type: array
          items:
            $ref: "#/components/schemas/Pharmacy" # (11)
    Pharmacy:
      type: object
      required:
        - type
        - id
        - attributes
      properties:
        type:
          type: string
          example: Pharmacy
        id:
          type: string
          example: 6e976911-2707-4018-a6db-0d1342326379
        attributes:
          type: object
          required:
            - id
            - name
            - city
            - state
            - cerner
            - clinics
          properties:
            id:
              type: string
              example: "358"
            name:
              type: string
              example: "Cheyenne VA Medical Center Pharmacy"
            city:
              type: string
              example: "Cheyenne"
            state:
              $ref: '#/components/schemas/State'
    PrescriptionList:
      type: object
      required:
        - data
      properties:
        data:
          type: array
          items:
            $ref: "#/components/schemas/Prescription"
    Prescription:
      type: object
      required:
        - type
        - id
        - attributes
      properties:
        type:
          type: string
          example: Prescription
        id:
          type: string
          example: db8a52f0-b3d2-4cc9-bcab-7053d88737d5
        attributes:
          type: object
          required:
            - productNumber
            - referenceDrug
            - brandName
            - activeIngredients
            - referenceStandard
            - dosageForm
            - route
          properties:
            productNumber:
              type: string
              example: '001'
            referenceDrug:
              type: boolean
              example: false
            brandName:
              type: string
              example: 'FAMOTIDINE PRESERVATIVE FREE'
            activeIngredients:
              type: object
              required:
                - name
                - strength
              properties:
                name:
                  type: string
                  example: "FAMOTIDINE"
                strength:
                  type: string
                  example: "10MG/ML"
            referenceStandard:
              type: boolean
              example: false
            dosageForm:
              type: string
              enum: ["INJECTABLE", "TABLET"] # (13)
              example: "TABLET"
            route:
              type: string
              enum: ["INJECTION", "ORAL"]
              example: "ORAL"
    State: # (14)
      type: string
      enum:
        - AK
        - AL
        - AR
        - AZ
        - CA
        - CO
        - CT
        - DE
        - FL
        - GA
        - HI
        - IA
        - ID
        - IL
        - IN
        - KS
        - KY
        - LA
        - MA
        - MD
        - ME
        - MI
        - MN
        - MO
        - MS
        - MT
        - NC
        - ND
        - NE
        - NH
        - NJ
        - NM
        - NV
        - NY
        - OH
        - OK
        - OR
        - PA
        - RI
        - SC
        - SD
        - TN
        - TX
        - UT
        - VA
        - VT
        - WA
        - WI
        - WV
        - WY
      example: "WY"
```

1. OpenAPI Spec 3.0.x is the required version. Version 3.1 can not be used yet as SwaggerUI does not support it at this time.
2. If you use JSON Schema to define your models, you can set the dialect globally through the `jsonSchemaDialect` property.
3.  The `servers` section should reflect your API's future external public-facing location when hosted on Lighthouse. All endpoints should then be relative to those base URLs. You can copy the example definition starting on this line (#9) to use in your OAS doc; remember to replace `rx` with your namespace.
4. Using `$ref` properties (references) to link to response body schema definitions helps keep your endpoint definitions short.
5. Errors happen. We require that all APIs let consumers know the error schema your API will return should your API encounter an unexpected error.
6. Many VA APIs rely on one or more upstream services for their data. If your API could throw a `500` if an upstream service returns an error, consider returning a `502` error instead, so you and the consumer know a dependency rather than the API is causing the issue.
7. If your API could throw a `500` if an upstream service is down, consider returning a `503` error instead, so you and the consumer know a dependency rather than the API is causing the issue.
8.  All endpoints **must** have a security property. In this example, the endpoint does not require authentication. However, the `security` tag is still included but it contains an empty object. This lets us know security was left out on purpose.
9.  Often, multiple API operations have some common parameters or return the same response structure. To avoid code duplication, you can place the common definitions in the global `components` section and reference them using [`$ref`](https://swagger.io/docs/specification/using-ref/).
10.  Responses, and specifically errors, are likely to be the same for all endpoints. Defining them within the `components/responses` can help keep down the size of your path definitions.
11.  `$refs` can have `$refs`.  Think of schemas like their database counterparts. Each model and its relations should be a distinct schema.
12. This endpoint returns a veteran's prescription history, which qualifies as Personal Health Information (PHI). All endpoints that return PII or PHI **must** use OAuth.
13.  Enums should be considered constants. As in 'C' style languages, they should be UPPER_CASE with underscores for spaces.
14. Components don't have to be resources. Any data that appears in multiple locations, such as a list of states, can be a component.
15. Info tags **must** include a `title` and a `description`.
16. Path methods **must** contain a shorter `summary` and a longer `description` that explains the purpose and function of each operation.
17. Requests should use URL or body parameters rather than headers to pass along requisite data unique to that endpoint.
18. Protected endpoints **must** define `401` Unauthorized and `403` Forbidden responses.
19. The `prescription.read` scope is required for this endpoint so it is listed in the security section.