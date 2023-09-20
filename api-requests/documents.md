# 🗃 Documents

Document is a small JSON document that is immutable.

It holds some media/file, document must have <mark style="color:blue;">`name`</mark>, <mark style="color:blue;">`type`</mark> and optionally <mark style="color:blue;">description</mark>.

## Document model

<details>

<summary>Document object</summary>

**Document data object `<Document>`**

```json
{
   "id": "string",
   "proof": "string",
   "object": "<Object>", // imutable
   "receipt": "<ReceiptObject>",
   "ownership": "<OwnershipObject>"
}
```

**Meta object `<MetaObject>`**

```json
{
  "created_by": "string", // required
  "created_at": "number", // required
  "data_hash": "string" // required
}
```

**Data object `<ObjectData>`**

```json
{
   "type":"string", // required
   "name":"string", // required
   "description":"string",
   "file_hash": "string",
   "url": "string",
   "identifiers": "object",
   "properties":"object",
}
```

**Ownership object `<OwnershipObject>`**

```json
{
  "id_hash":"string", 
  "received_by":"string", 
  "received_at":"number",
  "organization_id":"string",
  "account_id":"string" // required
}
```

**Receipt object `<ReceiptObject>`**

```json
{
    "account_id": "string",
    "privacy": {
        "protected": "boolean",
        "private": "boolean",
        "secret_token": "string"
    }
}
```

</details>

***

### **Document object** `<Document>`

<table><thead><tr><th width="135">Name</th><th width="241">Type</th><th width="77">Required</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>string</td><td>true</td><td>The unique ID of the document.</td></tr><tr><td>proof</td><td>string</td><td>true</td><td>A crypto-secure proof of the document’s authenticity and ownership.</td></tr><tr><td>object</td><td><strong>&#x3C;ObjectData></strong></td><td>true</td><td>The immutable data of the document, including its type, name, file hash, and other metadata.</td></tr><tr><td>receipt</td><td><strong>&#x3C;ReceiptObject></strong></td><td>/</td><td>A receipt indicating that the document has been received by the organization and its ownership has been established. Generated by the system</td></tr><tr><td>ownership</td><td><strong>&#x3C;OwnershipObject></strong></td><td>/</td><td>The ownership information for the document, including the account ID of the owner. Generated by the system</td></tr></tbody></table>

***

### Object `<Object>`

<table><thead><tr><th width="120">Name</th><th width="197">Type</th><th width="81">Required</th><th>Description</th></tr></thead><tbody><tr><td>meta</td><td><strong>&#x3C;MetaObject></strong></td><td>true</td><td>The metadata for the document, including the timestamp of its creation and the hash of its data.</td></tr><tr><td>data</td><td><strong>&#x3C;ObjectData></strong></td><td>true</td><td>The immutable data of the document, including its type, name, file hash, and other metadata.</td></tr><tr><td>signature</td><td>string</td><td>false</td><td>A signature that verifies the authenticity of the document data.</td></tr></tbody></table>

***

#### Meta object `<MetaObject>`

<table><thead><tr><th width="144">Name</th><th width="95">Type</th><th width="108">Required</th><th></th></tr></thead><tbody><tr><td>created_by</td><td>string</td><td>true</td><td>The address of the creator of the document.</td></tr><tr><td>created_at</td><td>number</td><td>true</td><td>The timestamp of the document’s creation.</td></tr><tr><td>data_hash</td><td>string</td><td>true</td><td>The hash of the document’s data.</td></tr></tbody></table>

***

#### **`<ObjectData>` Data ( immutable )**

<table><thead><tr><th width="146">Name</th><th width="192">Type</th><th width="93">Required</th><th>Description</th></tr></thead><tbody><tr><td>type</td><td>string</td><td>true</td><td>The type of the asset, formatted as a dot-separated string.</td></tr><tr><td>name</td><td>string</td><td>false</td><td>The name of the event.</td></tr><tr><td>description</td><td>string</td><td>false</td><td>A description of the event.</td></tr><tr><td>documents</td><td>Array &#x3C;Document></td><td>false</td><td>Array of documents attached to this event.</td></tr><tr><td>identifiers</td><td>Object &#x3C;Identifiers></td><td>false</td><td>Identifiers associated with the document.</td></tr><tr><td>properties</td><td>Object &#x3C;Properties></td><td>false</td><td>Properties associated with the document.</td></tr></tbody></table>

***

### Receipt object `<ReceiptObject>`

<table><thead><tr><th width="167">Name</th><th width="96">Type</th><th width="78">Required</th><th></th></tr></thead><tbody><tr><td>id_hash</td><td>string</td><td>true</td><td>The hash of the document’s ID.</td></tr><tr><td>received_by</td><td>string</td><td>true</td><td>The address of the account that received the document.</td></tr><tr><td>received_at</td><td>number</td><td>true</td><td>The timestamp of the document’s receipt.</td></tr><tr><td>organization_id</td><td>string</td><td>false</td><td>The ID of the organization that received the document.</td></tr><tr><td>account_id</td><td>string</td><td>true</td><td>The ID of the account that received the document.</td></tr></tbody></table>

***

### **Ownership object** `<OwnershipObject>`

<table><thead><tr><th width="144">Name</th><th width="96">Type</th><th width="106">Required</th><th></th></tr></thead><tbody><tr><td>account_id</td><td>string</td><td>false</td><td>Document owner’s account id.</td></tr><tr><td>privacy</td><td>object</td><td>false</td><td>Document’s privacy settings.</td></tr></tbody></table>

***

## Create document

{% swagger method="get" path="/documents" baseUrl="https://api-documents.zi.mt" summary="" %}
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

{% swagger-parameter in="body" name="object" type="Object" required="true" %}
Immutable document data
{% endswagger-parameter %}

{% swagger-parameter in="body" name="file" type="File" required="false" %}
FIles[0] / FormData
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0x9cd1679fe040d73bd96630315513772f945f2e6f28c9eae3db15b28e8e49927a1e2fa22aac01cd6489168eb2505b5c6b7787c539ea14a81b90b66054745175481b",
        "id": "0x8617fc6abeb2c812d8bcf816f1b373f03bf5480ca5ed79c425f8382b6e599193",
        "receipt": {
            "id_hash": "0xbd89288d5fd12f2a74a1c1cc837abc50893ee9480874e9a7504ef61c929fb158",
            "received_by": "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC",
            "received_at": 1677799429558,
            "organization_id": "0x094c45c967a9ff3e2ea5bc6bd21021353059aeef0f169c622f7fe09b02508e6d",
            "account_id": "0x28ef33aa0109d818d9d703637ad8d2e8f95d8e567a5d07711fb3a882979ea6e5"
        },
    },
    "status": "ok",
    "message": "Created"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error", 
    "message": "Invalid properties"
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
{% endswagger %}

<details>

<summary>Body schema</summary>

```json
{
   "object":{
      "meta":{
         "created_by":"0x07B7FF00678c917EF1995B264E0e09171D25A5FE",
         "created_at":1683269271000,
         "data_hash":"0xa493237bcfc9745cb5323879435804bd0e38e72eb213c03ed0224ab9a1e0140b"
      },
      "data":{
         "type":"image/jpeg",
         "name":"0x66783794ef40c1a598df31906a039bd5423be48ff3e9346ddc8a0ce671ea6614",
         "file_hash":"0x093f6d69185107f8940ccd4617e866af30a90211631952b15686787d03e397f1",
         "properties":{
            "size":1356885
         }
      },
      "signature":"0xeee1205ed1a7352b58731f0ccd575a80ade5a9f359feca85e488c8baa980202672d8688d6ed92431de66ebf8174046f2ae20661b0eb31d54307578e6e053b2051c"
   }
}
```

</details>

***

## Get single document

{% swagger method="get" path="/documents/:id" baseUrl="https://api-documents.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">**`AUTHORIZED`**</mark> <mark style="color:blue;">**`ADMIN`**</mark>

The `GET` request to `/documents/:id` retrieves a single document with the ID specified in the request.

The request payload should include the ID of the document to retrieve.

The response includes the proof of the document's authenticity and ownership, as well as the receipt and ownership information for the document.
{% endswagger-description %}

{% swagger-parameter in="path" name="id" required="true" type="String" %}
Unique document id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String " required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0x9cd1679fe040d73bd96630315513772f945f2e6f28c9eae3db15b28e8e49927a1e2fa22aac01cd6489168eb2505b5c6b7787c539ea14a81b90b66054745175481b",
        "id": "0x8617fc6abeb2c812d8bcf816f1b373f03bf5480ca5ed79c425f8382b6e599193",
        "object": {
            "meta": {
                "created_by": "0x8F19141719fe0cE897fdC3389dc8d2F6Aa013398",
                "created_at": 1677799428,
                "data_hash": "0x4076d029bd3d570e926f48a543c2c47ba579caed77b88cae8ec87d5c51846edc"
            },
            "data": {
                "type": "image/jpeg",
                "name": "anna-meshkov-8jTun57jS_U-unsplash",
                "file_hash": "0xf7643f89ca734861baf0ca54889f05b51c2753379b7e91c95623203263fb8447",
                "properties": {
                    "size": 1129318
                }
            },
            "signature": "0xc760f09d112bd85fb45ea826b474a7d45b6b00c0ea1659c7447ccb73327c9b5f4c0477f970dc0b627dc859fbe04130a68149f9961402f3a6bd9bc4bf3b5883561c"
        },
        "data": {
            "type": "image/jpeg",
            "name": "anna-meshkov-8jTun57jS_U-unsplash"
        },
        "receipt": {
            "id_hash": "0xbd89288d5fd12f2a74a1c1cc837abc50893ee9480874e9a7504ef61c929fb158",
            "received_by": "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC",
            "received_at": 1677799429558,
            "organization_id": "0x094c45c967a9ff3e2ea5bc6bd21021353059aeef0f169c622f7fe09b02508e6d",
            "account_id": "0x28ef33aa0109d818d9d703637ad8d2e8f95d8e567a5d07711fb3a882979ea6e5"
        },
        "ownership": {
            "account_id": "0x28ef33aa0109d818d9d703637ad8d2e8f95d8e567a5d07711fb3a882979ea6e5"
        },
        "hub_meta": {
            "analytics_resolved": true,
            "modified_at": 1677799432
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

{% swagger-response status="404: Not Found" description="Document not found" %}
```json
{
    "status": "error",
    "message": "Document not found"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Search documents

{% swagger method="post" path="/documents/search" baseUrl="https://api-documents.zi.mt" summary="" %}
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

{% swagger-parameter in="body" name="query" type="Object" required="true" %}
Search query schema
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": [
        {
            "proof": "0xf197d2575c02a1265f313d43f6c6e280db1d1d1fe3607302f152b09dec81829e27c349450c6f7013614f24594e8b01ffbd1a8be835b0d09f4ef49bd4d1c6f7211b",
            "id": "0x11412a7ee3085b321f188ebf6e354ba8a69105171319f9a83ada542cb4395987",
            "object": {
                "meta": {
                    "created_by": "0x8F19141719fe0cE897fdC3389dc8d2F6Aa013398",
                    "created_at": 1677800987,
                    "data_hash": "0x22e4162d0da3ca61fd1287212c3a3e9c6d18e11d06b5087513dd3c6b4395bcab"
                },
                "data": {
                    "type": "image/jpeg",
                    "name": "alex-sh-USzLPpZ3-3o-unsplash",
                    "file_hash": "0xab3ad277a66f61d645f010ab9d8f2f5b317c80ba04121729052b66332b76b740",
                    "properties": {
                        "size": 8627134
                    }
                },
                "signature": "0x4942e3eab768958ccd6c891fc40669b6c232d2c6e0a6bea036b199d14b7b9c9417ebd62885ec9139aee950e7719fe6a4dce9dd999cd59b9d2e304983b16a43cc1b"
            },
            "data": {
                "type": "image/jpeg",
                "name": "alex-sh-USzLPpZ3-3o-unsplash"
            },
            "receipt": {
                "id_hash": "0x003cfd2cc72e777c6187b1d27fc92519a7bb6710bcb11f1fc7f0cb8551527a05",
                "received_by": "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC",
                "received_at": 1677800996308,
                "organization_id": "0x094c45c967a9ff3e2ea5bc6bd21021353059aeef0f169c622f7fe09b02508e6d",
                "account_id": "0x28ef33aa0109d818d9d703637ad8d2e8f95d8e567a5d07711fb3a882979ea6e5"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1677801006
            },
            "ownership": {
                "account_id": "0x28ef33aa0109d818d9d703637ad8d2e8f95d8e567a5d07711fb3a882979ea6e5"
            }
        }
    ],
    "status": "ok",
    "pagination": {
        "next": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3MTU2NjAwNDY4fQ==",
        "previous": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3ODAwOTk2MzA4fQ==",
        "hasNext": true,
        "size": 30,
        "skip": 0,
        "limit": 30
    }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Quire parameter has invalid properties"
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
{% endswagger %}

<details>

<summary>Body schema</summary>

```json
{
  "query": {
    "documents": [
      {
        "field": "object.data.type",
        "operator": "equal",
        "value": "image/jpeg"
      }
    ]
  }
}
```

</details>

***

## Download file (buffer)

{% swagger method="get" path="/documents/download/:id" baseUrl="https://api-documents.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**ADMIN**

</mark>
{% endswagger-description %}

{% swagger-parameter in="path" name="id" required="true" type="String" %}
Unique document id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "file": "Uint8Array[]", // buffer
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Document not found" %}
```json
{
    "status": "error",
    "message": "Document not found"
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
{% endswagger %}

***