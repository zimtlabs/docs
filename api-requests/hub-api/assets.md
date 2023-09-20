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

# Assets

An Asset is a crypto-secure, random, globally unique ID, for something whose ownership can be proven. It’s a placeholder with some data (immutable & mutable). It’s always signed & hashed, and every asset always belongs to exactly one organisation (the one who signed it).

{% hint style="danger" %}
How to generate the IDs offline as they’re signed by the hub? E.g. we can sign them only by the creator instead of hub key (only requires user/admin/device key)
{% endhint %}

All objects updated by the workers / server must be outside the mutable data.

***

## Asset model

{% tabs %}
{% tab title="Event" %}
```json
{
   "id": "string",
   "proof": "string",
   "object": "<Object>", // imutable
   "data": "<DataObject>", // mutable
   "receipt": "<ReceiptObject>",
   "ownership": "<OwnershipObject>"
}
```

<table><thead><tr><th width="134">Name</th><th width="203">Type</th><th width="98">Required</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>string</td><td>true</td><td>The unique ID of the asset.</td></tr><tr><td>proof</td><td>string</td><td>true</td><td>A crypto-secure proof of the asset's authenticity and ownership.</td></tr><tr><td>object</td><td><strong>&#x3C;Object></strong></td><td>true</td><td>The immutable data of the asset, including its type, name, collection ID, and other metadata.</td></tr><tr><td>data</td><td><strong>&#x3C;DataObject></strong></td><td>false</td><td>The mutable data of the asset, including its status, location, and other information that can be updated over time.</td></tr><tr><td>receipt</td><td><strong>&#x3C;ReceiptObject></strong></td><td>/</td><td>A receipt indicating that the asset has been received by the organization and its ownership has been established. Generated by the system</td></tr><tr><td>ownership</td><td><strong>&#x3C;OwnershipObject></strong></td><td>/</td><td>The ownership information for the asset, including the account ID of the owner. Generated by the system</td></tr></tbody></table>
{% endtab %}

{% tab title="Object" %}
**Object `<Object>`**

```json
{
  "meta": "<MetaObject>",
  "data": "<ObjectData>",
  "signature":"string",
}
```

<table><thead><tr><th width="121">Name</th><th width="155">Type</th><th width="99">Required</th><th>Description</th></tr></thead><tbody><tr><td>meta</td><td><strong>&#x3C;MetaObject></strong></td><td>true</td><td>The metadata for the asset, including the timestamp of its creation and the hash of its data.</td></tr><tr><td>data</td><td><strong>&#x3C;ObjectData></strong></td><td>true</td><td>The immutable data of the asset, including its type, name, collection ID, and other metadata.</td></tr><tr><td>signature</td><td>string</td><td>false</td><td>A signature that verifies the authenticity of the asset data.</td></tr></tbody></table>

**Meta object `<MetaObject>`**

```json
{
  "created_by": "string",
  "created_at": "number",
  "data_hash": "string"
}
```

<table><thead><tr><th width="141">Name</th><th width="100">Type</th><th width="103">Required</th><th></th></tr></thead><tbody><tr><td>created_by</td><td>string</td><td>true</td><td>The address of the creator of the asset.</td></tr><tr><td>created_at</td><td>number</td><td>true</td><td>The timestamp of the asset's creation.</td></tr><tr><td>data_hash</td><td>string</td><td>false</td><td>The hash of the asset's data.</td></tr></tbody></table>

**Data ( **<mark style="color:red;">**immutable**</mark>** ) `<ObjectData>`**

```json
{
   "type":"string",
   "name":"string",
   "collection_id":"string", // collection to which the asset belongs to
   "description":"string",
   "documents": "<DocumentArray>",
   "location": "<LocationObject>",
   "properties":"object",
   "identifiers": "<IdentifiersObject>",
}
```

<table><thead><tr><th width="141">Name</th><th width="190">Type</th><th width="101">Required</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>string</td><td>true</td><td>The type of the asset, formatted as a dot-separated string.</td></tr><tr><td>name</td><td>string</td><td>false</td><td>The name of the event.</td></tr><tr><td>description</td><td>string</td><td>false</td><td>A description of the event.</td></tr><tr><td>documents</td><td>Array <strong>&#x3C;Document></strong></td><td>false</td><td>Array of documents attached to this event.</td></tr><tr><td>identifiers</td><td>Object</td><td>false</td><td>Identifiers associated with the document.</td></tr><tr><td>location</td><td><strong>&#x3C;LocationObject></strong></td><td></td><td></td></tr><tr><td>properties</td><td>Object</td><td>false</td><td>Properties associated with the document.</td></tr></tbody></table>
{% endtab %}

{% tab title="Data" %}
**Data object ( mutable ) `<DataObject>`**

<table><thead><tr><th width="191.33333333333331">Name</th><th width="288">Type</th><th>Required</th></tr></thead><tbody><tr><td>type</td><td>string</td><td>false</td></tr><tr><td>name</td><td>string</td><td>false</td></tr><tr><td>description</td><td>string</td><td>false</td></tr><tr><td>documents</td><td>Array <strong>&#x3C;DocumentObject></strong></td><td>false</td></tr><tr><td>location</td><td><strong>&#x3C;LocationObject></strong></td><td>false</td></tr><tr><td>properties</td><td><strong>&#x3C;PropertiesObject></strong></td><td>false</td></tr><tr><td>identifiers</td><td><strong>&#x3C;IdentifiersObject></strong></td><td>false</td></tr><tr><td>minted</td><td>boolean</td><td>false</td></tr><tr><td>nft</td><td><strong>&#x3C;NFTObject></strong></td><td>false</td></tr><tr><td>status</td><td><strong>&#x3C;StatusObject></strong></td><td>false</td></tr></tbody></table>

\
\
**Asset data `<DataObject>`**

The NFT object is used to track non-fungible tokens associated with the asset. It includes the following fields:

**NFT object** (object is not available before minting)

<table><thead><tr><th width="181">Name</th><th width="107">Type</th><th>Description</th></tr></thead><tbody><tr><td>chain_id</td><td>string</td><td>network chain id</td></tr><tr><td>chain_name</td><td>string</td><td>network name “polygon”, “mumbai”</td></tr><tr><td>minted_at</td><td>number</td><td>The timestamp at which the token was minted</td></tr><tr><td>contract_address</td><td>string</td><td>The smart contract address for the token</td></tr><tr><td>owner_address</td><td>string</td><td>Asset owner address</td></tr><tr><td>external_id</td><td>string</td><td>Asset id</td></tr><tr><td>token_id</td><td>string</td><td>the token ID</td></tr><tr><td>asset_path</td><td>string</td><td>the base URI and extension for the asset base_uri/{external_id}/base_uri_extension</td></tr><tr><td>transaction_hash</td><td>string</td><td>the hash of the transaction associated with the token</td></tr><tr><td>transaction_explorer_url</td><td>string</td><td>the URL for the transaction explorer</td></tr><tr><td>confirmed</td><td>boolean</td><td>a boolean indicating whether the token has been confirmed (i.e. at least 5 blocks have confirmed the transaction)</td></tr></tbody></table>

```json
{
   "nft":{
	  "chain_id": "string",
	  "chain_name": "string",
	  "minted_at": "number",
	  "contract_address": "string",
	  "owner_address": "string",
	  "external_id": "string",
	  "token_id": "string",
	  "asset_path": "string",
	  "transaction_hash": "string",
	  "transaction_explorer_url": "string",
     	   "confirmed?":"boolean" 
   }
}

// add example for status object
```

Worker properties - define later worker properties

```json
{
	"worker": "polygon",
	"chain_id": "0x212"
}
```
{% endtab %}

{% tab title="Receipt" %}
**Object receipt `<ReceiptObject>`**

```json
{
  "id_hash":"string",
  "received_by":"string",
  "received_at":"number",
  "organization_id":"string",
  "account_id":"string"
}
```

<table><thead><tr><th width="163">Name</th><th width="103">Type</th><th width="115">Required</th><th></th></tr></thead><tbody><tr><td>id_hash</td><td>string</td><td>true</td><td>The hash of the asset's ID.</td></tr><tr><td>received_by</td><td>string</td><td>true</td><td>The address of the account that received the asset.</td></tr><tr><td>received_at</td><td>number</td><td>true</td><td>The timestamp of the asset's receipt.</td></tr><tr><td>organization_id</td><td>string</td><td>false</td><td>The ID of the organization that received the asset.</td></tr><tr><td>account_id</td><td>string</td><td>true</td><td>The ID of the account that received the asset.</td></tr></tbody></table>
{% endtab %}

{% tab title="Ownership" %}
```
{
   "account_id":"string",
}
```
{% endtab %}
{% endtabs %}

***

{% tabs %}
{% tab title="Documents" %}
**Documents array `<Documents>`**

```json
[
  {
    "id":"string",
    "url":"string",
    "type":"string",
    "properties":"object"
  }
]
```

Document id or url are required

<table><thead><tr><th width="128">Name</th><th width="96">Type</th><th width="110">Required</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>string</td><td>true</td><td>The ID of the document.</td></tr><tr><td>url</td><td>string</td><td>true</td><td>The URL of the document.</td></tr><tr><td>type</td><td>string</td><td>false</td><td>The type of the document. (image/jpg, image/png)</td></tr><tr><td>properties</td><td>string</td><td>false</td><td>Properties associated with the document. (size, resolution, alt, title)</td></tr></tbody></table>
{% endtab %}

{% tab title="Location" %}
**Location object `<LocationObject>`**

```json
{
   "type":"string",
   "geometry":{
     "type":"string",
     "coordinates":[
        "number",
        "number"
     ]
   },
   "country": "string",
   "region": "string",
   "city": "string",
   "zip": "string",
   "street": "string",
   "properties": "object"
}
```

<table><thead><tr><th width="136">Name</th><th width="187">Type</th><th width="98">Required</th><th></th></tr></thead><tbody><tr><td>type</td><td>string</td><td>true</td><td>The type of location.</td></tr><tr><td>geometry</td><td><strong>&#x3C;GeometryObject></strong></td><td>true</td><td>The geometry of the location.</td></tr><tr><td>country</td><td>string</td><td>false</td><td>The country of the location.</td></tr><tr><td>region</td><td>string</td><td>false</td><td>The region of the location.</td></tr><tr><td>city</td><td>string</td><td>false</td><td>The city of the location.</td></tr><tr><td>zip</td><td>string</td><td>false</td><td>The zip code of the location.</td></tr><tr><td>street</td><td>string</td><td>false</td><td>The street address of the location.</td></tr><tr><td>properties</td><td>string</td><td>false</td><td>Properties associated with the location.</td></tr></tbody></table>

**Geometry object** **`<GeometryObject>`**

| Name        | Type                 | Required |                                  |
| ----------- | -------------------- | -------- | -------------------------------- |
| type        | string               | true     | The type of the geometry.        |
| coordinates | \<Array> \[lat, lng] | true     | The coordinates of the geometry. |
{% endtab %}

{% tab title="Properties" %}
Properties are stored in a key:value format, where the value can be any type.

**`<Properties>`**

```json
{
    "id":"0x123123",
    "ids":["0x123123", "0x123123", "0x123123"],
    "total": 5
    "details": {
        "test": 123
    }
}
```
{% endtab %}

{% tab title="Identifiers" %}
Identifiers are stored in a key:value format, where the value can be an array containing strings or numbers.

**`<Identifiers>`**

```json
{
    "ids":["0x123123"],
    "uid":["0x123123", "0x123123", "0x123123"]
}
```
{% endtab %}
{% endtabs %}

***

## Create new asset

{% swagger method="post" path="/assets" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorized" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-parameter in="body" name="object" type="Object" required="true" %}
Asset immutable object
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="false" %}
Asset mutable object
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ownership" type="Object" required="false" %}
Ownership object
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
  "response": {
    "proof": "0x92c055f67fa3b3d350e3d6e587b229a2f01b37b33927aa017204d8d51e4d5fe922f5f45c77c5d8eee772dc40de7a031ea9dc38069cc56f0ac4491469a3fc47111b",
    "id": "0x1bc06747b2e6ae4e58b96a8a9d1ffb09149726993f0a2b6b19425c4a502cd868",
    "receipt": {
      "id_hash": "0xd01388f33cb40f5dc0f68350076ee90d40a83b69c2d32b0ab171e7151201bb31",
      "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
      "received_at": 1677928493081,
      "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
      "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
    },
    "ownership": {
      "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4",
    }
  },
  "meta": {
    "code": 200,
    "message": "Created"
  }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Validation error"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Unauthorized request" %}
```json
{
    "status": "error",
    "message": "Unauthoirzed"
}
```
{% endswagger-response %}
{% endswagger %}

<details>

<summary>Body schema</summary>

```json

{
  "object": {
    "meta": {
      "created_by": "0x627969CD9Ef88bA7e61694947020540d7eD0001d",
      "created_at": 1663140277763,
      "data_hash": "0x4399c2945b39b193daaefeeaeb60db8cda6de7215eba02425e4eb2c6a1cddaca"
    },
    "data": {
      "type": "zimt.asset.nft",
      "name": "New asset",
      "collection_id": "1",
      "description": "New NFT asset",
			"documents": [
            {
              "type": "image/jpeg",
              "name": "_MG_3775_obrada",
              "id": "0x3541132d41c697e0fec7d669445245d0e73fc8e3fc31460c20b7eb36883b6147",
              "properties": {
                "width": 3003,
                "height": 4209,
              }
            }
      ],
    },
    "signature": "0xed30fbf01fad7d950992b05b507ef3e3dd7c1243cf804f38d8d5a6d3a59dca193e899b29463b59455b5b9987aa251ecb268a124f09fc9702814df1797d958e6c1c"
  }
}
```

</details>

***

## Update asset <mark style="color:blue;">`update`</mark> <mark style="color:orange;">`replace`</mark>

{% swagger method="put" path="/assets/:id" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark>

The Update Asset endpoint allows users to update mutable data of existing assets.

The endpoint accepts a PUT request with an ID parameter, which specifies the asset to be updated. The request must also include a data object, which contains the updated information for the asset.

The data object can include new values for the asset's name, description, collection ID, and properties. It may also include an updated documents array, which can hold media for the asset.

The response to a successful update request includes the same information as the response to a create request, including the updated proof and ID for the asset, the new data object, and the updated ownership information.

As well as updating the asset’s mutable data with the data provided, we create an event for that update action.

There are two types of events used for asset update:
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique asset id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="false" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
  "proof": "123",
  "id": "1",
  "object": {
    "meta": {
      "created_by": "0x627969CD9Ef88bA7e61694947020540d7eD0001d",
      "created_at": 1661175493100,
      "data_hash": "0x42e8f1d138cf423303937fac4f9ade642bc89ce7bfe4b2904bed33bdae070670"
    },
    "signature": "0xe633051fc76ae...",
    "data": {
      "type": "zimt.lot",
      "name": "Info - Asset 1",
      "collection_id": "1",
      "properties": {
        "nft": {
          "confirmed": true
        },
        "prop": "123",
        "tags": [
          "tag1"
        ]
      }
    }
  },
  "data": {
    "name": "Replace"
  },
  "receipt": {
    "id_hash": "0xf94c888c3983844ffc04006fabf04577c9a37ff8893423478d096754e20acfdf",
    "received_by": "0x678b3c5090B25b3a63120CF0218750886e37A96E",
    "received_at": 1661175493100,
    "organization_id": "1",
    "account_id": "1"
  },
  "hub_meta": {
    "modified_at": 1693814240,
    "views": 4,
    "has_events": true
  },
  "ownership": {
    "account_id": "1"
  }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}

{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Unauthorized request" %}
```json
{
    "status": "error", 
    "message": "Unauthorized"
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="Permission denied" %}
```json
{
    "status": "error", 
    "message": "Unauthorized"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Asset not found" %}
```json
{
    "status": "error", 
    "message": "Asset not found"
}
```
{% endswagger-response %}
{% endswagger %}

<details>

<summary>Body schema</summary>

```json
{
  "object": {
    "meta": {
      "created_by": "0xf8C6fe32259915656FbC028B51d02f10CA84abbE",
      "created_at": 1693812478355,
      "data_hash": "0xd614cbeb6425def8c800c61d4b7dbe02d42e3960bcba97044473af47e9d80573"
    },
    "data": {
      "type": "asset.replace",
      "asset_id": "1",
      "name": "Replace"
    },
    "signature": "0x0e6fff43ac9d92655f8bfb1b238cb07923d1894df510ab794f806a20b6a6d5537281a040a3ca1eb6281b30f7ead63d3c012178ab530b24421d724d9f7601c3d51b"
  }
}
```

</details>

***

## Get single asset

{% swagger method="get" path="/assets/:id" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark>

The "Get single asset" endpoint allows users to retrieve information about a specific asset, either through a protected or public request.

When making a public request, users do not need to include an authorization token. However, they will only be able to access basic information about the asset, such as its name and description. They will not be able to access any sensitive or private information about the asset.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique asset id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-parameter in="query" name="author" type="Boolean" required="false" %}
\<AccountPublicObject> will be attached in the data object.

asset.meta.created\_by
{% endswagger-parameter %}

{% swagger-parameter in="query" name="owner" type="Boolean" required="false" %}
\<AccountPublicObject> will be attached in the data object.

asset.ownership.account\_id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0x6c2d84ce77229843b87e104e6757fabfbe5e637563bd2c87261adf35766566771b07e5edb997151c3a2eba9208d31d720f3bd4048e23beb4290a37bafc7a43f01b",
        "id": "0x6596312eb24774a23aafc5b5864f643e27c6821ee62b6d0e37282d785bd9c238",
        "object": {
            "meta": {
                "created_by": "0xfa17fc61Da37fc5EE6cbFb0EB93a0714a1972475",
                "created_at": 1694988212040,
                "data_hash": "0x73dd28b257f110564525c6de63f8bc918a343a2d3dc9eeb8d4810fec6813aea5"
            },
            "signature": "0x442ed06f29c1461b9f270b41f977ce805d7d307cd7aa29ec2589a1510f0a80304cd32fd47cd1d50310892fd090d1b3a870e471e62ebbeb437eb68389489818501b",
            "data": {
                "collection_id": "0xad2dd53c67f8330d8bc0747b45bcd9bb9214957b12724b8bc0e4c85ae95d1d3f",
                "type": "asset.project"
            }
        },
        "data": {
            "type": "asset.project",
            "name": "Only test",
            "description": "Test is a Web3 discovery platform that employs decentralized attestation to verify the authenticity and legitimacy of early-stage Web3 projects, offering investors a trusted and transparent means to discover and invest in these ventures. Additionally, OnlyDust serves as a platform for streamlining open-source software development, connecting developers with suitable projects, facilitating effective contributions, and providing incentives for their work within the Web3 ecosystem.",
            "minted": false,
            "properties": {
                "website": [
                    "https://www.test.xyz/"
                ],
                "socials": [
                    "https://twitter.com/test",
                    "https://github.com/test"
                ],
                "socials_object": {
                    "twitter": "https://twitter.com/test",
                    "github": "https://github.com/test"
                },
                "slug": "test-dust"
            }
        },
        "receipt": {
            "id_hash": "0xd413dc45f05644f00f04ab76479205ee0a605ab3a3b391e71dddfd869c565d3b",
            "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at": 1694988212465,
            "organization_id": "0xd8c88f906252635db67285f43df70510015c541b6caf358521fda3c879b9db66",
            "account_id": "0x7f2cb93cc17006afe4b5f41f3c42c002bb099db005c5ce38d647ccf8db6e1aa9"
        },
        "ownership": {
            "account_id": "0x7f2cb93cc17006afe4b5f41f3c42c002bb099db005c5ce38d647ccf8db6e1aa9",
            "short_name": "the-test"
        },
        "hub_meta": {
            "analytics_resolved": true,
            "views": 2,
            "modified_at": 1695046007
        }
    },
    "status": "ok"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Unauthorized request" %}
```json
{
    "status": "error", 
    "message": "Unauthorized"
}
```
{% endswagger-response %}

{% swagger-response status="403: Forbidden" description="Permission denied" %}
```json
{
    "status": "error", 
    "message": "Unauthorized"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Asset not found" %}
```json
{
    "status": "error", 
    "message": "Asset not found"
}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Owner / Author" %}
Account Public Object **\<AccountPublicObject>**

```json
{
  "short_name": "string",
  "address": "string",
  "id": "string",
  "avatar": {
    "id": "string",
    "name": "string",
    "properties": {...}
  }
}
```
{% endtab %}
{% endtabs %}

***

## Get single asset public

{% swagger method="get" path="/assets/:id/public" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique asset id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-parameter in="query" name="author" type="Boolean" required="false" %}
\<AccountPublicObject> will be attached in the data object.

asset.meta.created\_by
{% endswagger-parameter %}

{% swagger-parameter in="query" name="owner" type="Boolean" required="false" %}
\<AccountPublicObject> will be attached in the data object.

asset.ownership.account\_id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
  "response": {
    "proof": "0xf6cfeb297f8d18271eada98857e059966d553542fb591e10750542eb7fa6012c1ba2d7f8a13ab60cb6ed204074fda930ad926eb78251358e3034f7fd00a63e201b",
    "id": "0x6cbdd72a53f3e719fc4b6bd111fd30becea1aebdd226c8174f5899d4032fe26a",
    "object": {
      "meta": {
        "created_by": "0x9477591A6D4221d62eE350f90CfCcB15Ef5BF1E1",
        "created_at": 1681386754,
        "data_hash": "0x40f47d3a4f29b3e010aef956a8930286a839df6b935b725e8bb1b55d239542a5"
      },
      "data": {
        "type": "cream.asset.photo",
        "name": "Cocoa & Chocolate",
        "description": "Very yummi chocoa - check out!",
        "documents": [
          {
            "type": "image/jpeg",
            "name": "_MG_3318",
            "id": "0x9d465c61d1663c20782f9e993f15e41a8890933e24c70dca0cc5dff281bd8ff1",
            "properties": {
              "width": 5472,
              "height": 3648,
              "fileSize": 985714,
              "fileExtension": "jpg",
              "localUrl": "blob:https://staging.doublecream.co/923b6328-50db-4189-8500-8a002edc8d2a"
            }
          }
        ],
        "collection_id": "0x448708d508779d29ea2b1ed2b6b15d8f65f98fb50ff486e636665fc2f824bed6",
        "properties": {
          "exclusive": true,
          "payment": {
            "type": "token"
          }
        }
      },
      "signature": "0x7ded21a251aba6faccdb7678baf6ed4c70fe7257b9a70792bbd673af5225d75021569d1e2c92b227d0ffae3699e15ebed74fbd16792bb9f3dff10c85d0b1fc5f1b"
    },
    "data": {
      "minted": true,
      "description": "Very yummi chocoa - check out!",
      "name": "Cocoa & Chocolate",
      "properties": {
        "allowedLicenses": [
          {
            "id": "0xbaf903c07908245eabaefb2bba2c0f0c70ddcd89aaf9d8174e314e94e62f3429",
            "name": "Non profit",
            "type": "cream.license.nonprofit",
            "price": 10
          },
          {
            "id": "0x692e4521bc99dd31a104b00dd329f8958d4e21ca44450ff03f1dc2ea9d1a0d7b",
            "name": "Commercial",
            "type": "cream.license.commercial",
            "price": 250
          }
        ],
        "analytics": {
          "0x692e4521bc99dd31a104b00dd329f8958d4e21ca44450ff03f1dc2ea9d1a0d7b": {
            "sold": 0
          },
          "0xbaf903c07908245eabaefb2bba2c0f0c70ddcd89aaf9d8174e314e94e62f3429": {
            "sold": 0
          }
        },
        "location": "",
        "nft": {
          "chain_id": "0x89"
        },
        "payment": {
          "type": "token"
        },
        "reactions": {
          "likes": 0,
          "score": 0
        },
        "sale": {
          "offerable": true,
          "price": 1500
        },
        "tags": []
      },
      "type": "cream.asset.photo",
      "status": {
        "mint": false
      },
      "nft": {
        "chain_id": "0x89",
        "chain_name": "Polygon Mainnet",
        "contract_address": "0x449E31eFa7F124DC0E061746dd28a79bb2550606",
        "external_id": "0x6cbdd72a53f3e719fc4b6bd111fd30becea1aebdd226c8174f5899d4032fe26a",
        "minted_at": 1681386770421,
        "owner_address": "0x9477591A6D4221d62eE350f90CfCcB15Ef5BF1E1",
        "token_id": "9",
        "transaction_hash": "0xf0fff772e7f0fca38422130ae649af0c539b211328e3b56f9a8b8cee7c81ab74",
        "confirmed": true
      },
      "author": {
        "short_name": "foodcrafters_477",
        "address": "0x9477591A6D4221d62eE350f90CfCcB15Ef5BF1E1",
        "id": "0xd3a5cde8047dbd719e717b39f1bc8263140ffd612db8f5d870f91028888d7c47"
      },
      "owner": {
        "short_name": "foodcrafters_477",
        "address": "0x9477591A6D4221d62eE350f90CfCcB15Ef5BF1E1",
        "id": "0xd3a5cde8047dbd719e717b39f1bc8263140ffd612db8f5d870f91028888d7c47"
      }
    },
    "receipt": {
      "id_hash": "0xabdcfdd3a0ac1112d9d95cd8048d36a0a4e07112e0434b913186610b0eef4eb1",
      "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
      "received_at": 1681386755127,
      "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
      "account_id": "0xd3a5cde8047dbd719e717b39f1bc8263140ffd612db8f5d870f91028888d7c47"
    },
    "ownership": {
      "account_id": "0xd3a5cde8047dbd719e717b39f1bc8263140ffd612db8f5d870f91028888d7c47",
      "short_name": "foodcrafters_477"
    },
    "hub_meta": {
      "analytics_resolved": true,
      "modified_at": 1695048752,
      "has_events": true,
      "views": 35
    }
  },
  "meta": {
    "code": 200
  }
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Asset not found" %}
```json
{
    "status": "error", 
    "message": "Asset not found"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Search assets

{% swagger method="post" summary="" path="/assets/search" baseUrl="https://api.zi.mt" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="query" type="Object" required="true" %}
Asset search query
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":[
     {
   "proof":"0xf402f6f18fb611a6c621b5e66fe34b7ab589b139cb0c47754a25c83eb9ff95c967131897700cc2d4522e6e27350383299a4971758dcb98e29ba2678bf675c6c61b",
   "id":"0x804d02199943d69b174423k542bf68cffc4f1b2b50840cf9a93534e3c3e9d",
   "object":{
      "meta":{
         "created_by":"0xAEc3b1e33b7e4810421fc1778FEB29365E70C8E8",
         "created_at":1661175493300,
         "data_hash":"0x8993b05102199943d69bf22cc33d1835414ad40405eb7597cbccb25c1938f3a1"
      },
      "signature":"0xe630x8993b05102199943d69bf22cc33d1835414ad40405eb7597cbccb25c1938f3a10x8993b05102199943d69bf22cc33d1835414ad40405eb7597cbccb25c1938f3a1",
      "data":{
         "type":"zimt.item",
         "name":"Info - Asset 13",
         "properties":{
	    "exclusive": true,
            "prop":"567",
            "tags":[
               "tag3",
               "tag4"
            ]
         }
      }
   },
   "receipt":{
      "id_hash":"0xf94c888c3983844ffc04006fabf04577c9a37ff8893423478d096754e20acfdf",
      "received_by":"0x678b3c5090B25b3a63120CF0218750886e37A96E",
      "received_at":1661175493300,
      "organization_id":"1",
      "account_id":"0x804d0cf37a12ab6174446b084c399bf68cffc4f1b2b50840cf9a93534e3c3e9d"
   },
   "ownership":{
      "account_id":"0x804d0cf37a12ab6174446b084c399bf68cffc4f1b2b50840cf9a93534e3c3e9d",
   }}
   ],
   "status":"ok",
   "pagination":{
      "next":"eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjYxMTc1NDkzMTAwfQ==",
      "previous":"eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjYzMTQwODY4MDA2fQ==",
      "hasNext":false,
      "size":11,
      "skip":0,
      "limit":30
   }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Validation error",
    "errors": "request has invalid properties."
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Unauthorized request" %}
```json
{
    "status": "error",
    "message": "Unauthorized",
}
```
{% endswagger-response %}
{% endswagger %}

<details>

<summary>Body schema</summary>

<pre class="language-json"><code class="lang-json"><strong>{
</strong>   "query":{
      "assets":[
      [
         {
            "field":"object.data.name",
            "operator":"contains",
            "value":"test"
         },
         {
            "field":"object.data.standard",
            "operator":"contains",
            "value":"test"
         },
         {
            "field":"object.data.symbol",
            "operator":"contains",
            "value":"test"
         },
         {
            "field":"object.data.type",
            "operator":"contains",
            "value":"test"
         }
      ], []
      ]
   },
   "query_settings":{
      "type":"$or",
      "types": ["$or", "$and"]
   },
   "limit":"number",
   "skip":"number",
   "sortAscending":"boolean"
}
</code></pre>

</details>

***

## Mint Queue

{% swagger method="post" path="/assets/:id/mint" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark>

This request is used to update the property of an asset that a worker is observing so that the asset can be minted in the background.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique asset id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "status":"ok", 
   "message": "Success"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get Mint status

{% swagger method="get" path="/assets/:id/mintstatus" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark>

The `/assets/:asset_id/mintstatus` endpoint is used to retrieve the current mint status of an asset.

To use this endpoint, you should provide the `asset_id` as a parameter in the path. The response will include a `confirmed` property that indicates whether the asset has been confirmed on the blockchain.

Note that the `nft` object will only be present if the asset has been successfully minted.

In the response, the `confirmed` property indicates whether the transaction has been confirmed on the blockchain. If `confirmed` is `false`, it means that the transaction is still pending confirmation.

The `minted` property indicates whether the asset has been successfully minted. If `minted` is `false`, it means that the minting process has not yet been completed.

If the asset has been successfully minted, the response will include a `nft` object that contains metadata about the asset's NFT. This object includes the following properties:

If the asset has not been successfully minted, the `nft` object will not be present in the response.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Asset unique id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
  "confirmed": true,
  "minted": true,
  "nft": {
    "chain_id": "0x89",
    "chain_name": "Polygon Mainnet",
    "contract_address": "0x449E31eFa7F124DC0E061746dd28a79bb2550606",
    "external_id": "0x1bc06747b2e6ae4e58b96a8a9d1ffb09149726993f0a2b6b19425c4a502cd868",
    "minted_at": 1677928509472,
    "owner_address": "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
    "token_id": "4",
    "transaction_hash": "0x4cdb7a095201aec0aff4765d5dda5d592c41e6bcb498d84da4af15961e6c0d68",
    "confirmed": true
  }
}

```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Asset not found" %}
```json
{
    "status": "error",
    "message": "Asset not found"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get Asset Metadata

{% swagger method="get" path="/assets/:id/metadata" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:green;">**`PUBLIC`**</mark>

Retrieves metadata for a specific asset, including its name, description, type, URL, attributes, and image.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique asset id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "name": "Test photo",
    "description": "some description here",
    "type": "cream.asset.photo",
    "external_url": "<https://cream-api-staging.herokuapp.com/assets/public/0x1bc06747b2e6ae4e58b96a8a9d1ffb09149726993f0a2b6b19425c4a502cd868>",
    "attributes": [
        {
            "name": "type",
            "value": "cream.asset.photo"
        },
        {
            "name": "tags",
            "value": [
                "test",
                "nft",
                "crypto",
                "one",
                "more",
                "tag"
            ]
        },
        {
            "name": "location",
            "value": "Switzerland"
        }
    ],
    "image": "<https://cream-api-staging.herokuapp.com/assets/public/0x1bc06747b2e6ae4e58b96a8a9d1ffb09149726993f0a2b6b19425c4a502cd868>",
    "image_url": "<https://cream-api-staging.herokuapp.com/assets/public/0x1bc06747b2e6ae4e58b96a8a9d1ffb09149726993f0a2b6b19425c4a502cd868>"
}

```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Asset not found" %}
```json
{}
```
{% endswagger-response %}
{% endswagger %}

{% tabs %}
{% tab title="Metadata" %}
<table><thead><tr><th width="196">key</th><th>description</th></tr></thead><tbody><tr><td>image</td><td>This is the URL to the image of the item. Can be just about any type of image (including SVGs, which will be cached into PNGs by OpenSea), and can be <a href="https://github.com/ipfs/is-ipfs">https://github.com/ipfs/is-ipfs</a> URLs or paths. We recommend using a 350 x 350 image.</td></tr><tr><td>image data</td><td>Raw SVG image data, if you want to generate images on the fly (not recommended). Only use this if you're not including the image parameter.</td></tr><tr><td>external_url</td><td>This is the URL that will appear below the asset's image on OpenSea and will allow users to leave OpenSea and view the item on your site.</td></tr><tr><td>description</td><td>A human readable description of the item. Markdown is supported.</td></tr><tr><td>name</td><td>Name of the item.</td></tr><tr><td>attributes</td><td>These are the attributes for the item, which will show up on the OpenSea page for the item.</td></tr><tr><td>background_color</td><td>Background color of the item on OpenSea. Must be a six-character hexadecimal without a pre-pended #.</td></tr><tr><td>animation_url</td><td>A URL to a multi-media attachment for the item. The file extensions GLTF, GLB, WEBM, MP4, M4V, OGV, and OGG are supported, along with the audio-only extensions MP3, WAV, and OGA. Animation_url also supports HTML pages, allowing you to build rich experiences and interactive NFTs using JavaScript canvas, WebGL, and more. Scripts and relative paths within the HTML page are now supported. However, access to browser extensions is not supported.</td></tr><tr><td>youtube_url</td><td>A URL to a YouTube video.</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

***