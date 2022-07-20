---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http: REST
  - javascript--react: React
  - javascript--browser: Javascript

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Platform-X API documentiation

# Authentication

All authentication except direct username/password auth is initiated by calling the `/authorize` or `/authorise` endpoints. If you wish to implement your own authentication and authorisation interface, this section is for you.

## Authorization Code Flow with PKCE
This flow is **Highly Recommended** for mobile or SPA's.

```http
GET /authorize?client_id=my-project&response_type=code&redirect_url=https://www.my-app.com&code_challenge=xyz&state=abc HTTP/1.1
```

```http
HTTP/1.1 302 Found
Location: https://my-project.grocket.dev?client_id=my-project&response_type=code&redirect_url=https://www.my-app.com&code_challenge=xyz&state=abc
```

```javascript--react
import { useAuth } from '@platformx/auth';

const MyComponent = (props) => {
    const { signinWithRedirect } = useAuth();

    signinWithRedirect();
}
```

```javascript--browser
const px = require('@platformx/auth');

px.signinWithRedirect();
```


### Query Parameters

Parameter | Default | Description
--------- | ------- | ---------
client_id |  | **Required**. The unique identifier for your accounts project
response_type |  | **Required**. Suported options are `code` or `token`
redirect_uri |  | **Required**. A registered URL that the authorisation server will redirect to after successful authorisation.
code_challenge |  | A challenge that has been generated from the `code_verifier`. **Required** when using the `code` option for the `response_type` parameter. 
code_challenge_method | S256 | At present, S256 is the only supported method.
scope | | A space-delimited, case-sensitive list of requested scopes. Although this list represents scopes being requested, the response may contain a subset of `scopes` that was granted by the client.
audience | | The unique identifier of the API that the client is requesting acceess to.
state | | **Highly Recommended** An opaque value used to maintain state between the request and response. The parameter is highly recommended for preventing cross-site request forgery. SDK's will automatically generate this if no value is provided.



<aside class="success">
Where possible, use the recommended options to keep your users secure and safe.
</aside>




### HTTP Request

# Kittens

## Get All Kittens

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> Response

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten


```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

