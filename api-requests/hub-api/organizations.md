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

# Organizations

Each org is a unique `account` in a Hub. All data, users, etc. is fully isolated between orgs and there’s no cross-org sharing or visibility whatsoever!

No user in an org can see/interact with anything in another org - **EVER**!

Orgs are usually linked to one of our “projects” (e.g. Emerald is one org, Siliconaire, is another org, DoubleCream, Wine, etc.) and we should use accounts/teams within that org for access control around the data.

_**Root**_ Pub & private key of the org cannot be changed (they should be super private!), as they will only be used as “_**ultimate backup**_” (e.g. delete all admins, etc.)

When organization is created, we also create a first “admin” user for that org, and all interactions within an org should be done via the admin users

We’ll have one ZIMT organization, the “admins” of this organization are the only ones to who have more visibility & control over other organizations (ZIMT accounts in the “admin” team)

## Check username availability

{% swagger method="get" path="/organizations/check?short_name=" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="query" name="short_name" required="true" %}
Organization username
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Available" %}
```json
{
    "exists": true
}
```
{% endswagger-response %}

{% swagger-response status="200: OK" description="Already exists" %}
```json
{
    "exists": false
}
```
{% endswagger-response %}
{% endswagger %}

***

## Signup request

{% swagger baseUrl="https://api.zi.mt" method="post" path="/organizations/signup" summary="Create signup request." %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="full_name" required="true" type="string" %}
User first name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="email" required="true" type="string" %}
Unique email address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="address" required="true" type="string" %}
Account address
{% endswagger-parameter %}

{% swagger-parameter in="body" name="organization_name" required="true" type="string" %}
Unique organization name
{% endswagger-parameter %}

{% swagger-parameter in="body" name="security_token" required="true" %}
Encrypted password with user private key
{% endswagger-parameter %}

{% swagger-response status="200" description="Organization request created" %}
```json
{
    "response": {
        "proof": "0x3dc2e1431272ba7c4b5e8f1bbf0c3478dd1674c2282ad3c4459339f536bbce2c5d0ee1856a69b126ce219d6eb4f0faea498dfd643e136314b87327c07adb4a0b1c",
        "id": "0x0f46494e9547e78994dc8358ae5df14462850c2f3163b05e2556181e18315bad",
        "receipt": {
            "id_hash": "0x78806b4e205b4ee40c1a6994e8b79ab774679f2f8544f37e0b96f57a31346995",
            "received_by": "0x1f92D0085C9fD2942327Cdd02Ee4392FEC41baBC",
            "received_at": 1678222888289
        }
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
    "message": "Validation error",
    "errors": {
        "email": "Invalid email",
        "full_name": "Must contain minimum 3 characters",
        "organization_name": "Field is required",
        "admin_address": "Address not valid",
        "security_token": "Field is required",
        "signature": "Invalid signature",
        "super_admin_address": "Address not valid"
    }
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Organization already exists" %}
```json
{
    "status": "error",
    "message": "Organization already exists."
}
```
{% endswagger-response %}
{% endswagger %}

***

## Signup confirmation (disabled)

{% swagger method="get" path="/organizations/signup/:id/confirm" baseUrl="https://api.zi.mt" summary="Confirm registration via email address" %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="query" name="token" required="true" %}
Confirmation token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Succes" %}
```json
{
    "status": "ok",
    "message": "Confirmed" // check what is the exact message
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Invalid confirm token"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Expired token" %}
```json
{
    "status": "error",
    "message": "Sign up confirm token expired"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Resend confirmation (disabled)

{% swagger method="get" path="/organizations/signup/:id/confirm/resend" baseUrl="https://api.zi.mt" summary="Resend confirmation email" %}
{% swagger-description %}
<mark style="color:green;">

**`PUBLIC`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="query" name="token" required="true" %}
JWT Confirmation token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "status": "ok",
    "message": "success"
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Invalid token" %}
```json
{
    "status": "error",
    "message": "Invalid token"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get organization requests

{% swagger method="get" path="/organizations/requests" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZEDHUB_ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="query" name="page_limit" required="false" %}
Items per page
{% endswagger-parameter %}

{% swagger-parameter in="query" name="name" required="false" %}
Search by organization name
{% endswagger-parameter %}

{% swagger-parameter in="query" name="email" required="false" %}
Search by email
{% endswagger-parameter %}

{% swagger-parameter in="query" name="active" required="false" %}
Search by status
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "response": [
        {
            "data": {
                "active": true,
                "resolved": true,
                "account_id": "0x1eb92f7d2a9982128a05f010028c4e1e5b43d77f1be80b32f662533dff4f6081",
                "organization_id": "0xd1b65f2fe357c05e7acfc3cc3721dfcdbe8822606fa5556ce0262c24cf81b180",
                "plan": {
                    "id": "0xdb6185b4ac2fa1decc263ef83d05b629c2d1b588dbbaf0052f0047513af1bc2e",
                    "data": {
                        "active": {
                            "plan": {
                                "id": "0x51e11eb7209851bc0fd13b8dd45100d649357108d435257903292a4889f435ee",
                                "name": "Enterprise plan"
                            },
                            "updated_at": 1691654755901,
                            "period_start": 1691654755901,
                            "period_end": 1694246755901,
                            "period_end_grace": 1692259555901
                        },
                        "organization": {
                            "id": "0xd1b65f2fe357c05e7acfc3cc3721dfcdbe8822606fa5556ce0262c24cf81b180",
                            "name": "swiss-wine"
                        }
                    }
                },
                "plans": [
                    {
                        "id": "0x023c042d529710c35a0b06cb7a44914dfef90090dd2675832d19828bafe280e1",
                        "data": {
                            "name": "Free plan",
                            "short_name": "free-plan",
                            "description": "Free plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {}
                                    ]
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "events": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 100
                                },
                                "documents": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "collections": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "accounts": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "teams": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "settings": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "apiKeys": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "strategies": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": true,
                            "private": false
                        }
                    },
                    {
                        "id": "0x65e1309b67b4a1ae85f6496290e71f388c34cbbaa96c9032a7bd58c91df6c9bd",
                        "data": {
                            "name": "Starter plan",
                            "short_name": "starter-plan",
                            "description": "Starter plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {}
                                    ]
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 1000
                                },
                                "events": {
                                    "day": 1000,
                                    "month": 1000
                                },
                                "documents": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 1000
                                },
                                "collections": {
                                    "day": 2,
                                    "month": 2,
                                    "total": 2
                                },
                                "accounts": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "teams": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "settings": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "apiKeys": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "strategies": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": false,
                            "private": false
                        }
                    },
                    {
                        "id": "0x51e11eb7209851bc0fd13b8dd45100d649357108d435257903292a4889f435ee",
                        "data": {
                            "name": "Enterprise plan",
                            "short_name": "enterprise-plan",
                            "description": "Enterprise plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {
                                            "id": "0x1253c1060be72230630ef65be001a574b80a9ba63c00435a3821a9b36c4f6403",
                                            "name": "Viewer"
                                        }
                                    ]
                                },
                                "strategies": {
                                    "all": true,
                                    "create": true,
                                    "dedicated": true
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 10000
                                },
                                "events": {
                                    "day": 100000
                                },
                                "documents": {
                                    "day": 10000
                                },
                                "collections": {
                                    "day": 10,
                                    "total": 10
                                },
                                "accounts": {
                                    "day": 100,
                                    "total": 10000
                                },
                                "teams": {
                                    "day": 100,
                                    "total": 10000
                                },
                                "apiKeys": {
                                    "day": 10,
                                    "total": 10
                                },
                                "strategies": {
                                    "day": 5,
                                    "total": 5
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": false,
                            "private": false
                        }
                    }
                ]
            },
            "id": "0xa5d2b58e7d3de5a46c8430fd2bfff26e93457fb6bcfcfc1da6336fa7ef2e9157",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1691654306780,
                    "data_hash": "0x0c342025ad1b8bbe8c9da133c8883ef13efba993d3a156e380b2e933b8531e4f"
                },
                "data": {
                    "full_name": "Swiss wine",
                    "email": "swisswine@mailinator.com",
                    "address": "0xde7969d09936EE1BD7e26070842E67f7bF598C40",
                    "organization_name": "swiss-wine",
                    "security_token": "U2FsdGVkX1+cY4JlYmtAC71LEJCFJdfoarW6Ps7DeUI6IxHv1h95608mslrl8nfRpIZr//LzPbbRnhJm6CNxjS0+Wbdyz+EeaqIGbgwDjkt7zRt6aOuV/oEuzhv6vrQO45HqXOETmuavza/E9Vm1MW8f3LdXxdMvmKzmGTOkxVHT7BqyZcw5qaI9uzm61iweJgVw7rtSPd6MWj/q3m/44g=="
                },
                "signature": "0xb5751a6dcf1537aaf458e12297b6008c3acda2f36572e8932618d62d184ed47a2f5fdc55b10a4e8d02f126e6a0c6e52fc8744e96cf7a576eaf41e7d5e95686a91b"
            },
            "proof": "0x19dec521290bef8ecf25ff924026ce0dcabb963ec4d124ac7812dddd3b3d952e73ff0d88a3904e6125e2ef8476c5d54617e2abe8e05be91a0aae3a2e0f3e09511c",
            "receipt": {
                "id_hash": "0x6ca9406e86888787b9a82bfb7d0418c4af470add50213e060d508ccf69fe8847",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1691654306782
            }
        },
        {
            "data": {
                "active": true,
                "resolved": true,
                "account_id": "0x7f2cb93cc17006afe4b5f41f3c42c002bb099db005c5ce38d647ccf8db6e1aa9",
                "organization_id": "0xd8c88f906252635db67285f43df70510015c541b6caf358521fda3c879b9db66",
                "plan": {
                    "id": "0xfcea1f075ffac2a7047c93616626d859c7dbaedc43e8644c866ab3c19614a6bd",
                    "data": {
                        "active": {
                            "plan": {
                                "id": "0x51e11eb7209851bc0fd13b8dd45100d649357108d435257903292a4889f435ee",
                                "name": "Enterprise plan"
                            },
                            "updated_at": 1687336957381,
                            "period_start": 1687336957381,
                            "period_end": 1689928957381,
                            "period_end_grace": 1687941757381
                        },
                        "organization": {
                            "id": "0xd8c88f906252635db67285f43df70510015c541b6caf358521fda3c879b9db66",
                            "name": "diligence"
                        }
                    }
                },
                "plans": [
                    {
                        "id": "0x023c042d529710c35a0b06cb7a44914dfef90090dd2675832d19828bafe280e1",
                        "data": {
                            "name": "Free plan",
                            "short_name": "free-plan",
                            "description": "Free plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {}
                                    ]
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "events": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 100
                                },
                                "documents": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "collections": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "accounts": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "teams": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "settings": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "apiKeys": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "strategies": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": true,
                            "private": false
                        }
                    },
                    {
                        "id": "0x65e1309b67b4a1ae85f6496290e71f388c34cbbaa96c9032a7bd58c91df6c9bd",
                        "data": {
                            "name": "Starter plan",
                            "short_name": "starter-plan",
                            "description": "Starter plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {}
                                    ]
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 1000
                                },
                                "events": {
                                    "day": 1000,
                                    "month": 1000
                                },
                                "documents": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 1000
                                },
                                "collections": {
                                    "day": 2,
                                    "month": 2,
                                    "total": 2
                                },
                                "accounts": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "teams": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "settings": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "apiKeys": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "strategies": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": false,
                            "private": false
                        }
                    },
                    {
                        "id": "0x51e11eb7209851bc0fd13b8dd45100d649357108d435257903292a4889f435ee",
                        "data": {
                            "name": "Enterprise plan",
                            "short_name": "enterprise-plan",
                            "description": "Enterprise plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {
                                            "id": "0x1253c1060be72230630ef65be001a574b80a9ba63c00435a3821a9b36c4f6403",
                                            "name": "Viewer"
                                        }
                                    ]
                                },
                                "strategies": {
                                    "all": true,
                                    "create": true,
                                    "dedicated": true
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 10000
                                },
                                "events": {
                                    "day": 100000
                                },
                                "documents": {
                                    "day": 10000
                                },
                                "collections": {
                                    "day": 10,
                                    "total": 10
                                },
                                "accounts": {
                                    "day": 100,
                                    "total": 10000
                                },
                                "teams": {
                                    "day": 100,
                                    "total": 10000
                                },
                                "apiKeys": {
                                    "day": 10,
                                    "total": 10
                                },
                                "strategies": {
                                    "day": 5,
                                    "total": 5
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": false,
                            "private": false
                        }
                    }
                ]
            },
            "id": "0x73ee4720648059cc1641adb5d3e5c4ba6a9b261f634979a4eeb28706a9ad932d",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1686574811636,
                    "data_hash": "0x9ec76b03e0f5c0e61f27fc6d25572a8da8ac591e1139d2a5085d5da1559040a3"
                },
                "data": {
                    "full_name": "The Diligence",
                    "email": "milos@zimt.co",
                    "address": "0xfa17fc61Da37fc5EE6cbFb0EB93a0714a1972475",
                    "organization_name": "diligence",
                    "security_token": "U2FsdGVkX1+F3DnEpUf1ivCQtVK3jCITbPtuogi7Ekg9EIeB2g/iItaHTJBWwKsL8g/cq3O11lDuZmaq/8Zd1jWYsJousF3AvAuWGADwawwH2muhyohYB62bfQHIrmOCPkk1go1TzzQ0GagJtAxbrDNMRCKqqaLfXlqwQb0gB7/nnbO0o7qe/0IdbbSwqWBzSEO1/I6vhl3gFFldqc/Mkg=="
                },
                "signature": "0x43968482e6e1b91a98ee3a00f9623ee4359e4e9ce2032a2b93e8c8796f03d41c2562c149dc6309f3599827592b5a7420e0d06f990a55f14a4a6a2315ef8d435b1c"
            },
            "proof": "0x0b401753b84e74cfb1e3b156cb298cfdc5235affe74e6ad039d44e18e9c669ba322ac8b84f19a9004dcff2b95b963c0fd23f692a7bdd937d62cb9b1f52f84fb21b",
            "receipt": {
                "id_hash": "0xd937ccfa9ed686fe9997fb8735ccf116275dc53ae8e983a9cb600552de5575b0",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1686574811649
            }
        },
        {
            "data": {
                "active": true,
                "resolved": true,
                "account_id": "0x1f1de84adeda504d3b01df023907a99617900e94cff9a01b9d869e5e5a6c3cd4",
                "organization_id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                "plan": {
                    "id": "0xede28db1374038623815c27c0140b723bd2a9d90b57e49d501218804a2903036",
                    "data": {
                        "active": {
                            "plan": {
                                "id": "0x51e11eb7209851bc0fd13b8dd45100d649357108d435257903292a4889f435ee",
                                "name": "Enterprise plan"
                            },
                            "updated_at": 1677524546259,
                            "period_start": 1677524546259,
                            "period_end": 1680116546259,
                            "period_end_grace": 1678129346259
                        },
                        "organization": {
                            "id": "0xf005b32df693af8e83d09e1e58d4f902a1aaa80203eb38c099c0849852359aa0",
                            "name": "doublecream"
                        }
                    }
                },
                "plans": [
                    {
                        "id": "0x023c042d529710c35a0b06cb7a44914dfef90090dd2675832d19828bafe280e1",
                        "data": {
                            "name": "Free plan",
                            "short_name": "free-plan",
                            "description": "Free plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {}
                                    ]
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "events": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 100
                                },
                                "documents": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "collections": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "accounts": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "teams": {
                                    "day": 1,
                                    "month": 1,
                                    "total": 1
                                },
                                "settings": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "apiKeys": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "strategies": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": true,
                            "private": false
                        }
                    },
                    {
                        "id": "0x65e1309b67b4a1ae85f6496290e71f388c34cbbaa96c9032a7bd58c91df6c9bd",
                        "data": {
                            "name": "Starter plan",
                            "short_name": "starter-plan",
                            "description": "Starter plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {}
                                    ]
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 1000
                                },
                                "events": {
                                    "day": 1000,
                                    "month": 1000
                                },
                                "documents": {
                                    "day": 100,
                                    "month": 100,
                                    "total": 1000
                                },
                                "collections": {
                                    "day": 2,
                                    "month": 2,
                                    "total": 2
                                },
                                "accounts": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "teams": {
                                    "day": 10,
                                    "month": 10,
                                    "total": 10
                                },
                                "settings": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "apiKeys": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                },
                                "strategies": {
                                    "day": 0,
                                    "month": 0,
                                    "total": 0
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": false,
                            "private": false
                        }
                    },
                    {
                        "id": "0x51e11eb7209851bc0fd13b8dd45100d649357108d435257903292a4889f435ee",
                        "data": {
                            "name": "Enterprise plan",
                            "short_name": "enterprise-plan",
                            "description": "Enterprise plan",
                            "features": {
                                "api_keys": {
                                    "allowed": [
                                        {
                                            "id": "0x51b45e504dd051ded20b6e3ffe58ccf32faf49d5f37682d2db6816dbb6447962",
                                            "name": "Dashboard"
                                        },
                                        {
                                            "id": "0x1253c1060be72230630ef65be001a574b80a9ba63c00435a3821a9b36c4f6403",
                                            "name": "Viewer"
                                        }
                                    ]
                                },
                                "strategies": {
                                    "all": true,
                                    "create": true,
                                    "dedicated": true
                                }
                            },
                            "limits": {
                                "assets": {
                                    "day": 10000
                                },
                                "events": {
                                    "day": 100000
                                },
                                "documents": {
                                    "day": 10000
                                },
                                "collections": {
                                    "day": 10,
                                    "total": 10
                                },
                                "accounts": {
                                    "day": 100,
                                    "total": 10000
                                },
                                "teams": {
                                    "day": 100,
                                    "total": 10000
                                },
                                "apiKeys": {
                                    "day": 10,
                                    "total": 10
                                },
                                "strategies": {
                                    "day": 5,
                                    "total": 5
                                }
                            },
                            "alerts": {
                                "assets": {
                                    "day": true,
                                    "total": true
                                },
                                "events": {
                                    "day": true,
                                    "total": true
                                },
                                "documents": {
                                    "day": true,
                                    "total": true
                                },
                                "collections": {
                                    "day": true,
                                    "total": true
                                }
                            },
                            "active": true,
                            "default": false,
                            "private": false
                        }
                    }
                ]
            },
            "id": "0x1849ad83eceecae73f9abadcc17b0a7c012e25552627143b32ce798f1de82af9",
            "object": {
                "meta": {
                    "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                    "created_at": 1677524531898,
                    "data_hash": "0x0ee53bb0b9fb60cc4e0a835f3aa64d24176a91e1844ec41576fea4917b1960ed"
                },
                "data": {
                    "full_name": "Double Cream",
                    "email": "dc@mailinator.com",
                    "address": "0x99985DD90635C4a765C4bCD340Bb0115A72B20F1",
                    "organization_name": "doublecream",
                    "security_token": "U2FsdGVkX199ofPp/evrWnS28vp9sVVjoYsovv0t1PBVTf6u07OJXgtUIeH2ClJDL0gvxb2F58fOXN6kTG1Hf7Lz5nYpf0LUdMaaGXNB862DLOK5WsgZKuEc1BBuN6jk0bcesViWVpnslIrMxQ8b2P8jCJfIFHTZ42YXEX+raV+QuEw143YVhRb4dO23nlX52kwr1qyxrYs9bn/PGLY8EQ=="
                },
                "signature": "0xec279c487ca6e7ca7d4a51ad28fe73e37a293bf8fa61cbbf08c80f0b35b1d9242e00e90434b2fedb53029d8d43223d30ea0489bd658d2a14091f1cfba361b7fa1b"
            },
            "proof": "0xf347f77648622e4b7673e75b6ad78766005bbe950ec35f8559e679f74a61249e27ee17ef721fcd1e1f20d822945abe946aea250aa682243f6663f797a8e85f9f1b",
            "receipt": {
                "id_hash": "0xe38a5d54fe94a6888fe53b03bec04f73fc18e6ddd60eda053edd1bc4f75020dd",
                "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "received_at": 1677524531902
            }
        }
    ],
    "status": "ok"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Approve / Suspend organization

{% swagger method="put" path="/organizations/requests/:id/status" baseUrl="https://api.zi.mt" summary="Change organization status (approve / suspend)" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZEDHUB_ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="query" name="status" required="true" %}
Organization status (approve / suspend)
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
    "status": "ok",
    "message": "Confirmed" 
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="" %}
```json
{
    "status": "ok",
    "message": "Confirmed"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Get organization details

{% swagger method="get" path="/organizations/login" baseUrl="https://api.zi.mt" summary="/organizations/me" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZED`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="header" name="Authorization" required="true" %}
Authorization JWT token
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```json
{
   "response":{
      "organization":{
         "data":{
            "owner":{
               "id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90",
               "data":{
                  "address":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
                  "full_name":"Vlad Trifa",
                  "email":"vlad@zimt.co",
                  "security_token":"U2FsdGVkX1/LIr8AXhI85G56Pz27SYwNwJ+5z4Ao/7PS8J25U5OVNLJTtqf2RT4QAnnPmN/QGkazw711jPdqeEx/cm0LWlcYOuY1/qx/bIHJZwxGRumA9EOCCoUYsFKXHVS3nfQoEsDZClODMTFRaT7+CYadpfx/upaQ9yp+qk2pBuDAiw9YpPFTF3bFzEZpAjw9sEKpdEYVIY47ZP33AA==",
                  "organization_id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
                  "active":true,
                  "settings":{
                     "about":{
                        "biography":"testasdasdasdas",
                        "city":"asd",
                        "country":"Afghanistan",
                        "state":"asda",
                        "zip":"asd"
                     }
                  }
               }
            },
            "name":"ZIMT",
            "short_name":"zimt",
            "plan":{
               "id":"0xb2b6b251cb565eb629280fda044484bfa3ccfccdd6454b2c0d89f93a7e5b6e26",
               "data":{
                  "name":"Enterprise plan",
                  "short_name":"enterprise-plan",
                  "description":"Enterprise plan",
                  "features":{
                     "api_keys":{
                        "allowed":[
                           {
                              "id":"0x26e71474b55712b8b80505851bb50f73488de5f5e14c010a6c63610c0c98f7a4",
                              "name":"Dashboard"
                           },
                           {
                              "id":"0xfde85c7115929907a7ff530b1c07e009ab9ea41dec988907fd70ade46b3dc6b0",
                              "name":"Viewer"
                           }
                        ]
                     },
                     "strategies":{
                        "all":true,
                        "create":true,
                        "dedicated":true
                     }
                  },
                  "limits":{
                     "assets":{
                        "day":10000
                     },
                     "events":{
                        "day":100000
                     },
                     "documents":{
                        "day":10000
                     },
                     "collections":{
                        "day":10,
                        "total":10
                     },
                     "accounts":{
                        "day":100,
                        "total":10000
                     },
                     
                  },
                  "alerts":{
                     "assets":{
                        "day":true,
                        "total":true
                     },
                     "events":{
                        "day":true,
                        "total":true
                     },
                     "documents":{
                        "day":true,
                        "total":true
                     },
                     "collections":{
                        "day":true,
                        "total":true
                     }
                  },
                  "active":true,
                  "default":false,
                  "private":false
               }
            },
            "organizationPlan":{
               "id":"0xd8a9fdde277ef3925db57450da5fac80dbb4f7b116d750f02a597ff57a532e6e",
               "data":{
                  "active":{
                     "plan":{
                        "id":"0xb2b6b251cb565eb629280fda044484bfa3ccfccdd6454b2c0d89f93a7e5b6e26",
                        "name":"Enterprise plan"
                     },
                     "updated_at":1669933039845,
                     "period_start":1669933039845,
                     "period_end":1672525039,
                     "period_end_grace":1670537839
                  },
                  "organization":{
                     "id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
                     "name":"ZIMT"
                  }
               }
            },
            "active":true
         },
         "id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
         "object":{
            "meta":{
               "created_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
               "created_at":1669933039787
            },
            "signature":"0x675d13cf0b4b23785b9a67829d7d1ee49310611e1bd2a215963f6d0d45f7f3e766892b69d866ff4c588d3df867bf712a3a2187a01eddbb40a9859d3858f1c6d91c"
         },
         "proof":"0xdf4be3e622f1cd481f23814dace791dfad228999ad1c8979199d5e53c119b10a6af8a2e1d0d153534422f1e61749a9f372c767465d5a9435269a6f18110128cb1b",
         "receipt":{
            "id_hash":"0x5fc2525b77fb1630931b447aa22eca921d6be2f8336f52d8b0fb71c35092f602",
            "received_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
            "received_at":1669933039790
         }
      },
      "account":{
         "proof":"0x135088750faf7adf4b2968e2905e5321791e2b4bec78883f53c833d1e6a05fb24d39a9d12c63a0344cf47c833fe01f899cea7908341aa8916d394002448483fb1b",
         "id":"0xe1e2b93decff0d8bd259d0f5566417faaef985f9c14b52dcc670d87fce42ce90",
         "object":{
            "meta":{
               "created_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
               "created_at":1669933039983
            },
            "signature":"0xad1e4478b7f8d529f1fa73b9b17a3a1ba811b7b5dab4561dd1056d4a1d369ef3613bfe6ef2765912c985a81765ca64e2288b6f2c39322e118117b061fb762fe71b"
         },
         "data":{
            "address":"0xf0f505E4E41e1897942b3baDFfEBE88De17bE0a2",
            "full_name":"Vlad Trifa",
            "email":"vlad@zimt.co",
            "organization_id":"0x67e2429ab5b029a6b91f3358150eb75a3fbd51fabc506bbb61ffbd7df4bc42cb",
            "active":true,
            "settings":{
               "about":{
                  "biography":"testasdasdasdas",
                  "city":"asd",
                  "country":"Afghanistan",
                  "state":"asda",
                  "zip":"asd"
               }
            },
            "owner":true,
            "admin":true,
            "super_admin":true,
            "hub_admin":true
         },
         "receipt":{
            "id_hash":"0x89857f9d51b72d3d274cfc6e24161eccd72e3da6ce9bfe4c1d0a6661217d1cf6",
            "received_by":"0x8fd379246834eac74B8419FfdA202CF8051F7A03",
            "received_at":1669933039995
         },
         "hub_meta":{
            "modified_at":1669983853
         }
      }
   },
   "status":"ok"
}
```
{% endswagger-response %}

{% swagger-response status="401: Unauthorized" description="" %}
```json
{
    "status": "error", 
    "message": "Unauthorized"
}
```
{% endswagger-response %}
{% endswagger %}

***

## Update organization settings

{% swagger method="put" path="/organizations/settings" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZEDHUB_ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="sections" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="languages" required="false" %}

{% endswagger-parameter %}

{% swagger-parameter in="body" name="theme" required="false" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Succes" %}
```json
{
    "response": {
        "proof": "0x7a4bf4acbc348438ff0e2533630ba5b9ccd0bfaf04ae98896c232d0a9b0276724fc30f0e5a7754a75ee63be1fba8e762ffa6d0d5d3f2f406f5738e9a66b335de1b",
        "id": "0xe7e5a508302a13cfa5342f46dcdcbb5e0379f1bf467b9e5229870bfac586f4ec",
        "object": {
            "meta": {
                "created_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
                "created_at": 1677524440236
            },
            "signature": "0xdabb51d8b41f8fd7444c40888ee0b265fdf32b4df0231c05b0db0303635e7bef6d49297e24e975e7d71c50f7324cf8df17ecd3ad30a250b300e8be1cccc88d6f1c"
        },
        "data": {
            "name": "ZIMT",
            "allow_public_signup": false,
            "short_name": "zimt",
            "active": true,
            "owner_id": "0xb63c299875f0dc98b02c9cb1578d1c7ab48a03a929ddc8b5d01cf6ed6f4717cd",
            "settings": {
                "sections": {
                    "product": {
                        "buy": {
                            "active": false,
                            "button": true,
                            "favorite": false,
                            "items": [],
                            "position": 5,
                            "report": false,
                            "share": false
                        },
                        "footer": {
                            "active": true,
                            "name": "footer"
                        },
                        "ingredients": {
                            "active": true,
                            "open": false
                        },
                        "labels": {
                            "active": true,
                            "button": true,
                            "items": [],
                            "position": 5
                        },
                        "nutrition": {
                            "active": true,
                            "open": false,
                            "position": 2,
                            "type": "accordion"
                        },
                        "properties": {
                            "active": true,
                            "fields": [
                                "name",
                                "description",
                                "type"
                            ],
                            "name": "properties",
                            "open": true
                        },
                        "producer": {
                            "active": true,
                            "button": true
                        }
                    }
                },
                "languages": {
                    "en": {
                        "assets": {
                            "load_more": "Load more",
                            "title": "Assets"
                        },
                        "buy": {
                            "description": "You will find this item at the following retailers.",
                            "title": "Where to buy"
                        },
                        "content": {
                            "title": "About"
                        },
                        "details": {
                            "button": "Buy now",
                            "created_at": "Since",
                            "created_by": "Produced by",
                            "favorite": "Favorite",
                            "report": "Report",
                            "share": "Share page"
                        },
                        "footer": {
                            "powered": "Powered by"
                        },
                        "hero": {
                            "category": "Winery",
                            "date": "Since",
                            "label": "Discover the producer"
                        },
                        "identifiers": {
                            "code": "Some code",
                            "id": "Ref numbers",
                            "title": "Identifiers"
                        },
                        "ingredience": {
                            "description": "",
                            "title": "Ingredience"
                        },
                        "labels": {
                            "description": "",
                            "load_more": "Load more",
                            "title": "Certificates"
                        },
                        "location": {
                            "address": "Address",
                            "description": "",
                            "email": "Email",
                            "location": "Where to find us",
                            "phone": "Phone",
                            "social": {
                                "description": "Feel free to follow us on social media.",
                                "title": "Social"
                            },
                            "title": "Contact",
                            "website": "Website"
                        },
                        "nutrition": {
                            "calcium": "Calcium",
                            "carbohydrate": "Carbon hydrate",
                            "description": "",
                            "dietary-fibre": "Fibre",
                            "energy": "Energy",
                            "fat": "Fat",
                            "iron": "Iron",
                            "magnesium": "Magnesium",
                            "manganese": "Manganese",
                            "potassium": "Potassium",
                            "protein": "Protein",
                            "saturated-fat": "Saturated fat",
                            "serving-per-container": "Serving (container)",
                            "serving-size-g": "Serving (g)",
                            "sodium": "Sodium",
                            "sugars": "Sugar",
                            "title": "Nutritional",
                            "vitamin-a": "Vitamin (a)",
                            "vitamin-b2": "Vitamin (b2)",
                            "vitamin-b6": "Vitamin (b6)",
                            "vitamin-b9": "Vitamin (b9)",
                            "vitamin-c": "Vitamin (c)",
                            "vitamin-e": "Vitamin (e)",
                            "vitamin-k": "Vitamin (k)"
                        },
                        "partners": {
                            "title": "Our Partners"
                        },
                        "properties": {
                            "aoc": "Aoc",
                            "bottler": "Bottler",
                            "category": "Category",
                            "color": "Color",
                            "country": "Country",
                            "grapes": "Grapes",
                            "method": "Method",
                            "nutrition": "Nutrition facts",
                            "region": "Region",
                            "size": "Size",
                            "sulfites": "Sulfites",
                            "temperature": "Drinking temperature",
                            "title": "Properties",
                            "type": "Wine type",
                            "vinification": "Vinification",
                            "vintage": "Vintage",
                            "volume": "Alcohol volumes"
                        },
                        "sidebar": {
                            "home": "Home",
                            "languages": {
                                "de": "German",
                                "en": "English",
                                "ru": "Russian",
                                "title": "Language"
                            },
                            "pages": {
                                "about": "About",
                                "contact": "Contact",
                                "privacy": "Privacy policy",
                                "terms": "Terms and conditions"
                            }
                        },
                        "slogan": {
                            "subtitle": "Contact us to claim it.",
                            "title": "Is this your product profile?"
                        }
                    }
                }
            }
        },
        "receipt": {
            "id_hash": "0x6567799de77b80d50d3d31196b131a966a36b251c8dac9440b1cc2735b98cc31",
            "received_by": "0x61e0Bef3613BE74331F131cfA9D0be29c456A370",
            "received_at": 1677524440241
        },
        "hub_meta": {
            "modified_at": 1694455522
        }
    },
    "status": "ok"
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
{% endswagger %}

***

## Change organization plan

{% swagger method="put" path="/organizations/:id/plan" baseUrl="https://api.zi.mt" summary="" %}
{% swagger-description %}
<mark style="color:red;">

**`AUTHORIZEDHUB_ADMIN`**

</mark>
{% endswagger-description %}

{% swagger-parameter in="body" name="plan_id" required="true" %}
Unique plan id
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="Success" %}
```json
{
    "status": "ok",
    "message": "Success" 
}
```
{% endswagger-response %}

{% swagger-response status="400: Bad Request" description="Validation error" %}
```json
{
    "status": "error",
    "message": "Invalid plan id" 
}
```
{% endswagger-response %}
{% endswagger %}

##
