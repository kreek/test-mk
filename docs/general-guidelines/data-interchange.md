# Data Interchange

!!! success "Guidance"
    - Lighthouse APIs **should** interchange data serialized as JSON.
    - APIs **may** return binary data for files (images, PDFs, etc).
    - XML **may** be used when interfacing with legacy systems.
    - Binary data formats (ProtoBufs, Avro, Thrift) are **not recommended** for public-facing APIs.


## JSON

!!! warning "Requirement"
    - JSON **must** conform to the JSON Data Interchange Format described in [RFC 7159](https://datatracker.ietf.org/doc/html/rfc7159)
    - Request and response bodies **must** be valid against [JSON Schema Draft v4](https://json-schema.org/specification.html) or higher.


As REST is the dominant web API architecture, JSON is the dominant web data-interchange format.

REST APIs revolve around resources. They are better described using JSON's structured data than a markup language that structures information, such as XML.

JSON's advantages over XML include:
 - Its shared data model, obviously with JavaScript, but also with any language that implements booleans, numbers, strings, arrays, and objects (or objects like data structures, i.e. dictionaries or hashes). 
 - It produces a smaller payload than XML. 
 - In most languages, it compresses (gzips) faster. 
 - is more human-readable than XML. 

## Information models

!!! success "Guidance"
    - We **recommend** that APIs default to using [JSON:API](https://jsonapi.org/) as an information model.
    - Industry-specific standards, such as [FHIR](http://hl7.org/fhir/) for health data **may** be used.
    - Information/hypermedia models such as OData, HAL, JSON-LD, and SIREN are **not recommended**.

An information model specifies a set of shared conventions for JSON data to ensure formatting is consistent across a set of APIs.

For example, even if APIs agree on using JSON, they could each still disagree on how they format and identify resources. Additionally, there are varied ways one could render related resources, metadata, errors, and design patterns such as pagination and filtering. With an information model in place, figuring out how to [render errors](https://jsonapi.org/format/#error-objects) or [paginate a list](https://jsonapi.org/format/#fetching-pagination) is a matter of looking up the recommendation from the model’s specification.
