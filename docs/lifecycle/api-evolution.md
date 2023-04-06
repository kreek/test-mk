# API Evolution

API Evolution is an enhancement strategy for APIs where major versioning is unnecessary to fulfill a backward-compatible contract as long as you are **adding rather than removing** endpoints, fields, and query parameters.

!!! warning "Requirement"
    - To be non-breaking, and not require versioning, additions **must** be optional, meaning that all the endpoints function as before and do not break if a consumer application ignores the recent changes.

!!! success "Guidance"
    - Providers **should** design APIs in a forward and extensible way to maintain compatibility and avoid duplication of resources, functionality, and excessive versioning.


## Extensibility

An evolvable API is extensible, but designing for extensibility takes special care and forethought since it's possible for early design decisions to make later extensibility of an API more difficult.

The following practices are not extensible:

- Ordered query or request body parameters 
- Returning one error rather an array of errors in responses
- Flat data in request and response bodies

For example, if a response to a payment history API returned a flat list of payment identifiers, like what is below, then adding additional data--without versioning--would be impossible. 

```json
{
  "data": [
    "37a6c0b9-6033-484f-a707-84649e5c7c35",
    "2c7a317a-40d0-4ec7-92ef-df954a6f2818",
  ]
}

```
If we later want to return a payment date there is nowhere to attach it. A more extensible solution is to wrap data in an object with named fields. Using a resource in the response is even safer. For example: 
```json
{
  "data": [
    {
      "id": "37a6c0b9-6033-484f-a707-84649e5c7c35",
      "type": "PaymentHistory",
      "attributes": {
        "amount": "$3,444.70",
        "date": "2019-12-15T00:00:00Z",
      }
    },
    {
      "id": "2c7a317a-40d0-4ec7-92ef-df954a6f2818",
      "type": "PaymentHistory",
      "attributes": {
        "amount": "$3,444.70",
        "date": "2019-12-31T00:00:00Z",
      }
    }
  ]
}
```
### Extensible names
Be specific and detailed when choosing names to avoid future limitations. While an endpoint in your API may initially only return one (mailing) address, like this: 

```json
{
  "address": {
    "street": "3700 North Capitol Street NW #558",
    "city": "Washington",
    "state": "DC",
    "postalCode": "20011"
  }
}
```

It may need to return both a residential and a mailing address in the future. Using extensible names at the start would allow an easier shift to a response like this: 

```json
{
  "residentialAddress": {
    "street": "3700 North Capitol Street NW #558",
    "city": "Washington",
    "state": "DC",
    "postalCode": "20011"
  },
  "mailingAddress": {
    "street": "511 10th St NW",
    "city": "Washington",
    "state": "DC",
    "postalCode": "20004"
  }
}
```

## Adding new endpoints
!!! success "Guidance"
    - Adding a new endpoint or a method to an existing path is an evolutionary change. Incrementing the major version in these cases is **not recommended**.

Consumers or product owners may consider *multiple* new endpoints a major change. Still, it is up to the provider to decide if the major version needs to be bumped.

## Evolving requests
!!! warning "Requirement"
    - There **must** NOT be any change in the HTTP verbs (such as GET, POST, and so on) supported by an existing URI.
    - Query-parameters **must** be unordered.
    - New query parameters appended to URIs **must** be optional.


### Renaming endpoint paths

If an endpoint's path needs to be renamed, a provider should do so in an additive manner so that it is effectively a new endpoint with the same functionality. The provider should keep and mark the old endpoint as [deprecated](../deprecation).

### Adding query parameters

While providers should only do so in moderation, adding optional query parameters can avoid versioning an API due to relatively minor changes in functionality. 

For example, if we wanted to add pagination to a resource collection endpoint that was previously not paged, such as:

```
../v0/claims
```

then we could append optional `pageNumber` and `pageSize` query parameters to the request, like this:

```
../v0/claims?pageNumber=2&pageSize=10
```

As long as the original call signature of the endpoint functions as before, meaning it continues not to page without the new query params, then no versioning is necessary.

## Evolving responses

!!! warning "Requirement"
    - There **must** NOT be any change in the HTTP status codes returned by the URIs.
    - There **must** NOT be any change in the name and type of the request or response headers of an URI.
    - New headers **must** be optional.


### Adding fields

As shown in the [Extensibility](#extensibility) section above, adding fields as long as they are within an object would not require versioning because adding fields is an additive change, which existing consumer apps could ignore. For example: 

*Before the addition*

```json
{
    "id": "2c7a317a-40d0-4ec7-92ef-df954a6f2818",
    "type": "PaymentHistory",
    "attributes": {
      "amount": "$3,444.70",
      "date": "2019-12-31T00:00:00Z"
    }
}
```

*After the addition*

```json
{
    "id": "2c7a317a-40d0-4ec7-92ef-df954a6f2818",
    "type": "PaymentHistory",
    "attributes": {
      "amount": "$3,444.70",
      "date": "2019-12-31T00:00:00Z",
      "paymentMethod": "DIRECT_DEPOSIT"
    }
}
```

### Renaming fields

If a field needs to be renamed, you should do so in an additive manner and not remove the old field. Instead, mark the old field as [deprecated](../deprecation). 

While this results in some duplication over the sunset period for the old name, it will not break applications still expecting that field.
For example, if a provider needed to add a second date to an object that already had a date field, you could rename the old one for clarity.

*Adding ‘approvalDate’ and renaming ‘date’ to ‘paymentDate’*

```json
{
    "id": "2c7a317a-40d0-4ec7-92ef-df954a6f2818",
    "type": "PaymentHistory",
    "attributes": {
      "amount": "$3,444.70",
      "date": "2019-12-31T00:00:00Z", // marked as deprecated in the OAS
      "approvalDate": "2019-07-31T00:00:00Z",
      "paymentDate": "2019-12-31T00:00:00Z"
    }
}
```
