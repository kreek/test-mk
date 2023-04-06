# Custom Operations

!!! warning "Requirement"
    - If the HTTP method chosen for the custom operation does not accept a request body (`GET` or `DELETE`), the operation **must not** send one.

!!! success "Guidance"
    - Providers **should** show that a custom operation was intentional by ending the path with a verb.
    - Custom operations with body parameters **should** default to the `POST` method.
    - Custom operations **should** not use the `PATCH` method.


Providers should choose from the 5 standard HTTP methods whenever workable, using custom operations only for custom functionality that falls outside of the uses of one of the standard methods.

## Example using GET

In this example of getting tracking information for a prescription, the request does not need to send any parameters. Therefore, a `GET` method is preferred as it is the standard REST method for reading a resource.

### Example response

```json title="POST .../rx/v1/prescriptions/1c2dfaaa-4eb9-482e-86a9-4e7274975967/track"
// No request body 
```

### Example response

```json title="200 OK"
{
  "data": {
    "type": "PrescriptionTracking",
    "id": "add84f99-f9ce-48f8-a56c-625868d11efc",
    "attributes": {
      "prescriptionId": "1c2dfaaa-4eb9-482e-86a9-4e7274975967",
      "prescriptionName": "IBUPROFEN 400MG TAB",
      "trackingNumber": "add84f99-f9ce-48f8-a56c-625868d11efc",
      "shippedDate": "2049-07-22"
      "deliveryDervice": "USPS"
      "trackingURL": "https://tools.usps.com/go/TrackConfirmAction?tRef=fullpage&tLc=2&text28777=&tLabels=add84f99-f9ce-48f8-a56c-625868d11efc%2C&tABt=false"
    }
  }
}
```

## Example using POST

In this example, which contains (mock) PII, we're sending a prescription search query that includes a mock SSN number in a `POST` body. The request side of the operation uses a custom `Query` resource that is not persisted in the system and can not be retrieved. However, the operation returns a response that would look almost identical to a `Prescription` resource collection's read operation.

### Example request

```json title="POST .../rx/v1/prescriptions/search"
{
  "data": {
    "type": "Query",
    "attributes": {
      "ssn": "777-98-7654",
      "quantity": ">10",
      "dispensedDate": ">2049-01-01T00:00:00Z AND <2050-01-01T00:00:00Z"
    }
}
```

### Example response

```json title="200 OK"
{
  "data": [
    {
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
    },
    {
      "type": "Prescription",
      "id": "ac9d4b3f-e4bd-49dd-b794-64ad05480729",
      "attributes": {
        "prescriptionNumber": "1239832",
        "prescriptionName": "ACETAMINOPHEN 200MG TAB",
        "facilityName": "DAYT29",
        "stationNumber": "989",
        "orderedDate": "2049-07-22T11:23:00Z",
        "expirationDate": "2050-07-22T11:30:00Z",
        "dispensedDate": "2049-07-23T012:35:00Z",
        "quantity": 30,
        "isRefillable": true
      }
    }
  ]
}
```
