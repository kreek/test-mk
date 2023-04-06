# Create (POST)

!!! warning "Requirement"
    - Endpoints **must** use the `POST` method when creating resources without a consumer-supplied ID.
    - The operation **should** return a `201` with the created resource reflected in the response body.

!!! success "Guidance"
    - `POST` **should not** be idempotent; meaning, if the request is sent again, a second resource should be created.
    - The operation **should** return the created resource inside a `data` object.
    - The operation **should** return any errors inside an `errors` object.
    - The operation **should** return a `400` for _syntax_ data errors (such as invalid JSON).
    - The operation **should** return a `422` for _semantic_ data errors (such as failing application validation).


In most cases, the service generates an identifier for the resource. In cases where an identifier is supplied by the API consumer, follow the guidance for [creating a resource with a consumer supplied identifier](#create-with-a-consumer-supplied-identifier).


## Example request

```json title="POST ../rx/v0/prescriptions"
{
  "data": {
    "type": "Prescription",
    "attributes": {
      "prescriptionNumber": "1239876",
      "prescriptionName": "IBUPROFEN 400MG TAB",
      "facilityName": "DAYT29",
      "stationNumber": "989",
      "orderedDate": "2049-07-21T01:39:00Z",
      "expirationDate": "2050-07-21T01:39:00Z",
      "dispensedDate": "2049-07-22T010:07:00Z",
      "quantity": 30,
      "isRefillable": true
    },
  }
}
```

## Example response

For returning a 201 Created HTTP status code, the response body would confirm the resource had been created as shown below (in JSON::API format).

```json title="201 Created"
{
  "data": {
    "type": "Prescription",
    "id": "1c2dfaaa-4eb9-482e-86a9-4e7274975967",
    "attributes": {
      "prescriptionNumber": "1239876",
      "prescriptionName": "IBUPROFEN 400MG TAB",
      "facilityName": "DAYT29",
      "stationNumber": "989",
      "orderedDate": "2049-07-21T01:39:00Z",
      "expirationDate": "2050-07-21T01:39:00Z",
      "dispensedDate": "2049-07-22T010:07:00Z",
      "quantity": 30,
      "isRefillable": true
    }
  }
}
```

## Example error

```json title="422 Unprocessable Entity"
{
  "errors": [
    {
      "status": "422",
      "source": { "pointer": "/data/attributes/isRefillable" },
      "title":  "Invalid Attribute",
      "detail": "isRefillable must be a boolean value."
    }
  ]
}
```

## Create with a consumer-supplied identifier

Creating resources works differently when the consumer is supplying the identifier versus when the system is generating the identifier upon creation.

!!! success "Guidance"
    - A `PUT` method **should** be used, as the operation is idempotent even during creation.
    - On successful *creation* a `201` **should** be returned with the created resource in the body.
    - If the result is an *update* of an existing resource, a `204` **should** be returned with no response body.
    - The operation **should** return any errors inside an `errors` object.
    - The operation **should** return a `400` for _syntax_ data errors (such as invalid JSON).
    - The operation **should** return a `422` for _semantic_ data errors (such as failing application validation).

