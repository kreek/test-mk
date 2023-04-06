# Read (GET)

## Collections

!!! warning "Requirement"
    - The operation **must** use an HTTP `GET` verb when fetching collections.
    - Successful responses **must** return a `200` status code.
    - An empty list is a successful response and **must** return a `200` status code.

!!! success "Guidance"
    - The operation **should** return resources inside a `data` object.
    - The operation **should** return any errors inside an `errors` object.
    - Supplemental information, such as pagination, **should** be in a `meta` object.

A collection resource should return a list of representation of all of the given resources (instances), including any related metadata. An array of resources should be in the items field. Consistent naming of collection resource fields allows API clients to create generic handling for using the provided data across various resource collections.

The GET verb should not affect the system, and should not change the response on subsequent requests unless the underlying data changes (as in, it should be idempotent). Exceptions to 'changing the system' are typically instrumentation/logging-related. The list of data should be filtered based on the privileges available to the API client, so that it lists only the resoures for which the client has the authorization to view and not all the resources in the domain. Providing a summarized or minimized version of the data representation can reduce the bandwidth footprint in cases where individual resources contain a large object.

### Example request

```json title="GET ../rx/v0/prescriptions"
// No request body 
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


## Single resource

!!! warning "Requirement"
    - The operation **must** use a `GET` verb when fetching single resources.
    - Successful responses **must** return a `200` status code.
    - The operation **must** return a `404` status code if the resource can not be found.

!!! success "Guidance"
    - The operation **should** return the resource inside a `data` object.
    - The operation **should** return any errors inside an `errors` object.

A single resource is typically derived from the parent collection of resources and is often more detailed than an item in the representation of a collection resource. Executing GET should never affect the system and should not change the response on subsequent requests (as in, it should be idempotent).

All identifiers for sensitive data should be non-sequential and preferably non-numeric. In scenarios where this data might be used as a subordinate to other data, immutable string identifiers should be used for easier readability and debugging (such as, `NAME_OF_VALUE` vs `1421321`).

### Example request

```json title="GET ../rx/v0/prescriptions/1c2dfaaa-4eb9-482e-86a9-4e7274975967"
// No request body
```

### Example response

If the resource with that id is found, return a `200` 'OK' status code. The response body should include the resource type and id as shown below in JSON::API format.

```json title="200 OK"
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

If the resource is not found, return a `404` 'Not Found' status code.

```json title="404 Not Found"
{
  "errors": [
    {
      "status": "404",
      "title":  "Not Found",
      "detail": "The requested resource could not be found."
    }
  ]
}
```
