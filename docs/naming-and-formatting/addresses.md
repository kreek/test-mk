# Addresses

APIs are free to format data, including address data, as best fits the needs of their consumers. However, if providers are looking for a suggested schema for an address data type, we suggest following the one the VA Profile team uses.

The schema has separate fields for international country subdivisions (`province` rather than `state`), and postal codes (`internationalPostalCode` rather than `zipcode`).

```json
{
  "$schema": "http://json-schema.org/draft-04/schema",
  "type": "object",
  "required": [
    "addressLine1",
    "addressLine2",
    "addressLine3",
    "addressPou",
    "addressType",
    "city",
    "internationalPostalCode",
    "province",
    "stateCode",
    "zipCode",
    "zipCodeSuffix"
  ],
  "properties": {
    "id": {
      "type": "integer"
    },
    "addressLine1": {
      "type": "string"
    },
    "addressLine2": {
      "type": ["string", "null"]
    },
    "addressLine3": {
      "type": ["string", "null"]
    },
    "addressPou": {
      "type": "string"
    },
    "addressType": {
      "type": "string"
    },
    "city": {
      "type": "string"
    },
    "countryCodeIso3": {
      "type": "string"
    },
    "internationalPostalCode": {
      "type": ["string", "null"]
    },
    "province": {
      "type": ["string", "null"]
    },
    "stateCode": {
      "type": "string"
    },
    "zipCode": {
      "type": "string"
    },
    "zipCodeSuffix": {
      "type": ["string", "null"]
    }
  }
}
```

## Country codes
!!! success "Guidance"
    - Country codes **should** use [ISO 3166-1 alpha-3](https://www.iso.org/obp/ui/#search).
    - All other country subdivisions (Canadian provinces, Mexican states, UK counties, etc.) **should** also use the second part of their [ISO 3166-2 code](https://www.iso.org/obp/ui/#search).

## US state and territory codes
!!! success "Guidance"
    - US state codes **should** use the USPS postal abbreviation, which is the second part of a [ISO 3166-2:US](https://www.iso.org/obp/ui/#iso:code:3166:US) code.

Use the two letter United States Postal Service abbreviation for US states, the District of Columbia, US territories, Air/Army Post Office (APO) and Fleet Post Office (FPO).

### States
| Name           | Abbreviation  |
|----------------|---------------|
| Alaska         | AK            |
| Alabama        | AL            |
| Arkansas       | AR            |
| Arizona        | AZ            |
| California     | CA            |
| Colorado       | CO            |
| Connecticut    | CT            |
| Delaware       | DE            |
| Florida        | FL            |
| Georgia        | GA            |
| Hawaii         | HI            |
| Iowa           | IA            |
| Idaho          | ID            |
| Illinois       | IL            |
| Indiana        | IN            |
| Kansas         | KS            |
| Kentucky       | KY            |
| Louisiana      | LA            |
| Massachusetts  | MA            |
| Maryland       | MD            |
| Maine          | ME            |
| Michigan       | MI            |
| Minnesota      | MN            |
| Missouri       | MO            |
| Mississippi    | MS            |
| Montana        | MT            |
| North Carolina | NC            |
| North Dakota   | ND            |
| Nebraska       | NE            |
| New Hampshire  | NH            |
| New Jersey     | NJ            |
| New Mexico     | NM            |
| Nevada         | NV            |
| New York       | NY            |
| Ohio           | OH            |
| Oklahoma       | OK            |
| Oregon         | OR            |
| Pennsylvania   | PA            |
| Rhode Island   | RI            |
| South Carolina | SC            |
| South Dakota   | SD            |
| Tennessee      | TN            |
| Texas          | TX            |
| Utah           | UT            |
| Virginia       | VA            |
| Vermont        | VT            |
| Washington     | WA            |
| Wisconsin      | WI            |
| West Virginia  | WV            |
| Wyoming        | WY            |

### Districts
| Name                 | Abbreviation |
|----------------------|--------------|
| District of Columbia | DC           |

### Territories
| Name                     | Abbreviation |
|--------------------------|--------------|
| American Samoa           | AS           |
| Guam                     | GU           |
| Northern Mariana Islands | MP           |
| Puerto Rico              | PR           |
| U.S. Virgin Islands      | VI           |

### Military AFO/FPOs
| Name                                                 | Abbreviation |
|------------------------------------------------------|--------------|
| Armed Forces Americas                                | AA           |
| Armed Forces Europe, Canada, Middle East, and Africa | AE           |
| Armed Forces Pacific                                 | AP           |
