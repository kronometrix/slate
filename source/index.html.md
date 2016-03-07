---
title: API Reference

language_tabs:
  - http 
  - cUrl
  - PHP
  - Java

toc_footers:
  - <a href='http://kronometrix.io'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

This is the place where you will find details about the accessible API provided by Kronometrix for its users.

The Kronometrix API can be accessed over HTTP, using standard POST requests.

The Kronometrix API can be structured in 2 main sections: the **Public API** and the **Private API**.

## Public API

The **Public API** is being conceived to allow users to interact with the data in Kronometrix, by offering a way to list subscriptions and datasources an user has access to, to determine the data structures available and to query for statistical data in the application.

## Private API

The **Private API** is aimed towards *Kronometrix Agents*, allowing them to insert data into Kronometrix. All recorders use the **Private API** to insert the recorded data into the datastructures stored with Kronometrix. This part of the API is also public, but has been called "private" because it is used internally by the two parts of the Kronometrix system (recorders and server) and is, generally, not used by the users themselves.

# Accessing the API

To access the API (either public or private) you will need an *API Token*. The API Token can be obtained by logging in Kronometrix and accessing the section *Settings -> API Tokens*. You can generate multiple API Tokens and name them according to their uses. The API Tokens are related only with your user account and will allow access only to your subscriptions and datasources, according to the set up visibility.

The API Token will be present in the API request's **header** under the field name "Token":

`Token: <api_token>`

The response from the server will be `200 OK` in case of success, or an http error code in case of failure. The body of the response will contain the text "OK" in case of success, or a description of the error in case of error (in JSON format)

> Request:

```http
POST /api/get_subscriptions HTTP/1.1
Host: <host_ip> 
Token: invalid_token
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW
```

> Response:

```
401 Unauthorized

{
  "error": "Invalid token"
}
```

<aside class="notice">
You must replace `<api_token>` with your personal API Token.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON structured like this:

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

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
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

