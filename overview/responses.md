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

# 📬 Responses

***

## Response status codes

Each API request returns status message and code.

### Successful requests

For successful requests - <mark style="color:green;">`2XX`</mark> status codes.

| Status Code                                      | Description                                                                                   |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------- |
| <mark style="color:green;">200</mark> OK         | The request succeeded.                                                                        |
| <mark style="color:green;">201</mark> Created    | The request succeeded, and a new resource was created as a result. (used after POST requests) |
| <mark style="color:green;">204</mark> No Content | The server successfully executed the method but returns no response body.                     |

###

### **Failed** requests

For failed requests - <mark style="color:orange;">`4XX`</mark> status codes.

| Status code                                                   | Description                                                                                               |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| <mark style="color:orange;">400</mark> Bad request            | The server could not understand the request. Validation, Payload format                                   |
| <mark style="color:orange;">401</mark> Unauthorised           | The client must authenticate itself to get the requested response                                         |
| <mark style="color:orange;">403</mark> Permission denied      | Client is authorised but without permissions to request                                                   |
| <mark style="color:orange;">404</mark> Not found              | The server can not find the requested resource                                                            |
|                                                               |                                                                                                           |
| <mark style="color:yellow;">422</mark> Unprocurable Entity    | Server understands the content type, syntax is correct of the request, but its unable to complete request |
| <mark style="color:yellow;">415</mark> Unsupported Media Type | Payload format is in an unsupported format                                                                |
|                                                               |                                                                                                           |

`5XX` status code will appear when something is wrong with a server or service.

| Status code                                               | Description                                        |
| --------------------------------------------------------- | -------------------------------------------------- |
| <mark style="color:red;">500</mark> Internal Server Error | An internal server error has occurred              |
| <mark style="color:red;">503</mark> Service Unavailable   | The server cannot handle the request for a service |
| <mark style="color:red;">501</mark> Not implemented       | Server does not support request                    |

* `AUTHENTICATION`
* `AUTHORIZATION`
* `INTERNAL`
* `UNSUPPORTED_CLIENT`
* `NOT_FOUND`
* `NOT_IMPLEMENTED`
* `RESOURCE_LIMIT`
* `SERVICE_AVAILABILITY`
* `VALIDATION`

**Response structure**

```jsx
{
	"status": "<string> | <boolean>",
	"message": "<string>",
	"response": "<object> | <array>",
        "pagination": {
                "size": 30,
                "skip": 0,
                "limit": 30
            }
	"errors": [{field: "<string>", message: "<string>"}]
}
```

**`INPUT_VALIDATION_ERROR`**

**`INTERNAL_SERVER_ERROR`**

**`NOT_AUTHORIZED`**

**`RESOURCE_NOT_FOUND`**
