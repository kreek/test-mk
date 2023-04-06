# Standard Field Names

Providers are free to name fields as they see fit; however, to promote consistency across APIs, consider using names from the table below for fields with concepts similar to your own.

| Name       | Type | Description                                                                     |
|------------|-----------|---------------------------------------------------------------------------------|
| icn        | string    | A Master Person Index (MPI) Integration Control Number                          |
| birlsId    | string    | Beneficiary Identification and Records Locator (Sub)System Identifier           |
| edipi      | string    | A Department of Defense (DoD) Electronic Data Interchange Personal Identifier   |
| mhvId      | string    | My HealtheVet Identifier                                                        |
| secId      | string    | EAuth Universally Unique Identifier                                             |
| vet360Id   | string    | A VA Profile Vet360 Identifier                                                  |
| createDate | datetime  | The datetime a resource was created                                             |
| updateDate | datetime  | The datetime a resource was updated                                             |
| deleteDate | datetime  | The datetime a resource was (soft) deleted                                      |
| startDate  | datetime  | The beginning of a datetime range                                               |
| endDate    | datetime  | The end of a datetime range                                                     |
| page       | integer   | The current paginated page                                                      |
| pageSize   | integer   | The number of items in a paginated page                                         |
| totalSize  | integer   | The total of number of items regardless of pagination                           |
| first      | string    | The first page of a paginated list                                              |
| last       | string    | The last page of a paginated list                                               |
| prev       | string    | The previous page of a paginated list                                           |
| next       | string    | The next page of a paginated list                                               |
| filter     | string    | How a list is filtered (for example, filter=createDate>2012-08-17T21:51:08Z)    |
| sort       | string    | How a list is sorted (for example, sort=age,name)                               |
