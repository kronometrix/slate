---
title: API Reference

language_tabs:
  - shell 
  - php
  - java

toc_footers:
  - <a href='http://kronometrix.io'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:

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

> HTTP POST Request:

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

To access the API (either public or private) you will need an *API Token*. The API Token can be obtained by logging in Kronometrix and accessing the section *Settings -> API Tokens*. You can generate multiple API Tokens and name them according to their uses. The API Tokens are related only with your user account and will allow access only to your subscriptions and datasources, according to the set up visibility.

The API Token will be present in the API request's **header** under the field name "Token":

`Token: <api_token>`

The response from the server will be `200 OK` in case of success, or an http error code in case of failure. The body of the response will contain the text "OK" in case of success, or a description of the error in case of error (in JSON format)

<aside class="notice">
You must replace <code>&lt;api_token&gt;</code> with your personal API Token.
</aside>


# Public API

## List available subscriptions

> Request

```shell
curl -X POST -H "Token: <api_token>" "http://<kronometrix_url>/api/get_subscriptions"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronometrix_url>/api/get_subscriptions');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://<kronometrix_url>/api/get_subscriptions")
  .post(null)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
[
  {
    "monitoring_object": "cpd",
    "id": "998d1bd7d61e8850952902c3bc3a81f1",
    "monitoring_object_name": "Computer Performance",
    "name": "My Subscription",
    "description": "This is an example subscription"
  },
  {
    "monitoring_object": "wpd",
    "id": "08b861f5919722505166b81f95fde672",
    "monitoring_object_name": "Web Application Framework Performance",
    "name": "My Web Subscription",
    "description": ""
  }
]
```

This endpoint retreives the list of datasources the user has access to, with associated informations.

### Request

`http://<kronometrix_url>/api/get_subscriptions`

*No parameters needed*

### Response

A JSON-encoded hash table, with the element "subscriptions" of type array, each element of the array corresponding to a subscription and having these fields:

Field | Details
----- | -------
id | The ID of the subscription
name | The name of the subscription
description | The description of the subscription
monitoring_object | The ID of the monitoring object
monitoring_object_name | The name of the monitoring object


