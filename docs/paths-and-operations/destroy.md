# Destroy (DELETE)

!!! warning "Requirement"
    - The operation **must** use the `DELETE` method when detroying resources.
    - The operation **must not** include a request body.
    - The operation **should** return any errors inside an `errors` object.
    - The operation **must** return a `404` status code if the resource can't be found.

!!! success "Guidance"
    - Success operations that _include_ a status in the body **should** return a `200` status code.
    - Success operations that _do not include_ a status in the body **should** return a `204` status code.

## Example request

```json title="DELETE ../rx/v0/prescriptions/1c2dfaaa-4eb9-482e-86a9-4e7274975967"
// No request body 
```

## Example response

```json title="204 No Content"
// No response body 
```
