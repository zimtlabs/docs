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

# Accounts

An **account** is a generated wallet that has some rights to interact with data/functions in an organisations

Organizations can create a new accounts

* Private key, Address and password of account will be visible for the organization owner
* Private keys can be inserted manually by the organization
* Account password can not be changed by organization owner
* Number of accounts are defined in the current organization plan
* Accounts can login to the system **only via API** and **not through dashboard**

Organization admins can **change profile settings** and **disable** accounts from the system.

{% hint style="danger" %}
Hub only keeps track of **public keys**, never the private keys in clear (encrypted via the password by the user and stored with our additional encrypting layer )
{% endhint %}

There’s no “password reset” functionality for users.

A user can change their password only if they are authorized and only if they provide their current password and a new desired password. In that case the old password which is encrypted with user’s private key is sent to our backend for validation, if the hash is valid we then change the user’s password.

Passwords must contain:

* Eight characters at least
* At least 1 uppercase letter
* At least 1 lowercase letter
* At least 1 number
* At least 1 special character

Organization admins **can not** regenerate the Keys of an account.

Accounts currently have only one single address.

Each organization can have _1..N_ users in it defined by the current plan.

{% hint style="warning" %}
👷‍♂️ **TODO**: (user is a pub key + optional email, name, etc. & co) - both the pub key & email are unique IDs in the organization (but users in diff organizations can have same email and/or key) - they’re not unique in the system
{% endhint %}

{% hint style="info" %}
Each request to the API must be scoped to an organization, which can be done via an API key or Auth token.
{% endhint %}

***

## Create

{% swagger method="post" path="/accounts" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="object" type="Object" required="true" %}
Immutable object
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}
Object data
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

<details>

<summary>Body schema</summary>

Data object

```json
{
    "object": {
        "signature": "0xbf20b38cdff4674739d80fe8fa7a06ce33fa7bb81796a92574bdff548ecf67746596ea6b461713bb133e8ae0fd1bc222f1a74bbe496f4c63939bde07d1338f51c",
        "meta": {
            "created_by": "0x7068495356D9CDee3EB6E61a20c154aBc3405f97",
            "created_at": 1655732340225
        }
    },
    "data": {
        "full_name": "User 29",
        "address": "0xb6B6B7dF1c353050eBEFc2B670DA46d5686bd41D",
        "email": "user29@zimt.co",
        "security_token": "U2FsdGVkX1+AkfgkvZRK5pAprt1g7mfIlFnvfrQHYP8Jt5i8cQjhw/4Dmhnn3eWSGMKeDD",
        "organization_id": "0xe16b6c9ed08238669ee32e35f30bd5ab23d5e65d72ee1f211bb8cf2f008e60e2"
    }
}
```

</details>

***

## Update

{% swagger method="put" path="/accounts/:id" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="id" type="String" required="true" %}
Unique account id
{% endswagger-parameter %}

{% swagger-parameter in="body" name="data" type="Object" required="true" %}

{% endswagger-parameter %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique account id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}

{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}

{% endswagger-response %}
{% endswagger %}

<details>

<summary>Body schema</summary>

**Data object**

```json
{
    "data":{
        "full_name": "string",
        "short_name": "string",
        "settings":{
            "about": {},
            "socials": {},
            "billing": {},
            "avatar": {},
            "cover": {}
	},  
    }
}

```

**About details**

```json
{ 
   "about": {
      "bio": "Bio",
      "company": "Double Cream",
      "interests": [
        "crypto",
        "nft"
      ],
      "location": {
        "address": "Swtes"
      },
      "website": "doublecream.co"
  }
}
```

**Avatar / Cover photo**

<pre class="language-json"><code class="lang-json">{
  "avatar": {
    "id": "0x58fc2933c61eaf714fed706900f634ceb75258dd03e317e5bf88717b642f2566",
    "name": "alex-sh-USzLPpZ3-3o-unsplash",
    "properties": {
      "avatar": true,
      "size": 8627134
    }
<strong>  }  
</strong>}  
</code></pre>

Billing

```json
{
  "billing": {
    "4778b5ab-275c-4e77-86d1-62e69a1fa3d9": {
      "city": "City",
      "company": "Double Cream",
      "country": "Country",
      "email": "dc@mailinator.com",
      "full_name": "Double Cream",
      "id": "4778b5ab-275c-4e77-86d1-62e69a1fa3d9",
      "primary": true,
      "street": "Doublecream address",
      "zip": "123123123"
    }
}
```

**Social media**

```json
{
  "socials":
    {
    "discord": "",
    "facebook": "",
    "instagram": "",
    "linkedin": "",
    "twitter": "doublecream.co",
    "youtube": ""
  }
}

```

**Password change**

<pre class="language-json"><code class="lang-json"><strong>{
</strong>    "data": {
        "security": {
            "tokenCurrent": "U2FsdGVkX1+f/ETFW0Q45a74cdHBMNCUcE9pBvnDn0fomsev6Lp6S4ENZJs3rSRQA8kl/Z", // current password, encrypt(private_key, password)
            "token": "U2FsdGVkX1+AkfgkvZRK5pAprt1g7mfIlFnvfrQHYP8Jt5i8cQjhw/4Dmhnn3eWSGMKeDDfm+SfIN27auZGzHDIZ4/b63zJdFOstnSPWpqU8zFnkzD/eXiEwn+gTWzYy" // new password, encrypt(private_key, new_password)
        }
    }
}
</code></pre>

</details>

***

## Delete account

{% swagger method="delete" path="/accounts/:id" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique account id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "status": "success",
    "message": "Removed"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Not found" %}
```json
{
    "status": "error",
    "message": "Not found"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get many

{% swagger method="get" path="/accounts" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": [{...}],
    "status": "ok",
    "pagination": {
        "next": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI0NDQwNDIxfQ==",
        "previous": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjkyMTMwMTgzMDM1fQ==",
        "hasNext": false,
        "size": 11,
        "skip": 0,
        "limit": 10000
    }
}
```

<pre class="language-json"><code class="lang-json">{
    "response": [
<strong>        {
</strong>            "data": {
                "address": "0x99C64D6199d8dC3A4B311bdac00826B5777842da",
                "full_name": "test123",
                "short_name": "test123321@mailinator.com",
                "email": "test123321@mailinator.com",
                "organization_id": "0xe7e5a508302a13cfa5342f46dcdcbb5e0379f1bf467b9e5229870bfac586f4ec",
                "active": true
            },
            "proof": "0xbc27c5ef5953938ad2e1c9829cb89bf37d22dcf8ee08da78c4d8d1ea4123c48a0031c8cc4e1c0be2e6fa51f37a3c7025f3ce0f1b9ce4ef924366a6fef8f5d1de1c",
            "id": "0x5f9a89277a070d3f661c615b230eb5615df1286b8354eeeafbdf48370f0511bc",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1692130183026
                },
                "signature": "0xf7654af2c0c66d6e9d8a1983e827c5768cc452725af6cacf2d7493ecd58a7fa64a7122d17874d8be553ed61dfe86efc30d8236a94c4d633cb419d50e0e244ffa1c"
            },
            "receipt": {
                "id_hash": "0x6b9a4c91e91e080c92ffc34e45a07c0bcbc075d2be39d2d951a8083bea876293",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1692130183035,
                "organization_id": "0xe7e5a508302a13cfa5342f46dcdcbb5e0379f1bf467b9e5229870bfac586f4ec",
                "account_id": "0xb63c299875f0dc98b02c9cb1578d1c7ab48a03a929ddc8b5d01cf6ed6f4717cd"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "views": 0,
                "modified_at": 1692130183
            }
        },
        {
            "data": {
                "address": "0xde7969d09936EE1BD7e26070842E67f7bF598C40",
                "full_name": "Swiss wine",
                "short_name": "swiss-wine",
                "email": "swisswine@mailinator.com",
                "organization_id": "0xd1b65f2fe357c05e7acfc3cc3721dfcdbe8822606fa5556ce0262c24cf81b180",
                "active": true
            },
            "proof": "0xc209efcf306e32a806c9ac53d25a5ab02b82a74815aafb64adf88ba33d85725c7c010bb7ddeb59807c8012a96fa34f40e0e8ca1e4b4f2f3c66acdba2beddb4ad1b",
            "id": "0x1eb92f7d2a9982128a05f010028c4e1e5b43d77f1be80b32f662533dff4f6081",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1691654409553
                },
                "signature": "0xd583819d5962cd3a1f22c2015eab37f6f4fa32aea408d1dc3503db2c3458e86d1a236f4fcf55f8a7000cbd082106cd97d51a397fbdcb3cabe2c4ff8ccf9038851c"
            },
            "receipt": {
                "id_hash": "0x85b49637cb15aa08f1137af17203bc44795cb91d0e1f9f8dbfe736ee172e233c",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1691654409556,
                "organization_id": "0xd1b65f2fe357c05e7acfc3cc3721dfcdbe8822606fa5556ce0262c24cf81b180"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "views": 0,
                "modified_at": 1691654409
            }
        },
        {
            "data": {
                "address": "0xfa17fc61Da37fc5EE6cbFb0EB93a0714a1972475",
                "full_name": "The Diligence",
                "short_name": "the-diligence",
                "email": "milos@zimt.co",
                "organization_id": "0xd8c88f906252635db67285f43df70510015c541b6caf358521fda3c879b9db66",
                "active": true
            },
            "proof": "0x357fb2683fa603d25620e64e780109566bad83e603befb22907112e2a56e0c733ba62942b896f024d669a526a909612885c49197fa00e837c8f39780c9cef55c1c",
            "id": "0x7f2cb93cc17006afe4b5f41f3c42c002bb099db005c5ce38d647ccf8db6e1aa9",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1686574853702
                },
                "signature": "0x8e411548dc4f00a8e952e2a4f2f1516d4849088b6fea360cb5e685c26dd6cb7c0485d89b8fc93644ebb0e82215160b093418dd005141a78565fc03533817af701c"
            },
            "receipt": {
                "id_hash": "0xab244fe0aa29055165953d8de084a544ce7531b5d75ab048183cf80cbd249d21",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1686574853707,
                "organization_id": "0xd8c88f906252635db67285f43df70510015c541b6caf358521fda3c879b9db66"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1686574854
            }
        },
        {
            "data": {
                "address": "0x9477591A6D4221d62eE350f90CfCcB15Ef5BF1E1",
                "full_name": "foodcrafters",
                "short_name": "foodcrafters_477",
                "email": "v@foodcrafters.org",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "active": true
            },
            "proof": "0xc37866c89f3859bcf39a628ea7ca9868d9a7c8e1f86fb9f23e0286b35853a51a69e4bbf6b6779734437bde208ce8a143817c9e6e3857b08b2bf417fde2dedf7c1b",
            "id": "0xd3a5cde8047dbd719e717b39f1bc8263140ffd612db8f5d870f91028888d7c47",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1681385683801
                },
                "signature": "0x7d0adb696aab2c8865a9b61e96d54559bfe2bd2b10c866e1305724cbf57f01195573f36286887250fc6893239d3c47859dd6cea94615736a3a8f19c90a7b96ba1b"
            },
            "receipt": {
                "id_hash": "0xc06c956d2e34da3ff8cd514215809ff292a688a73fde230bd8ba8ce5d4e22e68",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1681385683806,
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1681385684
            }
        },
        {
            "data": {
                "address": "0x3e0A11C9C2ef65bAdF1C7c8E1b68691A871659c1",
                "full_name": "milos_test",
                "short_name": "milos_test_e0A",
                "email": "milos_test@mailinator.com",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "active": true,
                "settings": {
                    "about": {
                        "bio": "I am an alien.",
                        "company": "Testing",
                        "interests": [
                            "testing",
                            "sports"
                        ],
                        "location": {
                            "address": "Moon"
                        },
                        "website": "www.testing.com"
                    },
                    "avatar": {
                        "name": "4512826-mob-psycho-100-anime",
                        "properties": {
                            "avatar": true,
                            "size": 634798
                        },
                        "id": "0x9db4aeefc435a63489865613b659e1b5429a0d07bcebc895e41b37b403106577"
                    },
                    "socials": {
                        "discord": "tester",
                        "facebook": "tester",
                        "instagram": "tester",
                        "linkedin": "tester",
                        "twitter": "tester",
                        "youtube": "tester"
                    },
                    "billing": {
                        "fbd8380b-edca-45ac-b6f5-fe2ae70137f7": {
                            "city": "Belgrade",
                            "company": "Testing",
                            "country": "Serbia",
                            "email": "milos_test@mailinator.com",
                            "full_name": "Testing Test",
                            "id": "fbd8380b-edca-45ac-b6f5-fe2ae70137f7",
                            "primary": true,
                            "street": "City Street 73",
                            "zip": "11000"
                        }
                    }
                }
            },
            "proof": "0x57740bef9eac657c6a17cd22b31b2bab02f18975421ccc97e471c19a85fba5ca1009151df6a74973d3e6696b8980139ff8ada6f5885b8faec4e859c064fdf36f1b",
            "id": "0xf67b5e8740493c5c5ce48b73eb1f0929eedcb803fe2b5f50fa7a133237a465c7",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1680615794277
                },
                "signature": "0xebdf28c1a0107be5e4d61ea0354895b200fa863249f2b14193e7302fae54550d0ac52cca23a421d09e03e379baab38a51153046c5400e172fc7af3553d51a2e81c"
            },
            "receipt": {
                "id_hash": "0xb89a28248677cce460695299ba8bebf40eee12a102708716d05e8237e4fdc67f",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1680615794281,
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1691665448
            }
        },
        {
            "data": {
                "address": "0xBCb350693e3B4d91F5a0B22bD8201a137Bc80d57",
                "full_name": "gaelcorboz",
                "short_name": "gaelcorboz_Cb3",
                "email": "gael.corboz@gmail.com",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "active": true
            },
            "proof": "0x5aea2875e4c08af4baa3dca0f121fe25bfe9d96eadbeaf18dc50566323a03815493f53f5e3a67949b763227ee9dc37a0e354d4977b5d00cdf6022d890de57e121b",
            "id": "0xa353565378e823e93f7bddd34d9edf9f5f02dcb3900a8f78c153ecc4b0f3324b",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1680539618649
                },
                "signature": "0x4833731a2dc12aa6f7d8c17795d239caa616873605d4d810a7122463f91aa19933fed42c71b2390cc9f845625f18ffde57d77f43405bfbaf1693b837038138401b"
            },
            "receipt": {
                "id_hash": "0x65c886dd31d4a8c21c905f03d39704e9f453f1fbfa8091a245c8884ede2db93c",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1680539618653,
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1680539619
            }
        },
        {
            "data": {
                "address": "0x58C07fcCDd4ab44015bA0Ca46B6f77d0AA59F5d9",
                "full_name": "julien",
                "short_name": "julien_8C0",
                "email": "hi@julien.pro",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "active": true
            },
            "proof": "0x279e4eae4a45baccd498a47dea022948c52e2a63deaf06f941a5b185b192aad752b0f57f5d486d91c60a0ade4ea082e2ad602e421aaf43f6f5f21457e7453fe31c",
            "id": "0xb0c574da9c6702998336dc5ed9fc48f56716943c17467475b88593a739ecef46",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1680538936048
                },
                "signature": "0xcdb3f01813e70d7a01fd3a52e1dc813dc2dc1c7396b3752b6f389019b354e70d2574db44cc3fca44f9011dd62d0eafebb09a5933e40968e3caf5e0050e18621e1c"
            },
            "receipt": {
                "id_hash": "0x376d3de76f3d093b89681bb7db85846654a277a42722b989c7b8aba0934859c2",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1680538936057,
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1680538936
            }
        },
        {
            "data": {
                "address": "0xB29b32C73065e40b7Ca805C9a65608fbeE1f2DB7",
                "full_name": "test1100",
                "short_name": "test1100_29b",
                "email": "test1100@mailinator.com",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "active": true
            },
            "proof": "0x1c63206fd9fb03c9841fd804f00535fe580de03a855078b39fef9b26d6fa4c8414487b1bb5bbcb9ade2859da3a439e50dbfe7f315f47964ec67ebfa74982bf021b",
            "id": "0x570840a53cecf427213ed2a83119d968c4c7c901ccc90b7d0a6dcb39f84623b4",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1678381116830
                },
                "signature": "0xe7324f721a49bb7eb03650e1c716d1dcbf267f6701caa50cb523218ce0af58b0330a9dc7c3a6021f3f9363fb656e0ea3e2ed84a4063a7256beb3a70cf9d4dcf71c"
            },
            "receipt": {
                "id_hash": "0x0404c0dc01d2144f21188b3638b1cba796b4dacaa8a05627e47d1f805f140c38",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1678381116836,
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1678381117
            }
        },
        {
            "data": {
                "address": "0x168D5B47868b0c3dD643B096a61A67E1a58bA650",
                "full_name": "Mladen Milicevic",
                "short_name": "jomlamladen",
                "email": "milicevicdj@gmail.com",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "active": true,
                "settings": {
                    "about": {
                        "bio": "test",
                        "company": "Digital Bridges",
                        "interests": [],
                        "location": {
                            "address": "Serbia"
                        }
                    },
                    "socials": {
                        "discord": "",
                        "facebook": "",
                        "instagram": "",
                        "linkedin": "",
                        "twitter": "",
                        "youtube": ""
                    },
                    "avatar": {
                        "id": "0x10fe95057da0b50e48b52065eb4b9f508faf446c6d0f0fb19849333dda7a2acc",
                        "name": "DB-logo-new",
                        "properties": {
                            "avatar": true,
                            "size": 22900
                        }
                    }
                }
            },
            "proof": "0xb3e9bfa5789ef9540d5e54a93fc7cf05db8b9a61884c08ed6b42edce9cb751d73cd3f76422a099e521ff98c39b252c55d836e5d30685b38145c3187e7c5abcc81c",
            "id": "0x2dc7861fde31c6dbba9154f3e3dcc8ca84bb28502af355a6130b341cc0bcce28",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1677526979954
                },
                "signature": "0xd8e7da052f03fd0312dc67aad8922969137d0a3b218d774baf6db453ac43138703de90e8d6fd017cb343deb0628e3c37bfc63839167535075d34a0a1248cb7ce1c"
            },
            "receipt": {
                "id_hash": "0xac44ad4eed73513071964f04f732bde234287e5f8ff9372c96fe9cae8e5c91f8",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1677526979963,
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4"
            },
            "hub_meta": {
                "analytics_resolved": true,
                "modified_at": 1677762890
            }
        },
        {
            "data": {
                "address": "0x6424c4EB1a98c8C1e818961203779620E6b5f0ef",
                "full_name": "Vlad Trifa",
                "short_name": "vlad-trifa_424",
                "email": "vlad@zimt.co",
                "organization_id": "0xe7e5a508302a13cfa5342f46dcdcbb5e0379f1bf467b9e5229870bfac586f4ec",
                "active": true
            },
            "proof": "0x3c388bdc865051dc7faf849f123db22834da1b3ecc5a4a9bba41921250db0fb7349210738a11c82ec95f8178baa99a2806d1f24247caeb0f6c153d7a346f5d541c",
            "id": "0xb63c299875f0dc98b02c9cb1578d1c7ab48a03a929ddc8b5d01cf6ed6f4717cd",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1677524440413
                },
                "signature": "0xab47ec00b8ab442fc8604a23bac96fea3034b63ed08d25be9a8d773d5eabf5df4d70b584330e7abf5c329c61afb947dfc4f44dc7a15d1683900fba74da9ff8cc1b"
            },
            "receipt": {
                "id_hash": "0x6cbb5efc776240c2f4e17deb38ccd9f81f99a9bf2a3e5bb15ea2a3c6de9b095b",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1677524440421
            },
            "hub_meta": {
                "modified_at": 1692895954
            }
        }
    ],
    "status": "ok",
    "pagination": {
        "next": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI0NDQwNDIxfQ==",
        "previous": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjkyMTMwMTgzMDM1fQ==",
        "hasNext": false,
        "size": 11,
        "skip": 0,
        "limit": 10000
    }
}
```
</code></pre>
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

## Get single

{% swagger method="get" path="/accounts/:id" baseUrl="https://api.zi.mt" summary="" %}
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

{% swagger-parameter in="path" name="id" type="String" required="false" %}
Unique account id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0xfe552d91787a4b477c10d37755b106c74d274df3b3d5dca70cf1748adda4ff360c03b6157f9b50fc0eee6a7ca9553632bd0c5abfa7f8b1d400fa69225c8451021b",
        "id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4",
        "object": {
            "meta": {
                "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "created_at": 1677524541298
            },
            "signature": "0x76a67241d9ac7e3d6f0cea05ee388848d97a1635599459d0d4b0577159307b8a064682d76971b38f5fb0b1c967610973e0aed116a99034159b619f645ce638691c"
        },
        "data": {
            "address": "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
            "full_name": "Double Cream",
            "short_name": "doublecream",
            "email": "dc@mailinator.com",
            "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
            "active": true,
            "settings": {
                "about": {
                    "bio": "Bio",
                    "company": "Double Cream",
                    "interests": [
                        "crypto",
                        "nft"
                    ],
                    "location": {
                        "address": "Swtes"
                    },
                    "website": "doublecream.co"
                },
                "socials": {
                    "discord": "",
                    "facebook": "",
                    "instagram": "",
                    "linkedin": "",
                    "twitter": "doublecream.co",
                    "youtube": ""
                },
                "billing": {
                    "4778b5ab-275c-4e77-86d1-62e69a1fa3d9": {
                        "city": "City",
                        "company": "Double Cream",
                        "country": "Country",
                        "email": "dc@mailinator.com",
                        "full_name": "Double Cream",
                        "id": "4778b5ab-275c-4e77-86d1-62e69a1fa3d9",
                        "primary": true,
                        "street": "Doublecream address",
                        "zip": "123123123"
                    }
                },
                "avatar": {
                    "id": "0x58fc2933c61eaf714fed706900f634ceb75258dd03e317e5bf88717b642f2566",
                    "name": "alex-sh-USzLPpZ3-3o-unsplash",
                    "properties": {
                        "avatar": true,
                        "size": 8627134
                    }
                },
                "cover": {
                    "id": "0x4d82ad421f6400f7b27c47249f4554e9033066fa10b7083d88de59bc0e250221",
                    "name": "_MG_3775_obrada",
                    "properties": {
                        "avatar": true,
                        "size": 9285838
                    }
                }
            }
        },
        "receipt": {
            "id_hash": "0x619d5914928361427c30f426f42dbba0601fda7cff6b602ee6d6f32eda527b14",
            "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at": 1677524541301,
            "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0"
        },
        "hub_meta": {
            "analytics_resolved": true,
            "modified_at": 1680693530
        }
    },
    "status": "ok"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Not found" %}
```json
{
    "status": "error",
    "message": "Account not found"
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

## Get single public

{% swagger method="post" path="/accounts/:id/public" baseUrl="https://api.zi.mt" summary="" expanded="false" %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique account id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0xfe552d91787a4b477c10d37755b106c74d274df3b3d5dca70cf1748adda4ff360c03b6157f9b50fc0eee6a7ca9553632bd0c5abfa7f8b1d400fa69225c8451021b",
        "id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4",
        "object": {
            "meta": {
                "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "created_at": 1677524541298
            },
            "signature": "0x76a67241d9ac7e3d6f0cea05ee388848d97a1635599459d0d4b0577159307b8a064682d76971b38f5fb0b1c967610973e0aed116a99034159b619f645ce638691c"
        },
        "data": {
            "address": "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
            "full_name": "Double Cream",
            "short_name": "doublecream",
            "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
            "active": true,
            "settings": {
                "about": {
                    "bio": "Bio",
                    "company": "Double Cream",
                    "interests": [
                        "crypto",
                        "nft"
                    ],
                    "location": {
                        "address": "Swtes"
                    },
                    "website": "doublecream.co"
                },
                "socials": {
                    "discord": "",
                    "facebook": "",
                    "instagram": "",
                    "linkedin": "",
                    "twitter": "doublecream.co",
                    "youtube": ""
                },
                "billing": {
                    "4778b5ab-275c-4e77-86d1-62e69a1fa3d9": {
                        "city": "City",
                        "company": "Double Cream",
                        "country": "Country",
                        "email": "dc@mailinator.com",
                        "full_name": "Double Cream",
                        "id": "4778b5ab-275c-4e77-86d1-62e69a1fa3d9",
                        "primary": true,
                        "street": "Doublecream address",
                        "zip": "123123123"
                    }
                },
                "avatar": {
                    "id": "0x58fc2933c61eaf714fed706900f634ceb75258dd03e317e5bf88717b642f2566",
                    "name": "alex-sh-USzLPpZ3-3o-unsplash",
                    "properties": {
                        "avatar": true,
                        "size": 8627134
                    }
                },
                "cover": {
                    "id": "0x4d82ad421f6400f7b27c47249f4554e9033066fa10b7083d88de59bc0e250221",
                    "name": "_MG_3775_obrada",
                    "properties": {
                        "avatar": true,
                        "size": 9285838
                    }
                }
            }
        },
        "receipt": {
            "id_hash": "0x619d5914928361427c30f426f42dbba0601fda7cff6b602ee6d6f32eda527b14",
            "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at": 1677524541301,
            "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0"
        },
        "hub_meta": {
            "analytics_resolved": true,
            "modified_at": 1680693530
        }
    },
    "status": "ok"
}
```
{% endswagger-response %}

{% swagger-response status="404: Not Found" description="Account not found" %}
```json
{
    "status": "error",
    "message": "Account not found"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Search

{% swagger method="post" path="/accounts/search" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="query" type="Object" required="true" %}
Search query
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": [
        {
            "data": {
                "address": "0x6424c4EB1a98c8C1e818961203779620E6b5f0ef",
                "full_name": "Vlad Trifa",
                "short_name": "vlad-trifa_424",
                "email": "vlad@zimt.co",
                "organization_id": "0xe7e5a508302a13cfa5342f46dcdcbb5e0379f1bf467b9e5229870bfac586f4ec",
                "active": true
            },
            "proof": "0x3c388bdc865051dc7faf849f123db22834da1b3ecc5a4a9bba41921250db0fb7349210738a11c82ec95f8178baa99a2806d1f24247caeb0f6c153d7a346f5d541c",
            "id": "0xb63c299875f0dc98b02c9cb1578d1c7ab48a03a929ddc8b5d01cf6ed6f4717cd",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1677524440413
                },
                "signature": "0xab47ec00b8ab442fc8604a23bac96fea3034b63ed08d25be9a8d773d5eabf5df4d70b584330e7abf5c329c61afb947dfc4f44dc7a15d1683900fba74da9ff8cc1b"
            },
            "receipt": {
                "id_hash": "0x6cbb5efc776240c2f4e17deb38ccd9f81f99a9bf2a3e5bb15ea2a3c6de9b095b",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1677524440421
            },
            "hub_meta": {
                "modified_at": 1692895954
            }
        }
    ],
    "status": "ok",
    "pagination": {
        "next": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI0NDQwNDIxfQ==",
        "previous": "eyJyZWNlaXB0LnJlY2VpdmVkX2F0IjoxNjc3NTI0NDQwNDIxfQ==",
        "hasNext": false,
        "size": 1,
        "skip": 0,
        "limit": 11
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
        "accounts":[
            [
                {
                    "field":"data.email",
                    "operator":"equal",
                    "value":"vlad@zimt.co"
                }
            ], 
            []
        ]
    },
   "query_settings":{
      "type":"$and",
      "types": ["$and", "$or"]
   },
   "limit":11,
   "skip":0,
   "sortAscending":false
}
```

</details>

***

## Check availability

{% swagger method="get" path="/accounts/check" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC - ?`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="email" type="String" required="false" %}
Account email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="organization_id" type="String" required="false" %}
Check parameter for specific organization
{% endswagger-parameter %}

{% swagger-parameter in="body" name="organization_name" type="String" required="false" %}
Check organization name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" type="String" required="false" %}
Account unique address
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "status": "ok",
    "response": true
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Email already exists"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get token

{% swagger method="get" path="/accounts/token" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="email" type="String" required="false" %}
Unique account email
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" type="String" required="false" %}
Unique account address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="organization_name" type="String" required="true" %}
Organization that user belong
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "status": "ok",
    "message": "Success",
    "response": { 
        "token": "0kjasndkjn1kjne13kjenkjhqoudhqoudnqoiln...."
    }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Invalid password"
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

## Change / toggle account status

{% swagger method="post" path="/accounts/:id/status" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>

<mark style="color:blue;">

**`ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="path" name="id" type="String" required="true" %}
Unique account id
{% endswagger-parameter %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0xfe552d91787a4b477c10d37755b106c74d274df3b3d5dca70cf1748adda4ff360c03b6157f9b50fc0eee6a7ca9553632bd0c5abfa7f8b1d400fa69225c8451021b",
        "id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4",
        "object": {...},
        "data": {...},
        "receipt": {...},
        "hub_meta": {...}
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

{% swagger-response status="404: Not Found" description="Account not found" %}
```json
{
    "status": "error",
    "message": "Not found"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get me

{% swagger method="get" path="/accounts/me" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" type="String" required="true" %}
JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "response": {
        "proof": "0x3c388bdc865051dc7faf849f123db22834da1b3ecc5a4a9bba41921250db0fb7349210738a11c82ec95f8178baa99a2806d1f24247caeb0f6c153d7a346f5d541c",
        "id": "0xb63c299875f0dc98b02c9cb1578d1c7ab48a03a929ddc8b5d01cf6ed6f4717cd",
        "object": {
            "meta": {
                "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "created_at": 1677524440413
            },
            "signature": "0xab47ec00b8ab442fc8604a23bac96fea3034b63ed08d25be9a8d773d5eabf5df4d70b584330e7abf5c329c61afb947dfc4f44dc7a15d1683900fba74da9ff8cc1b"
        },
        "data": {
            "address": "0x6424c4EB1a98c8C1e818961203779620E6b5f0ef",
            "full_name": "Vlad Trifa",
            "short_name": "vlad-trifa_424",
            "email": "vlad@zimt.co",
            "organization_id": "0xe7e5a508302a13cfa5342f46dcdcbb5e0379f1bf467b9e5229870bfac586f4ec",
            "active": true,
            "owner": true,
            "admin": true,
            "super_admin": true,
            "hub_admin": true
        },
        "receipt": {
            "id_hash": "0x6cbb5efc776240c2f4e17deb38ccd9f81f99a9bf2a3e5bb15ea2a3c6de9b095b",
            "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at": 1677524440421
        },
        "hub_meta": {
            "modified_at": 1692895954
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
{% endswagger %}
