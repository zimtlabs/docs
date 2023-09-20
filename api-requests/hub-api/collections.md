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

# Collections

Collection holds some settings which will unite all assets. Additionally user can deploy a collection on a multiple chains thus assets in that collection can only be minted on those chains.

Collection can define whether assets of that collection can be transferable, burnable or mintable. also user can define which addresses (wallets) can manage those assets

Also a user can add properties and identifiers of collection.

Example of the collection can be: concert tickets, membership cards, actual NFTs, certificates, licenses, etc.

Organization admin or creator of the collection can deploy collection on multiple chains.

{% hint style="info" %}
⚡ Collections are **immutable** and edit request is **not** available
{% endhint %}

User can also see <mark style="color:blue;">`metadata`</mark>, <mark style="color:blue;">`json`</mark>, <mark style="color:blue;">`properties`</mark>, <mark style="color:blue;">`identifiers`</mark> and media of the collection. Additionally user can see settings of collection, assets inside of that collection and status.

### Metadata

Metadata for assets inside of a collection are defined as <mark style="color:blue;">`Base URI`</mark> <mark style="color:blue;">+</mark> <mark style="color:blue;">`asset_id`</mark> <mark style="color:blue;">+</mark> <mark style="color:blue;">`Base URI`</mark>` `` ``Extension ` which for now is fixed to:

<mark style="color:blue;">`/assets/:asset_id/metadata`</mark>

{% hint style="info" %}
Depending on environment <mark style="color:blue;">`Base URI`</mark> might be different.
{% endhint %}

***

## Create collection

{% swagger method="post" path="/collections" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark> <mark style="color:blue;">**`ADMIN`**</mark>

Upon creating the collection user must provide `collection_name`, `symbol` and `description`

User can select whether the assets of that collection can be burned, minted or transferred.

User can also set the maximum supply of assets in that collection

💡 If maximum supply is set to **0** there is no limit of assets inside of that organization

Roles is an array of wallet addresses which can preform actions over assets inside of that collection (burnable, mintable, transferable)

**Format**: `["0xA12C...DA33", "0xB23E...F36F", ...]`

User can also set `properties` and `identifiers` for that collection.

Accepted types: `Object`, `JSON`, `Text`, `URL`, `Array`

**Additional description**

In order to create a new collection, we’ll have to execute a POST request on the specified endpoint. In the example below we are creating a ERC721 collection-type. (only standard supported by now)
{% endswagger-description %}

{% swagger-parameter in="body" name="object" type="Object" required="true" %}
Immutable object data
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}
Collection mutable data
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorized" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":{
      "proof":"0xaf71d08500b4b31a0e8db536538e0b03645ebe8ee89fecca7abf7e15387b92bf384f95ad72e63a03dd6e95a5c39ff2b0d9c6b9f24f8c9fc5a5c4f5de0e009c061c",
      "id":"0xb65b363e6dc1f897065b2c6c3478eecc6081f16c7f22734873cc430de3854c61",
      "receipt":{
         "id_hash":"0x0f5c71febe298c18a48f1cc68f35d13b1acbe1d6bbca7836a401627835fd57d9",
         "received_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
         "received_at":1670431267576,
         "organization_id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "ownership":{
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      }
   },
   "status":"ok",
   "message":"Created"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Validation error",
    "errors": "object is required; request has invalid properties."
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

```json
{
   "object":{
      "meta":{
         "created_by":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
         "created_at":1670431267105,
         "data_hash":"0x07b153abe384e3f627ed481ecd09ff2c548edb793596b13d5d8e69bbd18b35aa"
      },
      "signature":"0x71ee4e41720be4494fa4c56013e37a9311e5d510df4d8c42b05a4b2e9a0b961c06bbe320394a0a9e335874d8374f3ec82a432f63e5d4df85611bf0419d6ae2c01b",
      "data":{
         "deployment_configuration":{
            "base_uri":"https://api-dev.zi.mt/assets/",
            "base_uri_extension":"/metadata",
            "maximum_supply":"2"
         },
         "initial_roles":[
            {
               "role":"MINTER_ROLE",
               "addresses":["0x123123"]
            },
            {
               "role":"TRANSFER_ROLE",
               "addresses":[]
            },
            {
               "role":"BURN_ROLE",
               "addresses":[]
            }
         ],
         "standard":"erc721",
         "type":"ERC721Roles",
         "name":"test3",
         "description":"test3",
         "symbol":"test3",
         "burnable":true,
         "transferable":true
      }
   },
   "data":{
      "properties":{
         "prop1":"value1"
      }
   }
}
```

</details>

<table><thead><tr><th width="161">Name</th><th width="135.33333333333331">Type</th><th>Description</th></tr></thead><tbody><tr><td>standard</td><td>string</td><td>for now, only ERC721 supported</td></tr><tr><td>name</td><td>string</td><td>with or without spaces between words, i.e. Simple Collection Name/SimpleCollectionName, depending on user preferences</td></tr><tr><td>symbol</td><td>string</td><td>usually abbreviation of the collection name</td></tr><tr><td>burnable</td><td>boolean</td><td>if true, NFTs from this collection can be burned</td></tr><tr><td>transferable</td><td>boolean</td><td>if true, NFTs from this collection can be transferred to another address</td></tr><tr><td>deployment_configuration</td><td>Object<br>&#x3C;DeploymentConfiguration></td><td></td></tr><tr><td>initial_roles</td><td>Array<br>&#x3C;InitialRoles></td><td></td></tr><tr><td>identifiers</td><td>Object</td><td></td></tr><tr><td>properties</td><td>Object</td><td></td></tr></tbody></table>

***

## Search collections

{% swagger method="post" summary="" path="/collections/search" baseUrl="https://api.zi.mt" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="query" type="Object" required="true" %}
Collection search query
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":[
      {
         "id":"0xbaf903c07908245eabaefb2bba2c0f0c70ddcd89aaf9d8174e314e94e62f3429",
         "object":{
            "meta":{
               "created_by":"0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
               "created_at":1677525023763,
               "data_hash":"0x19c8a6e8f2f9875d3200b618d473972606b10d4722c7827b8d141c36a9be160b"
            },
            "signature":"0x4fcb0b1b78772980e298746a4c690393e719a97090218a6847a759a77b3437876603b430642dc46807785c7b36c6f81a13ea55e5383359694560914e2b1118a71c",
            "data":{
               "deployment_configuration":{
                  "base_uri":"<https://api-staging.zi.mt/assets/>",
                  "base_uri_extension":"/metadata",
                  "maximum_supply":"0"
               },
               "initial_roles":[
                  {
                     "role":"MINTER_ROLE",
                     "addresses":[
                        
                     ]
                  },
                  {
                     "role":"TRANSFER_ROLE",
                     "addresses":[
                        
                     ]
                  },
                  {
                     "role":"BURN_ROLE",
                     "addresses":[
                        
                     ]
                  }
               ],
               "standard":"erc721",
               "type":"ERC721Roles",
               "name":"Non profit",
               "description":"Non profit license",
               "symbol":"nonprofit",
               "burnable":true,
               "transferable":true
            }
         },
         "data":{
            "current_roles":[
               {
                  "role":"MINTER_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               },
               {
                  "role":"TRANSFER_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               },
               {
                  "role":"BURN_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               }
            ],
            "properties":{
               "type":"cream.license.nonprofit",
               "defaultPrice":"50",
               "default_price":"50"
            },
            "status":"not-deployed"
         },
         "ownership":{
            "account_id":"0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
         },
         "receipt":{
            "id_hash":"0x491916db4855cbde96313ec2a29d618c78c66dd138201287f66741e568575a2f",
            "received_by":"0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at":1677525024098,
            "organization_id":"0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
            "account_id":"0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
         },
         "proof":"0xdb4e6f008e2f20c593e6e8f553348ec69fcadc85b2cfcb82456226bfb0881dae3dd581b256467399c68e677f8d856b7854b58c1ac0de31d560ae37f3d8375d121c",
         "hub_meta":{
            "analytics_resolved":true,
            "modified_at":1677525024
         },
         "asset_count":2
      },
      
      {
         "id":"0x448708d508779d29ea2b1ed2b6b15d8f65f98fb50ff486e636665fc2f824bed6",
         "object":{
            "meta":{
               "created_by":"0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
               "created_at":1677524827817,
               "data_hash":"0x0b80b799908922294e523398c33617028d2073b4978a12146fb4c7b4eaf111c9"
            },
            "signature":"0xf5ca885d64cb9ca771b03d17c5b412436c35788639abff71db5bfa3065dd728816ff3eff9ea556083d60b063643547e23caf9c35f1c2753a57631a1ffe83dc491b",
            "data":{
               "deployment_configuration":{
                  "base_uri":"<https://api-staging.zi.mt/assets/>",
                  "base_uri_extension":"/metadata",
                  "maximum_supply":"0"
               },
               "initial_roles":[
                  {
                     "role":"MINTER_ROLE",
                     "addresses":[
                        
                     ]
                  },
                  {
                     "role":"TRANSFER_ROLE",
                     "addresses":[
                        
                     ]
                  },
                  {
                     "role":"BURN_ROLE",
                     "addresses":[
                        
                     ]
                  }
               ],
               "standard":"erc721",
               "type":"ERC721Roles",
               "name":"DC staging exclusive",
               "description":"Dc staging exclusive",
               "symbol":"exclusive",
               "burnable":true,
               "transferable":true
            }
         },
         "data":{
            "current_roles":[
               {
                  "role":"MINTER_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               },
               {
                  "role":"TRANSFER_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               },
               {
                  "role":"BURN_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               }
            ],
            "properties":{
               "type":"cream.license.exclusive"
            },
            "status":"deployed",
            "deployment":[
               {
                  "status":"deployed",
                  "chain_id":"0x89",
                  "chain_name":"Polygon Mainnet",
                  "contract_address":"0x449E31eFa7F124DC0E061746dd28a79bb2550606",
                  "deployed_by":"0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC",
                  "owned_by":"0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                  "transaction_hash":"0x4676b8c552e1b84b09479e11a20d2b8461e894dfe39a4a2574c7023dd4d12546"
               }
            ]
         },
         "ownership":{
            "account_id":"0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
         },
         "receipt":{
            "id_hash":"0x56f2b411761671d6da1c0491378949fc0209309f47b8ee68881830abc68ce1cc",
            "received_by":"0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at":1677524828189,
            "organization_id":"0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
            "account_id":"0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
         },
         "proof":"0x23a14f190b95ba55b398fa9008a8743950233833173373fe9feeb6f16dbf81f35b51f4caeabeae666c61397d659e4d259f7f3b723d763b19da9083db3a6fefa61c",
         "hub_meta":{
            "analytics_resolved":true,
            "modified_at":1677524881
         },
         "asset_count":4
      }
   ],
   "status":"ok",
   "pagination":{
      "next":"eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI0ODI4MTg5fQ==",
      "previous":"eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI1MDI0MDk4fQ==",
      "hasNext":false,
      "size":4,
      "skip":0,
      "limit":11
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

```json
{
   "query":{
      "collections":[
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
```

</details>

***

## Get many collections

{% swagger method="get" summary="" baseUrl="https://api.zi.mt" path="/collections" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark> <mark style="color:blue;">**`ADMIN`**</mark>

This endpoint retrieves information about all the collections owned by the authenticated user.
{% endswagger-description %}

{% swagger-parameter in="query" name="page" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="page_limit" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="name" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="standard" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="status" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="created" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="symbol" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="query" name="order_by" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":[
      {
         "id":"0xbaf903c07908245eabaefb2bba2c0f0c70ddcd89aaf9d8174e314e94e62f3429",
         "object":{
            "meta":{
               "created_by":"0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
               "created_at":1677525023763,
               "data_hash":"0x19c8a6e8f2f9875d3200b618d473972606b10d4722c7827b8d141c36a9be160b"
            },
            "signature":"0x4fcb0b1b78772980e298746a4c690393e719a97090218a6847a759a77b3437876603b430642dc46807785c7b36c6f81a13ea55e5383359694560914e2b1118a71c",
            "data":{
               "deployment_configuration":{
                  "base_uri":"<https://api-staging.zi.mt/assets/>",
                  "base_uri_extension":"/metadata",
                  "maximum_supply":"0"
               },
               "initial_roles":[
                  {
                     "role":"MINTER_ROLE",
                     "addresses":[
                        
                     ]
                  },
                  {
                     "role":"TRANSFER_ROLE",
                     "addresses":[
                        
                     ]
                  },
                  {
                     "role":"BURN_ROLE",
                     "addresses":[
                        
                     ]
                  }
               ],
               "standard":"erc721",
               "type":"ERC721Roles",
               "name":"Non profit",
               "description":"Non profit license",
               "symbol":"nonprofit",
               "burnable":true,
               "transferable":true
            }
         },
         "data":{
            "current_roles":[
               {
                  "role":"MINTER_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               },
               {
                  "role":"TRANSFER_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               },
               {
                  "role":"BURN_ROLE",
                  "addresses":[
                     "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                     "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
                  ]
               }
            ],
            "properties":{
               "type":"cream.license.nonprofit",
               "defaultPrice":"50",
               "default_price":"50"
            },
            "status":"not-deployed"
         },
         "ownership":{
            "account_id":"0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
         },
         "receipt":{
            "id_hash":"0x491916db4855cbde96313ec2a29d618c78c66dd138201287f66741e568575a2f",
            "received_by":"0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at":1677525024098,
            "organization_id":"0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
            "account_id":"0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
         },
         "proof":"0xdb4e6f008e2f20c593e6e8f553348ec69fcadc85b2cfcb82456226bfb0881dae3dd581b256467399c68e677f8d856b7854b58c1ac0de31d560ae37f3d8375d121c",
         "hub_meta":{
            "analytics_resolved":true,
            "modified_at":1677525024
         },
         "asset_count":2
      },
      
   ],
   "status":"ok",
   "pagination":{
      "next":"eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI0ODI4MTg5fQ==",
      "previous":"eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI1MDI0MDk4fQ==",
      "hasNext":false,
      "size":4,
      "skip":0,
      "limit":11
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

***

## Get single collection

{% swagger method="get" summary="" baseUrl="https://api.zi.mt" path="/collections/:id" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="path" name="id" required="true" type="String" %}
Unique collection id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorized" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":{
      "proof":"0xaf71d08500b4b31a0e8db536538e0b03645ebe8ee89fecca7abf7e15387b92bf384f95ad72e63a03dd6e95a5c39ff2b0d9c6b9f24f8c9fc5a5c4f5de0e009c061c",
      "id":"0xb65b363e6dc1f897065b2c6c3478eecc6081f16c7f22734873cc430de3854c61",
      "object":{
         "meta":{
            "created_by":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
            "created_at":1670431267105,
            "data_hash":"0x07b153abe384e3f627ed481ecd09ff2c548edb793596b13d5d8e69bbd18b35aa"
         },
         "signature":"0x71ee4e41720be4494fa4c56013e37a9311e5d510df4d8c42b05a4b2e9a0b961c06bbe320394a0a9e335874d8374f3ec82a432f63e5d4df85611bf0419d6ae2c01b",
         "data":{
            "deployment_configuration":{
               "base_uri":"<https://api-dev.zi.mt/assets/>",
               "base_uri_extension":"/metadata",
               "maximum_supply":"2"
            },
            "initial_roles":[
               {
                  "role":"MINTER_ROLE",
                  "addresses":[
                     
                  ]
               },
               {
                  "role":"TRANSFER_ROLE",
                  "addresses":[
                     
                  ]
               },
               {
                  "role":"BURN_ROLE",
                  "addresses":[
                     
                  ]
               }
            ],
            "standard":"erc721",
            "type":"ERC721Roles",
            "name":"test3",
            "description":"test3",
            "symbol":"test3",
            "burnable":true,
            "transferable":true
         }
      },
      "data":{
         "current_roles":[
            {
               "role":"MINTER_ROLE",
               "addresses":[
                  "0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
               ]
            },
            {
               "role":"TRANSFER_ROLE",
               "addresses":[
                  "0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
               ]
            },
            {
               "role":"BURN_ROLE",
               "addresses":[
                  "0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
               ]
            }
         ],
         "properties":{
            "prop1":"value1"
         },
         "status":"not-deployed"
      },
      "receipt":{
         "id_hash":"0x0f5c71febe298c18a48f1cc68f35d13b1acbe1d6bbca7836a401627835fd57d9",
         "received_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
         "received_at":1670431267576,
         "organization_id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "ownership":{
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "hub_meta":{
         "analytics_resolved":true,
         "modified_at":1670431268
      }
   },
   "status":"ok"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Collection not found" %}
```json
{
    "status": "error",
    "message": "Collection not found",
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

***

## Deploy collection `**contract`\*\*

{% swagger method="post" summary="" baseUrl="https://api.zi.mt" path="/collections/:id/deploy/worker" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark> <mark style="color:blue;">**`ADMIN`**</mark>

Updates the collection with id provided in params with props for the worker to deploy the collection.

We send the payload same as for the direct deploy via the hub, using the worker endpoint we check if the collection exists and update it’s mutable data with

Worker checks for new collections for deployment, gets all with deploy = true, and a chain abbreviation, the rest is the same as the directly using the deploy endpoint.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" required="true" type="String" %}
Unique collection id
{% endswagger-parameter %}

{% swagger-parameter in="body" name="custodial" type="Boolean" required="true" %}
Type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="public_address" type="String" required="true" %}
Account address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="chain_abbreviation" type="String" required="true" %}
Chain name
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":{
      "proof":"0x3efa68fbe5aab9e8fb14afe95e1336e04dcccec1c3635b0d6871eb68a1181a182fab3973b3631b75fb4946911f3bac1dc09bb441c83cf90c88703d94595ba2ee1b",
      "id":"0xfcb0335483a271470ba3c5f43d67af3399a26c7fdb08c9caf6b0ab56bcdf4fbd",
      "object":{
         "meta":{
            "created_by":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
            "created_at":1670108540348,
            "data_hash":"0x391f490c38abd98272c106375607054aef1151629a6cada05c4104ede61b12ab"
         },
         "signature":"0x6385dc9992d7baec2c78410d7807660b18eec925f531860f9801e3787bf03c716e8e6cacdca2bbca3ba0cc1d67404ac4c0f0c45ab9c0fa0b095858cc0e5145e41b",
         "data":{
            "deployment_configuration":{
               "base_uri":"<https://api-dev.zi.mt/assets/>",
               "base_uri_extension":"/metadata",
               "maximum_supply":"0"
            },
            "initial_roles":[
               {
                  "role":"MINTER_ROLE",
                  "addresses":[
                     
                  ]
               },
               {
                  "role":"TRANSFER_ROLE",
                  "addresses":[
                     
                  ]
               },
               {
                  "role":"BURN_ROLE",
                  "addresses":[
                     
                  ]
               }
            ],
            "standard":"erc721",
            "type":"ERC721Roles",
            "name":"test",
            "description":"asdasdasd",
            "symbol":"teas",
            "burnable":true,
            "transferable":true
         }
      },
      "data":{
         "current_roles":[
            {
               "role":"MINTER_ROLE",
               "addresses":[
                  "0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
               ]
            },
            {
               "role":"TRANSFER_ROLE",
               "addresses":[
                  "0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
               ]
            },
            {
               "role":"BURN_ROLE",
               "addresses":[
                  "0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC"
               ]
            }
         ],
         "status":"deployed",
         "deployment":[
            {
               "status":"deployed",
               "chain_id":"0x5",
               "chain_name":"Ethereum Goerli Testnet",
               "contract_address":"0xD5B5A9cc97702536D718aE4047A700790B210E3C",
               "deployed_by":"0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC",
               "owned_by":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
               "transaction_hash":"0x1973efbbd175f4b37deaf0db37b106dc6a543deda9257f3c3b2a9ca3a175f877"
            }
         ]
      },
      "receipt":{
         "id_hash":"0x6e0e7dd7d9149ef8ab8ca1de4967e2adbbb8f6d00e087451e2f9659dc9cec0ff",
         "received_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
         "received_at":1670108540867,
         "organization_id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "ownership":{
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "hub_meta":{
         "analytics_resolved":true,
         "modified_at":1670343676
      }
   },
   "status":"ok"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Validation error",
    "errors": "object is required; request has invalid properties."
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="Unuthorized request" %}
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

```json
{
 "custodial":true,
   "ids":[
      "0xb15e1ca6849b17793d25d0006fcfcbd495e65d57ba9ef2b32f654d6c8bf68ac2"
   ],
   "chain_id":"0x13881",
   "owner_address":"0xB32A18AC09feF1e252f8F5519A6FF663517D28AB",
   "public_address":"0xB32A18AC09feF1e252f8F5519A6FF663517D28AB"
}
```

</details>

<details>

<summary>Schema details</summary>

**Deploy collection details**

In order to be able to deploy a collection, we have to create it first, using the [Create a new collection](https://www.notion.so/Create-a-new-collection-7e7a357eed3c4b328b138a147317c506?pvs=21) endpoint. After that, we’ll have to execute a `POST` request on the

As input parameters we have:

* `custodial`: boolean; if true - custodial deployment, otherwise non-custodial; for now is hardcoded to true
* organization id: string; id of the organization this collection belongs to
* deployment config: struct; configuration parameters for deploying this collection
  * `chain abbreviation`: string; for now we only support rinkeby, bsc\_testnet, mumbai and optimism\_goerli
  * `base URI`: string; can be set by the user; for now hardcoded to “[https://zi.mt/assets/”](https://zi.mt/assets/%E2%80%9D)
  * `base URI extension`: string; usually “.json” if NFTs metadata stored on IPFS; for now hardcoded to “/metadata”
  * `maximum supply`: string; 0 if collection has an unlimited supply,
  * `minter role`: array; addresses granted with minter role at the deployment time; can mint tokens for themselves or for any other address
  * `burn role`: array; addresses granted with burn role at the deployment time; can burn
  * `transfer role`: array;
* (\*)new deployment: struct; optional; only needed on non-custodial deployments; set by the front-end when a collection is deployed using MetaMask

</details>

***

**Asset properties**

If the asset contains the following properties worker will be triggered

```jsx
{
   "properties": {
	"worker": {
	   "deploy": true,
	   "chain_abbreviation": "bsc"
  	}
   }
}
```

***

## Mint collection asset

{% swagger method="post" path="/collections/:id/mint" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="custodial" type="Boolean" required="true" %}
Type
{% endswagger-parameter %}

{% swagger-parameter in="body" name="ids" type="Array<string>" required="true" %}
List of assets that will be minted
{% endswagger-parameter %}

{% swagger-parameter in="body" name="chain_id" type="String" required="true" %}
Chain id
{% endswagger-parameter %}

{% swagger-parameter in="body" name="owner_address" type="String" required="true" %}
Account address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="public_address" type="String" required="true" %}
Account address
{% endswagger-parameter %}

{% swagger-parameter required="true" in="path" name="id" type="String" %}
Unique collection id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorized" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
   "response":{
      "proof":"0x317a75cc7d990adf494dde1c3cd261e73f0028381ba834ad40f4cfad4baaaca733d394acf5b186cb22d88d39d642a5e0bdc1d6970bf2cf31409cf57ce3080c991c",
      "id":"0x397f4d31a6430c4e75e0b59c9264e8bf34754cc8950784ab67f2a4bd0d48b16c",
      "object":{
         "meta":{
            "created_by":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
            "created_at":1670431804467,
            "data_hash":"0x1b12eacf89aeda69fd080d559788c5537b14c15740dc76a8abfc27dd3fdb1219"
         },
         "signature":"0x1c0f6ff5338ba42081563071c002d1d6b6884a99808ff8d2df6f2cfb3fd89a201e184ebd45f329b842de5dd3da4be8be0f4881fc03b0f39e0dd58e25093760761c",
         "data":{
            "name":"test2",
            "type":"test2",
            "description":"test2",
            "collection_id":"0xfcb0335483a271470ba3c5f43d67af3399a26c7fdb08c9caf6b0ab56bcdf4fbd"
         }
      },
      "data":{
         "minted":true,
         "nft":{
            "minted_at":1670431841634,
            "chain_id":"0x5",
            "chain_name":"Ethereum Goerli Testnet",
            "contract_address":"0xD5B5A9cc97702536D718aE4047A700790B210E3C",
            "owner_address":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
            "external_id":"0x397f4d31a6430c4e75e0b59c9264e8bf34754cc8950784ab67f2a4bd0d48b16c",
            "token_id":"2",
            "transaction_hash":"0xf42cc9d5140e83d166518af88c6335ab2dc44486b1633d854a8c31680805a0e7"
         }
      },
      "receipt":{
         "id_hash":"0x9e449f24e5e2bcb1e2126d36b8740ac389d50d409ef755aaa709e4cdbd4904b2",
         "received_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
         "received_at":1670431805218,
         "organization_id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "ownership":{
         "account_id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90"
      },
      "hub_meta":{
         "analytics_resolved":true,
         "modified_at":1670431841
      }
   },
   "status":"ok"
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

```json
{
 "custodial":true,
   "ids":[
      "0xb15e1ca6849b17793d25d0006fcfcbd495e65d57ba9ef2b32f654d6c8bf68ac2"
   ],
   "chain_id":"0x13881",
   "owner_address":"0xB32A18AC09feF1e252f8F5519A6FF663517D28AB",
   "public_address":"0xB32A18AC09feF1e252f8F5519A6FF663517D28AB"
}
```

</details>

***
