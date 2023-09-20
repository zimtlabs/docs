---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# ⚙ Requests

***

### Environment

To make a request, Hub2 accept following HTTP methods <mark style="color:blue;">`GET`</mark>, <mark style="color:red;">`POST`</mark>, <mark style="color:orange;">`PUT`</mark>, <mark style="color:yellow;">`PATCH`</mark>, or <mark style="color:purple;">`DELETE`</mark>

The URL to the API service:

| Environment     | URL                   | APP URL               |
| --------------- | --------------------- | --------------------- |
| ~~Development~~ | ~~dev-api.zi.mt~~     | ~~app-dev.zi.mt~~     |
| ~~Staging~~     | ~~staging-api.zi.mt~~ | ~~app-staging.zi.mt~~ |
| Production      | api.zi.mt             | app.zi.mt             |

Optionally, you can include query parameters on <mark style="color:blue;">`GET`</mark> requests.

***

### Headers

Headers are required for each request.

<mark style="color:blue;">`GET`</mark> - query params

<mark style="color:red;">`POST`</mark> - body data

<mark style="color:orange;">`PUT`</mark> - query params & body\_params

<mark style="color:yellow;">`PATCH`</mark> - body params

<mark style="color:purple;">`DELETE`</mark> - id parameter in the /url/`:id`/ usually response `204`

<table><thead><tr><th width="175.33333333333331">Type</th><th width="283">Value</th><th>Description</th></tr></thead><tbody><tr><td>Accept</td><td>Accept: application/</td><td>The response format, which is required for operations with a response body “json”</td></tr><tr><td>Authorization</td><td>Authorization: ZIMT_TOKEN</td><td>JWT token with user details</td></tr><tr><td>Content-Type</td><td>Content-Type: application/json</td><td>The request format, which is required for operations with a request body “json”</td></tr></tbody></table>

***

### Query parameters - ? (check)

The most `GET` requests can have one or more query parameters on the request **URI** that will be used to filter, sorting or pagination.

<table><thead><tr><th width="133">Parameter</th><th width="92.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>string</td><td></td></tr><tr><td>query</td><td>object</td><td>{ assets: [ { field: 'object.meta.created_by', operator: 'starts-with', value: '0x123' } ] }</td></tr></tbody></table>

***

### Query operators

| Parameter          | Type                  | Description                                             |
| ------------------ | --------------------- | ------------------------------------------------------- |
| equal              | string, number        | Property must be equal to value                         |
| not-equal          | string, number        | Property must be different then value                   |
| exists             | boolean               | Value is true or false                                  |
| greater-than       | number                | Value is greater then property value                    |
| greater-than-equal | number                | Value is greater or equal to property value             |
| less-than          | number                | Value is less then property value                       |
| less-than-equal    | number                | Value is less or equal to property value                |
| array-some         | array\[string,number] | Property is array and contain some from value array     |
| array-all          | array\[string,number] | Property is array and contain all the values from array |
| starts-with        | string                | Property starts with string in value                    |
| contains           | string                | Property that contain phrase from value                 |

***

**Query parameters example.**

```json
{
   "query":{
      "assets":[
         [
            {
               "field":"object.meta.created_by",
               "operator":"starts-with",
               "value":"0x123"
            }
         ],
         []
      ]
   },
   "limit":10,
   "query_settings":{
      "type":"$and"
      "types": ["$and", "$or"]
   },
}
```

```json
Get all records
{
   "query":{
      "assets":[[], []]
   },
   "query_settings":{
      "type":"$and"
      "types": ["$and", "$or"]
   },
   "limit":11,
   "skip":0,
   "sortAscending":false
}

Filter by property
{
   "query":{
      "assets":[
         {
            "field":"data.name",
            "operator":"contains",
            "value":"test"
         },
         {
            "field":"data.properties.description",
            "operator":"contains",
            "value":"test"
         },
         {
            "field":"data.properties.tags",
            "operator":"contains",
            "value":"test"
         },
         {
            "field":"data.properties.location",
            "operator":"contains",
            "value":"test"
         }
      ], []
   },
   "query_settings":{ // default query settings
      "type":"$or",     
      "types": ["$or", "$and"]
   },
   "limit":11,
   "skip":0,
   "sortAscending":false
}

Complex query - Mix multiplre querues

{
   "query":{
      "assets":[
         [ 
            {
               "field":"data.name",
               "operator":"contains",
               "value":"test"
            },
            {
               "field":"data.properties.description",
               "operator":"contains",
               "value":"test"
            },
            {
               "field":"data.properties.tags",
               "operator":"contains",
               "value":"test"
            },
            {
               "field":"data.properties.location",
               "operator":"contains",
               "value":"test"
            }
         ], // operator `$or` from query_settings[types][index] will be applied on the items in the array
         [
            {
               "field":"data.exclusive",
               "operator":"equal",
               "value":true
            }
         ] // operator `$and` from query_settings[types][index] will be applied to the items in the array
      ] 
// operator `$and` from query_settings[type] will be appliad to the main query
   },
   "query_settings":{
      "types":[
         "$or",
         "$and" 
      ],
      "type":"$and"
   },
   "limit":11,
   "skip":0,
   "sortAscending":false
}
```

### Sort / order options

* `default` → If id is defined, last\_created
* `last_modified` → sort by the date and time when the record was last modified.
* `last_opened` → sort by the date and time when the record was last opened.
* `last_created` → sort by the date and time when the record was created.
* `alphabetically` → sort by the records by name (alphabetically).
* `asc` / `desc` → ascending / descending

Example:

?**order\_by**=desc

***

### Pagination

_**page**_

`page`=1 and `page_limit`=30 returns the first 30 records.

`page`=2 and `page_limit`=30 returns the next 30 records. Min value: 1.

Max value: 1000

_**page\_limit**_

Minimum value: **1**.

Maximum value: **100**.

Default page limit **30** records.

Parameters

<table><thead><tr><th width="180">Parameter name</th><th width="121">Type</th><th width="88">Default</th><th>Description</th></tr></thead><tbody><tr><td>page_limit</td><td>number</td><td>30</td><td>The maximum number of records to return in the response</td></tr><tr><td>page</td><td>number</td><td>1</td><td>Page number</td></tr><tr><td>order_by</td><td>string</td><td>desc</td><td></td></tr></tbody></table>
