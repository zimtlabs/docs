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

# Analytics

After every object of objects above is created, updated, minted, deployed a method for analytics is called.

We pass as parameters the object(required), collection(optional, collection name) and additional options(optional).

With the object’s receipt organization\_id, received\_at and collection the method checks if analytics of every type exist.

If the analytics for that type **exists**, we update the analytic document.

* We increment the value by 1
* We update latest\_timestamp with received\_at value of the object(param)

If the analytics for that type **don’t exist** we create a new analytic with that type for the object passed to the method.

**Finally** the object passed to the analytics method is updated that the analytics are resolved, in object.hub\_meta.analytics\_resolved: true.

#### Usage:

Analytics are used for plan limit checking, we can always get all analytics for a specific collection and type, of an organization and see the value of that analytic object and compare it to the plans limits for that collection.

With analytics we can also use dates, timestamps and values to display charts, tables, graphs and how many of the limits the organization has used.

{% swagger method="get" path="/analytics" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":[
      {
         "_id":"string",
         "type":"hour",
         "action": "create",
         "object": "collections",
	 "organization_id": "0x123",
	 "date": "2022-12-18T13",
	 "created_at": 1671370170,
	 "value": 1,
	 "meta": {
	      "latest_timestamp": 1671371019769
	   }
      }
   ],
   "status":"string",
   "pagination":{
      "next":"string",
      "previous":"string",
      "hasNext":"boolean",
      "size":"number",
      "skip":"number",
      "limit":"number"
   }
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Unuthorized request" %}
```json
{
    "status": "error",
    "message": "Unauthorized"
}
```
{% endswagger-response %}
{% endswagger %}

<details>

<summary>Body schema</summary>

```json
{
   "query":{
      "assets":[
      [
         {
            "field":"string",
            "operator":"string",
            "value":"any"
         }
      ], 
      []
      ]
   },
   "query_settings":{
      "type":"$and",
       "types":["$and", "$or"]
   },
   "limit":11,
   "skip":0,
   "sortAscending":false
}
```

</details>

<details>

<summary>Data object description</summary>

**\_id:**

* mongodb id

**action:**

* which action of the above is successfully executed
* Of the actions listed for now we are using “create” | “mint” | “deploy”
  * “get” is takes a lot of effort to implement since page refresh will trigger the analytics update/creation
  * “update” not implemented because there was no use for it

**object:**

* database collection name of the entities the actions are applied to

**type:**

* when the action is executed

**organization\_id:**

* id of the organization object from the database

**account\_id:**

* id of the account object from the database

**date:**

* ISO string i.e. “2022-12-18T13”
* depending on the type it can be:
  * type: year, date: “2022”
  * type: month, date: “2022-12”
  * type: day, date: “2022-12-18”
  * type: hour, date: “2022-12-18T13”
  * type: total, date: null

**created\_at:**

* EPOCH timestamp in milliseconds

**value:**

* count of how many actions we executed on an object(collection)

**meta**(optional)**:**

* created\_at: EPOCH timestamp in milliseconds when the analytic object is created
* latest\_timestamp: EPOCH timestamp in milliseconds when the analytic object is updated

</details>

***
