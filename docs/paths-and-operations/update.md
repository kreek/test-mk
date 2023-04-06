# Update (PUT, PATCH)

## Full resource updates

!!! warning "Requirement"
    - The operation **must** use `PUT` for a full resource update.
    - Success operations **must** return a `200` status code.
    - The operation **must** return a `404` status code if the resource can not be found.

!!! success "Guidance"
    - `PUT` **should** be idempotent, meaning that calling it several times should return the same result.
    - The operation **should** return the resource inside a `data` object.
    - The operation **should** return errors inside an `errors` object.
    - The operation **should** return a `400` for _syntax_ data errors (such as invalid JSON).
    - The operation **should** return a `422` for _semantic_ data errors (such as failing application validation).

### Example request

```json title="PUT ../rx/v0/prescriptions/1c2dfaaa-4eb9-482e-86a9-4e7274975967"
{
  "data": {
    "type": "Prescription",
    "attributes": {
      "prescriptionNumber": "1239877",
      "prescriptionName": "IBUPROFEN 200MG TAB",
      "facilityName": "DAYT30",
      "stationNumber": "990",
      "orderedDate": "2049-07-22T02:29:00Z",
      "expirationDate": "2050-07-22T02:49:00Z",
      "dispensedDate": "2049-07-23T011:17:00Z",
      "quantity": 20,
      "isRefillable": false
    },
  }
}
```

### Example response

```json title="200 OK"
{
  "data": {
    "type": "Prescription",
    "attributes": {
      "prescriptionNumber": "1239877",
      "prescriptionName": "IBUPROFEN 200MG TAB",
      "facilityName": "DAYT30",
      "stationNumber": "990",
      "orderedDate": "2049-07-22T02:29:00Z",
      "expirationDate": "2050-07-22T02:49:00Z",
      "dispensedDate": "2049-07-23T011:17:00Z",
      "quantity": 20,
      "isRefillable": false
    },
  }
}
```

### Example error

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

## Partial resource updates
Web frameworks differ on which HTTP verb to use for partial resource updates, with some defaulting to `PUT` for partials updates rather than the REST-prescribed `PATCH`. Providers should prefer `PATCH`, but this is not a strict requirement.

!!! warning "Requirement"
    - On success, the operation must return a `200` status code.
    - The operation **must** return a `404` status code if the resource can not be found.

!!! success "Guidance"
    - The operation **should** use `PATCH` for a partial resource update.
    - With `PUT` or `PATCH` it **should** be idempotent, meaning that calling it more than once will not change the result.
    - Only the fields being updated **should** be included in the request body.
    - The operation **should** return the resource inside a `data` object.
    - The operation **should** return errors inside an `errors` object.
    - The operation **should** return a `400` for _syntax_ data errors (such as invalid JSON).
    - The operation **should** return a `422` for _semantic_ data errors (such as failing application validation).

### Example request

```json title="PATCH ../rx/v0/prescriptions/1c2dfaaa-4eb9-482e-86a9-4e7274975967"
{
  "data": {
    "type": "Prescription",
    "attributes": {
      "isRefillable": true
    },
  }
}
```

### Example response

```json title="200 OK"
{
  "data": {
    "type": "Prescription",
    "attributes": {
      "prescriptionNumber": "1239877",
      "prescriptionName": "IBUPROFEN 200MG TAB",
      "facilityName": "DAYT30",
      "stationNumber": "990",
      "orderedDate": "2049-07-22T02:29:00Z",
      "expirationDate": "2050-07-22T02:49:00Z",
      "dispensedDate": "2049-07-23T011:17:00Z",
      "quantity": 20,
      "isRefillable": true
    },
  }
}
```

### Example error

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
