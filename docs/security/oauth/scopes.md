# Scopes

[OAuth scopes](https://datatracker.ietf.org/doc/html/rfc6749#section-3.3) limit a consumer's access to an API. You will need to define the set of scopes your API supports, including:

- The scope’s name and display name
- A scope description
- Whether or not the scopes require user consent

It is possible to grant consumers access to a subset of scopes, depending on their needs. Scopes can be as broad or granular as needed; for example, `patient/Patient.write` granting access to write/update any patient data versus `patient/EmergencyContact.write` granting access to write or update a patient's emergency contact only.

You will need to validate that the access token has the scope(s) required for the requested resource. For more information, go to the [Token Validation](../token-validation) section. 

## Scope name

For the scope name, Lighthouse recommends one of the following naming conventions. 

| Naming Convention   | Format                                          | Example                    |
|---------------------|-------------------------------------------------|----------------------------|
| SMART-on-FHIR style | `<patient:user:system>/<resource>.<read:write>` | `patient/Observation.read` |
| Hierarchical style  | `<api>.<resource>.<action>`                     | `facilities.phone.manage`  |

## Scope display name

The scope display name will be shown to a user on a consent form. It should be written in plain language. For example, if your scope name is `patient/AllergyIntolerance.read` a good display name is:

> Allergies

## Scope description

The scope description is displayed to a user as help-text on a consent form. It should be written in plain language. For example, if your scope is `patient/AllergyIntolerance.read` a good description is:

> A list of any substances to which you have a negative reaction. Examples include pollen, gluten, or bee stings.

## User consent for scopes

Some users may be required to consent to scopes for the consuming application to access certain data. What we need from you is to know whether or not a user must consent to an application's request for this scope.

```
require consent: true
```
