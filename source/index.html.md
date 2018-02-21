---
title: API Reference

language_tabs:
  - shell 
  - php
  - java

toc_footers:
  - API ver. 1.3
  - <a href='http://kronometrix.io'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:

search: true
---

# Introduction

This is the place where you will find details about the accessible API provided by Kronometrix for its users.

The Kronometrix API can be accessed over HTTP, using standard POST requests.

# Accessing the API

> HTTP POST Request:

```http
POST /api/get_subscriptions HTTP/1.1
Host: 192.168.2.56
Token: 8c24562ce3ae7812ef709934b7858b03
```

> Response:

```
401 Unauthorized

{
  "error": "Invalid token"
}
```

To access the API you will have to use one of the two authentication methods:
 
* using an *API Token* 
* using basic authentication

The response from the server will be `200 OK` in case of success, or an http error code in case of failure. The body of the response will consist of a string with JSON-encoded data, with various fields in case of succes, or a description of the error in case of failure.

# Authentication

## Authentication with username & password and with token

To authentication with the Kronometrix server using the API you can use one of the two authentication methods:

* using an *API Token* (recommended)
* using basic authentication

### Using an API Token

> API Token authentication

```http
POST /api/get_subscriptions HTTP/1.1
Host: 192.168.2.56
Token: 8c24562ce3ae7812ef709934b7858b03
```

The recommended authentication method is the one using an API Token. This method is more secure and also offers better performance.

This method consists of using an API Token with every HTTP request you make to the API. The token must be present in the API request's **header** under the field name "Token":

`Token: <api_token>`

The API Token can be obtained by logging in Kronometrix and accessing the section *Settings -> API Tokens*. You can generate multiple API Tokens and name them according to their uses. The API Tokens are related only with your user account and will allow access only to your subscriptions and datasources, according to the set up visibility.

### Using basic authentication

> Basic authentication

```http
POST /api/get_subscriptions HTTP/1.1
Host: 192.168.2.56
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=
```

The second authentication method is the one using "basic authentication". With this method, you can use your username and password directly to make API requests. This method is less secure because it sends authentication information in plain text. Also, it is less performant server-side.

All examples below will use API Token authentication, but basic authentication can also be used.

## Create new API Token

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "token_name": "The token name"
    }
}' "http://<kronometrix_url>/api/create_token"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronometrix_url>/api/create_token');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "token_name": "The token name"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"token_name\": \"The token name\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronometrix_url>/api/create_token")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "token": "b3442fa2c7730d78a52497e37598e56c"
}
```

It is possible to create a new API Token using this API call:

### Request

`http://<kronometrix_url>/api/create_token`

Parameter | Details
--------- | -------
token_name | The name of the token to be created

### Response

The API Token that has been created

Field | Details
----- | -------
token | The newly created API Token

## Delete API Token

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "token": "477f78c737e6300e52184f1e31f29942"
    }
}' "http://<kronomentrix_url>/api/delete_token"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/delete_token');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "token": "477f78c737e6300e52184f1e31f29942"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"token\": \"477f78c737e6300e52184f1e31f29942\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/delete_token")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "removed": "477f78c737e6300e52184f1e31f29942"
}
```

You can delete existing API tokens using this API call:

### Request

`http://<kronometrix_url>/api/delete_token`

Parameter | Details
--------- | -------
token | The token to be deleted

### Response

The API Token that has been deleted

Field | Details
----- | -------
removed | The deleted API Token

If the token didn't exist, it will still appear as if it has been deleted (so you will get no error from trying to delete an inexistent token)

# Provisioning

## List subscriptions

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
    "user_role": [
      "owner"
    ],
    "id": "998d1bd7d61e8850952902c3bc3a81f1",
    "monitoring_object_name": "Computer Performance",
    "name": "My Subscription",
    "description": "This is an example subscription"
  },
  {
    "monitoring_object": "wpd",
    "user_role": [
      "admin"
    ],
    "id": "08b861f5919722505166b81f95fde672",
    "monitoring_object_name": "Web Application Framework Performance",
    "name": "My Web Subscription",
    "description": ""
  }
]
```

This endpoint retreives the list of subscriptions the user has access to, with associated informations.

### Request

`http://<kronometrix_url>/api/get_subscriptions`

*No parameters needed*

### Response

A JSON-encoded array, each element of the array corresponding to a subscription and having these fields:

Field | Details
----- | -------
id | The ID of the subscription
name | The name of the subscription
description | The description of the subscription
monitoring_object | The ID of the monitoring object
monitoring_object_name | The name of the monitoring object
user_role | Array of roles the user has for this subscription

## Create new subscription

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "name": "Name of the subscription",
        "description": "The description of the API-created subscription",
        "type": "wpd"
    }
}' "http://<kronomentrix_url>/api/create_subscription"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/create_subscription');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "name": "Name of the subscription",
        "description": "The description of the API-created subscription",
        "type": "wpd"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"name\": \"Name of the subscription\",\n        \"description\": \"The description of the API-created subscription\",\n        \"type\": \"wpd\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/create_subscription")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "subscription_id": "5a332f210e37bb8c2a928bccd5423e33"
}
```

This endpoint allows the user to create a new subscription of a specified type.

### Request

`http://<kronometrix_url>/api/create_subscription`

Parameter | Details
--------- | -------
name | The name of the new subscription
description | A description of the new subscription (optional)
type | The type of the new subscription

### Response

The ID of the new subscription

Field | Details
----- | -------
subscription_id | The ID of the new subscription

## Delete subscription

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "5a332f210e37bb8c2a928bccd5423e33"
    }
}' "http://<kronomentrix_url>/api/delete_subscription"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/delete_subscription');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "5a332f210e37bb8c2a928bccd5423e33"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"5a332f210e37bb8c2a928bccd5423e33\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/delete_subscription")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "removed": "5a332f210e37bb8c2a928bccd5423e33"
}
```

This endpoint allows the user to delete an existing subscription. 

<aside class="warning">
Attention! This operation can't be reverted, so use with care!
</aside>

### Request

`http://<kronometrix_url>/api/delete_subscription`

Parameter | Details
--------- | -------
sid | The ID of the subscription to be deleted

### Response

The ID of the subscription that has been deleted

Field | Details
----- | -------
removed | The ID of the subscription that has been deleted

## Rename subscription

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "4be4d9aef4ed832301e331c30ab88e5d",
        "name": "Another name",
        "description": "Edited description"
    }
}' "http://<kronomentrix_url>/api/rename_subscription"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/rename_subscription');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "4be4d9aef4ed832301e331c30ab88e5d",
        "name": "Another name",
        "description": "Edited description"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"4be4d9aef4ed832301e331c30ab88e5d\",\n        \"name\": \"Another name\",\n        \"description\": \"Edited description\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/rename_subscription")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "renamed": "4be4d9aef4ed832301e331c30ab88e5d"
}
```

This endpoint provides the user with the possibility to change the name of the subscription, or its description.

### Request

`http://<kronometrix_url>/api/rename_subscription`

Parameter | Details
--------- | -------
sid | Subscription ID to be renamed
name | The new name of the subscription
description | The new description of the subscription (optional)

### Response

The ID of the subscription that has been renamed

Field | Details
----- | -------
renamed | The ID of the subscription that has been renamed

## List datasources

This endpoint retreives the list of datasources which correspond to a subscription and which can be accessed by the user.

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "<subscription_id>"
    }
}' "http://<kronomentrix_url>/api/get_ds"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronometrix_url>/api/get_ds');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "<subscription_id>"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"<subscription_id>\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronometrix_url>/api/get_ds")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
[
  {
    "id": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
    "name": "ds-name1",
    "monitoring_object": "cpd"
  },
  {
    "id": "566ac121-59c7-509d-92e0-028b3c8d6154",
    "name": "other-ds",
    "monitoring_object": "cpd"
  }
]
```

### Request

`http://<kronometrix_url>/api/get_ds`

Parameter | Details
--------- | -------
sid | Subscription ID for which to query for datasources

### Response

A JSON-encoded array, each element of the array corresponding to a datasource and having these fields:

Field | Details
----- | -------
id | The ID of the datasource
name | The name of the datasource
monitoring_object | The ID of the monitoring object

## Search datasources

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "search": "te"
    }
}' "http://<kronomentrix_url>/api/search_ds"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/search_ds');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "search": "te"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"9ee583c7d0a8b314c947dccfdcd922ca\",\n        \"search\": \"te\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/search_ds")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
[
  {
    "id": "a1153f37-57f5-5046-b117-ac195faff39a",
    "monitoring_object": "cpd",
    "name": "tethys"
  }
]
```

This endpoint offers the users the possibility to search for a datasource in a subscription by datasource name or ID. The call will return a list of datasources that contain the search string in their name or ID.

### Request

`http://<kronometrix_url>/api/search_ds`

Parameter | Details
--------- | -------
sid | Subscription ID in which to search for datasources
search | The search term (part of the datasource's name or ID)

### Response

A JSON-encoded array, each element of the array corresponding to a datasource and having these fields:

Field | Details
----- | -------
id | The ID of the datasource
name | The name of the datasource
monitoring_object | The ID of the monitoring object

## Delete datasources

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "datasources": [
            "b2fadd11-f143-529f-bd97-5f5e30b19499",
            "527debbd-c89b-5ea9-8052-ea8a4ae93cee"
        ]
    }
}' "http://<kronomentrix_url>/api/delete_ds"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/delete_ds');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "datasources": [
            "b2fadd11-f143-529f-bd97-5f5e30b19499",
            "527debbd-c89b-5ea9-8052-ea8a4ae93cee"
        ]
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"9ee583c7d0a8b314c947dccfdcd922ca\",\n        \"datasources\": [\n            \"b2fadd11-f143-529f-bd97-5f5e30b19499\",\n            \"527debbd-c89b-5ea9-8052-ea8a4ae93cee\"\n        ]\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/delete_ds")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "deleted": [
    "b2fadd11-f143-529f-bd97-5f5e30b19499",
    "527debbd-c89b-5ea9-8052-ea8a4ae93cee"
  ]
}
```

This endpoint allows the user to delete one or many datasources in a subscription. 

<aside class="warning">
Attention! This operation can't be reverted, so use with care!
</aside>

### Request

`http://<kronometrix_url>/api/delete_ds`

Parameter | Details
--------- | -------
sid | The ID of the subscription from which to delete the datasources
datasources | An array of datasource IDs

### Response

The list of datasource IDs that have been deleted

Field | Details
----- | -------
deleted | An array of datasource IDs that have been deleted

## List devices of a datasource

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "dsid": "e03b7160-f82b-5125-871b-3567e76cb190"
    }
}' "http://<kronomentrix_url>/api/get_ds_devices"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/get_ds_devices');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "dsid": "e03b7160-f82b-5125-871b-3567e76cb190"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"9ee583c7d0a8b314c947dccfdcd922ca\",\n        \"dsid\": \"e03b7160-f82b-5125-871b-3567e76cb190\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/get_ds_devices")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "eth0": [
    "linux-nicrec"
  ],
  "sda2": [
    "linux-diskrec"
  ],
  "cpu0": [
    "linux-cpurec"
  ],
  "sda1": [
    "linux-diskrec"
  ],
  "cpu1": [
    "linux-cpurec"
  ],
  "sda5": [
    "linux-diskrec"
  ],
  "sda": [
    "linux-diskrec"
  ],
  "system": [
    "linux-sysrec",
    "linux-hdwrec"
  ]
}
```

This endpoint returns the list of devices and associated messages, for a certain datasource.

### Request

`http://<kronometrix_url>/api/get_ds_devices`

Parameter | Details
--------- | -------
sid | The ID of the subscription in which the datasource resides
dsid | The ID of the datasource

### Response

A JSON-encoded hash table, with each key representing a device ID, and the corresponding value consisting of an array of message names for that device.

## Send data

> Request

```shell
curl -X POST -H "Token: <api_token>" -H "Content-Type: application/x-www-form-urlencoded" -d 'payload=win-sysrec:9ee583c7d0a8b314c947dccfdcd922ca:TEST:system:1458174060:0.17:0.00:0.00:0.09:0.09:99.61:0.01:0.00:49:790:34.20:5627684:10826808:16454492:65.80:0.00:0:2490368:2490368:0:0.00:0:9:146.42:0:0:0:9:146.42:8:1.23:0:0:5:0.59:0:0:14:1.82:48aae8722de9a0f1b50b906ee32c69f888ee7b2deaf42e3133ec6dbece4ba31b' "http://<kronomentrix_url>/api/private/send_data"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/private/send_data');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'content-type' => 'application/x-www-form-urlencoded',
  'token' => '<api_token>'
));

$request->setContentType('application/x-www-form-urlencoded');
$request->setPostFields(array(
  'payload' => 'win-sysrec:9ee583c7d0a8b314c947dccfdcd922ca:TEST:system:1458174060:0.17:0.00:0.00:0.09:0.09:99.61:0.01:0.00:49:790:34.20:5627684:10826808:16454492:65.80:0.00:0:2490368:2490368:0:0.00:0:9:146.42:0:0:0:9:146.42:8:1.23:0:0:5:0.59:0:0:14:1.82:48aae8722de9a0f1b50b906ee32c69f888ee7b2deaf42e3133ec6dbece4ba31b'
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

MediaType mediaType = MediaType.parse("application/x-www-form-urlencoded");
RequestBody body = RequestBody.create(mediaType, "payload=win-sysrec%3A9ee583c7d0a8b314c947dccfdcd922ca%3ATEST%3Asystem%3A1458174060%3A0.17%3A0.00%3A0.00%3A0.09%3A0.09%3A99.61%3A0.01%3A0.00%3A49%3A790%3A34.20%3A5627684%3A10826808%3A16454492%3A65.80%3A0.00%3A0%3A2490368%3A2490368%3A0%3A0.00%3A0%3A9%3A146.42%3A0%3A0%3A0%3A9%3A146.42%3A8%3A1.23%3A0%3A0%3A5%3A0.59%3A0%3A0%3A14%3A1.82%3A48aae8722de9a0f1b50b906ee32c69f888ee7b2deaf42e3133ec6dbece4ba31b");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/private/send_data")
  .post(body)
  .addHeader("token", "<api_token>")
  .addHeader("content-type", "application/x-www-form-urlencoded")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```html
OK
```

This is the API call through which data enter Kronometrix. All datasources send data using this API call. This API call **supports only token-based authentication**!

### Request

`http://<kronometrix_url>/api/private/send_data`

Parameter | Details
--------- | -------
payload | The message to be processed by Kronometrix


### Response

The text `OK` in case of success (with HTTP code `200 OK`), or a text describing the error in case another HTTP code is received (thus denoting a failure).

### General considerations

- the authentication method for this call must be token-based (basic authentication doesn't work)
- the payload is sent as a `x-www-form-url-encoded` field (not as JSON-encoded data structure, like for the other API endpoints)
- some messages require to have a hash at the end. Generally, *sha256* hashing algorithm is used. This last field must contain the hash for the entire message up until the last separator

### Payload format

The payload contains a single message which is transmitted to Kronometrix. All messages are defined within the Kronometrix platform and are uniquely identified by their `message_id`. 

A message is composed of multiple fields separated by a character, which also is defined within the Kronometrix platform. The separator character is a property of the message. 
As a general recommendation, we propose the use of characters `:` or `;` as separators (keep in mind that the character `:` can be also found in the ISO formatted date field, so in this case the character `;` is recommended). 

The order of the fields is also defined in the platform as a characteristic of the message. The only restriction here is that the message **must always begin with the message ID**. The rest of the fields can be defined in any order, but the order in which the fields are specified in the message definition must be the same as the order in which the fields appear in the sent message.

A message is composed of the following fields:

- **message_id**: the ID of the message. This ID uniquely identifies the message within the platform. The message **must always begin with the message ID**.
- **sid**: the subscription ID. All messages must have this field.
- **dsid**: the ID of the datasource. All messages must have this field. Datasources are provisioned (created) when the first message with a new `dsid` is received. Then, to send data for the same datasource, you must use the same `dsid`.
- **dev\_id**: the ID of the device. All messages must have this field. A datasource can have multiple devices (for example, for a computer, which is regarded as a datasource, you can have multiple devices, like _CPU0_, _CPU1_, _disk_c_ etc.). If you don't want to use this feature, you can use a generic device ID, like `system`
- **timestamp**: the timestamp of the message. The timestamp can be specified as a UNIX timestamp (like the one in the example - the number of seconds since 1.01.1970) or as an ISO formatted date: `2016-09-27 18:50:07`
- various fields of type "numeric", "counter", "inventory", "metadata", "status" or "instrument":
  - _numeric_: a numeric field, which is processed for generating all statistics defined in the message. The field is also stored in the raw data file
  - _counter_: a numeric field which contains the sum of the values until the current moment. When received, it is processed as a difference between this value and the previous received value. The field is also stored in the raw data file
  - _inventory_: a general text field which is recorded as a "property" of the datasource. It can contain, for example, the host name, the OS name or version etc. The field is also stored in the raw data file
  - _metadata_: a general text field which sets one of the following object properties: _datasource name_, _datasource description_. The field is also stored in the raw data file only if it is defined as `ds_name_log` (for setting the datasource name)
  - _status_: a general text field which contains a status information. The possible values for this field are defined in the message definition. When received, Kronometrix calculates what percent of the time is spent for each status value. The field is also stored in the raw data file
  - _instrument_: a numeric field, similar with the type _numeric_, but which is **not** stored in the raw data file 
- **hash**: this field is optional (there are some messages which are defined without a hash field). It contains a hash (of type SHA256) of the entire message, without the hash field and the separator before it. The hash field must always be the last field in the message. 

## Send bulk data

> Request

```shell
curl -X POST -H "token: <api_token>" -H "message-id: <message_id>" -H "subscription-id: <subscription_id>" -H "datasource-id: <datasource_id>" -H "device-id: <device_id>" -d '<payload>' "http://<kronomentrix_url>/api/private/send_data_bulk"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/private/send_data_bulk');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'device-id' => '<device_id>',
  'datasource-id' => '<datasource_id>',
  'subscription-id' => '<subscription_id>',
  'message-id' => '<message_id>',
  'token' => '<api_token>'
));

$request->setBody('<payload>');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "<payload>");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/private/send_data_bulk")
  .post(body)
  .addHeader("token", "<api_token>")
  .addHeader("message-id", "<message_id>")
  .addHeader("subscription-id", "subscription_id")
  .addHeader("datasource-id", "datasource_id")
  .addHeader("device-id", "device_id")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```html
OK
```

This is the API call through which bulk data enter Kronometrix. Bulk data is the kind of generic data that is stored in Kronometrix as a file (raw data file), but **it is not parsed in any way**. This can be useful for uploading log dumps, binary data (e.g. images) or any other kind of big data.
This API call **supports only token-based authentication**!

### Request

`http://<kronometrix_url>/api/private/send_data_bulk`

Header field | Details
--------- | -------
token           | The API Token to be used for authentication
message-id      | The ID of the message; this message must be defined as `type="bulk"` in the LMO
subscription-id | The ID of the subscription
datasource-id   | The ID of the datasource
device-id       | The ID of the device
hash            | An *sha256* hash of the payload (_optional_)


### Response

The text `OK` in case of success (with HTTP code `200 OK`), or a text describing the error in case another HTTP code is received (thus denoting a failure).

### General considerations

- all the **header** fields, except "hash", are **required**
- the **body** of the request contains the **payload**; the size of the payload can be limited in the LMO (`request-max-size`)
- the LMO field `content-storage` defines if the latest payload received will overwrite the older content or it will be appended

# Summary statistics

## List message statistics

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "message": "<message_id>"
    }
}' "http://<kronometrix_url>/api/get_msg_stats"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronometrix_url>/api/get_msg_stats');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "message": "<message_id>"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"message\": \"<message_id>\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronometrix_url>/api/get_msg_stats")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "monitoring_object": "cpd",
  "message": "linux-sysrec",
  "stats": [
    {
      "name": "cpupct",
      "functions": {
        "MAX": [ [300, 300 ], [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ],
        "SUM": [ [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ],
        "COUNT": [ [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ],
        "MIN": [ [300, 300 ], [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ]
      }
    },
    {
      "name": "memusedpct",
      "functions": {
        "MAX": [ [300, 300 ], [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ],
        "SUM": [ [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ],
        "COUNT": [ [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ],
        "MIN": [ [300, 300 ], [10800, 60], [21600, 300], [43200, 900], [86400, 1800], [259200, 3600], [604800, 10800], [2592000, 43200], [7776000, 86400 ] ]
      }
    }
  ]
}
```

This endpoint lists the available statistics with intervals for a certain message. The message ID is the one listed for each device of the datasource.

### Request

`http://<kronometrix_url>/api/get_msg_stats`

Parameter | Details
--------- | -------
message | The message ID for which to obtain the available statistics and intervals

### Response

A JSON-encoded hash table with these fields:

Field | Details
----- | -------
monitoring_object | The ID of the monitoring object
message | The message ID (same with the "message" parameter sent)
stats | An array of objects, each object (hash-table) having these fields: "name" = parameter name; "functions" = an array of functions (hash table).<br/>Each element of the "functions" hash has the aggregation function ID as key ("MIN", "MAX", "SUM", "COUNT", "LAST", "PERCENTILE") and an array of intervals as value.<br/>Each interval is an array of 2 elements, first element representing the interval width in seconds, and the second element representing the resolution in seconds.

## List summary statistics functions

> Request

```shell
curl -X POST -H "Token: <api_token>" "http://<kronomentrix_url>/api/get_stats"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/get_stats');
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

MediaType mediaType = MediaType.parse("application/octet-stream");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/get_stats")
  .post(null)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
    "name": "The Library of Summary Statistics Functions",
    "description": "Includes Kronometrix summary statistics functions",
    "id": "libssf",
    "functions": {
      "MIN": {
        "name": "Min",
        "id": "MIN",
        "description": "Minimum value",
        "formats": [
          { "format": "Min([x1, x2, ...])", "description": "Returns the minimum value from the array" }
        ]
      },
      "MAX": {
        "name": "Max",
        "id": "MAX",
        "description": "Maximum value",
        "formats": [
          { "format": "Max([x1, x2, ...])", "description": "Returns the maximum value from the array" }
        ]
      },
      ...
    }
  }
```

This endpoint lists the available summary statistics functions available in Kronometrix.

### Request

`http://<kronometrix_url>/api/get_stats`

*No parameters needed*

### Response

A JSON-encoded hash describing the statistics functions library, with the following fields:

Field | Details
----- | -------
name | The name of the library
description | A short description of the library
id | An internal library ID
functions | A hash of objects, each object (hash-table) having these fields: "id" = the ID of the function; "name" = the name (definition) of the function; "description" = a short description of the function; "formats" = an array with the available function formats

## Get statistical data

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "9ee583c7d0a8b314c947882fdcd922ca",
        "dsid": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
        "device": "system",
        "message": "win-sysrec",
        "param": "cpupct",
        "function": "AVG",
        "interval": "10800",
        "resolution": "60"
    }
}' "http://<kronometrix_url>/api/get_values"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronometrix_url>/api/get_values');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "9ee583c7d0a8b314c947882fdcd922ca",
        "dsid": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
        "device": "system",
        "message": "win-sysrec",
        "param": "cpupct",
        "function": "AVG",
        "interval": "10800",
        "resolution": "60"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"9ee583c7d0a8b314c947882fdcd922ca\",\n        \"dsid\": \"527debbd-c89b-5ea9-8052-ea8a4ae93cee\",\n        \"device\": \"system\",\n        \"message\": \"win-sysrec\",\n        \"param\": \"cpupct\",\n        \"function\": \"AVG\",\n        \"interval\": \"10800\",\n        \"resolution\": \"60\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronometrix_url>/api/get_values")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
[
  [
    1457441220000,
    3.03
  ],
  [
    1457441280000,
    3.25
  ],
  [
    1457441340000,
    3.08
  ],
  [
    1457441400000,
    2.91
  ]
  ...
]
```

This endpoint lists statistical data for the required datasource, device, message, parameter, function, interval and resolution.

### Request

`http://<kronometrix_url>/api/get_values`

Parameter | Details
--------- | -------
sid | Subscription ID
dsid | Datasource ID
device | Device ID
message | Message ID
param | Parameter name
function | Aggregation function
interval | Interval width
resolution | Resolution

### Response

A JSON-encoded array, each element of the array being an array of 2 elements, the first element representing **the unix timestamp in milliseconds** (multiplied by 1000), and the second element representing the value of the parameter for that timestamp. The second value may be *null*, which means that there is not an available value for that timestamp.

# Raw data

## List raw data files

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "dsid": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
        "date": "2016-03-16"
    }
}' "http://<kronomentrix_url>/api/list_raw_data_files"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/list_raw_data_files');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "dsid": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
        "date": "2016-03-16"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"9ee583c7d0a8b314c947dccfdcd922ca\",\n        \"dsid\": \"527debbd-c89b-5ea9-8052-ea8a4ae93cee\",\n        \"date\": \"2016-03-16\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/list_raw_data_files")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
[
  {
    "file": "nicrec.e1iexpress.2016-03-16.krd",
    "size": "94 KB"
  },
  {
    "file": "cpurec.cpu2.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "cpurec.cpu5.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "hdwrec.system.2016-03-16.krd",
    "size": "126 KB"
  },
  {
    "file": "cpurec.cpu7.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "cpurec.cpu0.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "cpurec.cpu6.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "diskrec.diskC.2016-03-16.krd",
    "size": "124 KB"
  },
  {
    "file": "diskrec.diskD.2016-03-16.krd",
    "size": "116 KB"
  },
  {
    "file": "cpurec.cpu1.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "cpurec.cpu3.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "cpurec.cpu4.2016-03-16.krd",
    "size": "59 KB"
  },
  {
    "file": "sysrec.system.2016-03-16.krd",
    "size": "263 KB"
  }
]
```

This endpoint lists the available raw data files for a certain date (day). Please note that the date is in `yyyy-mm-dd` format!

### Request

`http://<kronometrix_url>/api/list_raw_data_files`

Parameter | Details
--------- | -------
sid | Subscription ID
dsid | Datasource ID
date | The date for which to list files

### Response

A JSON-encoded array, each element of the array being a hash table with two elements: *file* = the file name; *size* = the file size. 

## Download raw data file

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "dsid": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
        "file": "sysrec.system.2016-03-16.krd"
    }
}' "http://<kronometrix_url>/api/get_raw_data_file"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/get_raw_data_file');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "sid": "9ee583c7d0a8b314c947dccfdcd922ca",
        "dsid": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
        "file": "sysrec.system.2016-03-16.krd"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"sid\": \"9ee583c7d0a8b314c947dccfdcd922ca\",\n        \"dsid\": \"527debbd-c89b-5ea9-8052-ea8a4ae93cee\",\n        \"file\": \"sysrec.system.2016-03-16.krd\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/get_raw_data_file")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```plaintext
1458086400:0.58:0.00:0.00:0.30:0.28:98.63:0.01:0.00:74:1342:41.54:6834524:9619968:16454492:58.46:0.73:18064:2472304:2490368:0:5.41:0:11:188.44:0:0:0:11:193.85:11:2.89:0:0:7:0.80:0:0:18:3.69
1458086460:0.43:0.00:0.00:0.24:0.19:98.84:0.00:0.00:74:1339:41.52:6832560:9621932:16454492:58.48:0.73:18064:2472304:2490368:0:0.00:0:10:169.21:0:0:0:10:169.21:9:1.27:0:0:6:0.64:0:0:15:1.91
1458086520:0.43:0.00:0.00:0.27:0.15:98.77:0.00:0.00:74:1325:41.49:6827092:9627400:16454492:58.51:0.73:18064:2472304:2490368:0:1.34:0:10:143.63:0:0:0:10:144.97:10:2.30:0:0:6:0.67:0:0:17:2.97
1458086580:0.42:0.00:0.00:0.24:0.19:98.78:0.01:0.00:75:1317:41.49:6826668:9627824:16454492:58.51:0.73:18064:2472304:2490368:1:17.34:0:13:180.12:0:0:0:14:197.46:9:1.23:0:0:6:0.63:0:0:15:1.86
1458086642:0.78:0.00:0.00:0.33:0.46:98.41:0.02:0.00:74:1321:41.60:6845692:9608800:16454492:58.40:0.73:18064:2472304:2490368:0:0.26:0:20:662.37:0:0:0:20:662.63:10:2.35:0:0:7:0.72:0:0:18:3.07
...
```

This endpint allows users to download a raw data file.

### Request

`http://<kronometrix_url>/api/get_raw_data_file`

Parameter | Details
--------- | -------
sid | Subscription ID
dsid | Datasource ID
file | File name to download

### Response

The file content

# Widgets

## List widgets

> Request

```shell

curl -X POST -H "Token: <api_token>" "http://<kronomentrix_url>/api/get_widgets"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/get_widgets');
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
  .url("http://<kronomentrix_url>/api/get_widgets")
  .post(null)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "bc34d59ea95b38992569b9ac79e37b32": {
    "subscription_id": "9ee583c7d0a8b314c947dccfdcd922ca",
    "subscription_name": "Computer Performance",
    "params": {
      "cpu": {
        "val": "566ac121-59c7-509d-92e0-028b3c8d6154:system:linux-sysrec:cpupct:LAST:300:300"
      }
    },
    "settings": {
      "type": "gauge",
      "gauge_color": "#55af0f",
      "gauge_aswidget": true
    },
    "name": "CPU Gauge",
    "description": ""
  },
  "7a550181d038429b1dd4f8ccbc1ac758": {
    "subscription_id": "9ee583c7d0a8b314c947dccfdcd922ca",
    "subscription_name": "Computer Performance",
    "params": {
      "cpu": {
        "val": "527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:cpupct:AVG:10800:60",
        "color": "#619639"
      },
      "memory": {
        "val": "527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:memusedpct:AVG:10800:60",
        "color": "#0e74b2"
      }
    },
    "settings": {
      "type": "chart",
      "chart_aswidget": true,
      "chart_haslegend": true,
      "chart_type": "line"
    },
    "name": "CPU & Memory utilization",
    "description": "in %"
  }
}
```

This endpoint lists the available widgets for the current user, with associated information.

### Request

`http://<kronometrix_url>/api/get_widgets`

*No parameters needed*

### Response

A JSON-encoded array, each element of the array corresponding to a widget, having as the key the widget ID and containing these fields:

Field | Details
----- | -------
name | The name of the widget
description | The description of the widget
subscription_id | The ID of the subscription
subscription_name | The name of the subscription
params | An array of params, each element having as key the name of the param (unique) and as value a hash-table with the elements "val" having the param's value and "color" having the line color (color needed only for chart widgets)
settings | A hash-table containing at least one element - "type", which can be "chart", "indicator" or "gauge"; the rest of the hash's elements are dependant on the type of the widget

## Create new widget

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "name": "API-created widget",
        "description": "The description of the API-created widget",
        "subscription_id": "9ee583c7d0a8b314c947dccfdcd922ca",
        "params": {
            "cpu": {"val": "527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:cpupct:AVG:10800:60", "color": "#D40B51"},
            "memory": {"val": "527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:memusedpct:AVG:10800:60", "color": "#21C4B1"}
        },
        "settings": {
            "type": "chart",
            "chart_type": "line",
            "chart_aswidget": true,
            "chart_haslegend": true
        }
    }
}' "http://<kronomentrix_url>/api/create_widget"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/create_widget');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "name": "API-created widget",
        "description": "The description of the API-created widget",
        "subscription_id": "9ee583c7d0a8b314c947dccfdcd922ca",
        "params": {
            "cpu": {"val": "527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:cpupct:AVG:10800:60", "color": "#D40B51"},
            "memory": {"val": "527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:memusedpct:AVG:10800:60", "color": "#21C4B1"}
        },
        "settings": {
            "type": "chart",
            "chart_type": "line",
            "chart_aswidget": true,
            "chart_haslegend": true
        }
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"name\": \"API-created widget\",\n        \"description\": \"The description of the API-created widget\",\n        \"subscription_id\": \"9ee583c7d0a8b314c947dccfdcd922ca\",\n        \"params\": {\n            \"cpu\": {\"val\": \"527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:cpupct:AVG:10800:60\", \"color\": \"#D40B51\"},\n            \"memory\": {\"val\": \"527debbd-c89b-5ea9-8052-ea8a4ae93cee:system:win-sysrec:memusedpct:AVG:10800:60\", \"color\": \"#21C4B1\"}\n        },\n        \"settings\": {\n            \"type\": \"chart\",\n            \"chart_type\": \"line\",\n            \"chart_aswidget\": true,\n            \"chart_haslegend\": true\n        }\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/create_widget")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "widget_id": "65102c8e86cd5ed2baf60ebf69b31d9e"
}
```

This endpoint allows the user to create a new widget of a specified type, for a specified subscription.

### Request

`http://<kronometrix_url>/api/create_widget`

Parameter | Details
--------- | -------
name | The name of the new widget
description | The description for the new widget
subscription_id | The ID of the subscription for which the widget is created
params | A hash-table of parameters for this widget; the keys are paramater names, and the values have two elements:<ul><li>*val* = the parameter value, with the format <br/><code>&lt;ds_id&gt;:&lt;device_id&gt;:&lt;message_id&gt;:&lt;parameter&gt;:&lt;stat&gt;:&lt;interval&gt;:&lt;resolution&gt;</code></li><li>*color* = the parameter's line color; needed only for widgets of type *chart*</li></ul>
settings | A hash-table with the widget settings; each widget type has its own set of settings, as follows: <ul><li>**chart**:<ul><li>*type* = "chart"</li><li>*chart_type* = "line" or "stacked"</li><li>*chart_aswidget* = true/false - the control is displayed as widget or not</li><li>*chart_haslegend* = true/false - the chart has a legend or not</li></ul></li><li>**gauge**:<ul><li>*type* = "gauge"</li><li>*gauge_color* = the hex value of the color</li><li>*gauge_aswidget* = true/false - the control is displayed as widget or not</li></ul></li><li>**indicator**:<ul><li>*type* = "indicator"</li><li>*indicator_type* = "plain", "widget" or "colored"</li><li>*indicator_color* = the hex value of the color; needed only for *indicator_type*="colored"</li><li>*indicator_decimals* = the number of decimals; can be "0", "1" or "2"</li><li>*indicator_unit* = optional; a string with the indicator's unit</li></ul></li></ul>  

<aside class="info">
For the widgets of type <i>chart</i>, all parameters on a chart must have <b>the same interval and resolution</b>!
</aside>

### Response

The ID of the new widget

Field | Details
----- | -------
widget_id | The ID of the new widget

## Delete widgets

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "widgets": ["79a58bc264faa53967c532d43377fe38", "1415799bc10b5a1aae81ac862fde795b"]
    }
}' "http://<kronomentrix_url>/api/delete_widgets"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/delete_widgets');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "widgets": ["79a58bc264faa53967c532d43377fe38", "1415799bc10b5a1aae81ac862fde795b"]
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"widgets\": [\"79a58bc264faa53967c532d43377fe38\", \"1415799bc10b5a1aae81ac862fde795b\"]\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/delete_widgets")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
{
  "deleted": [
    "79a58bc264faa53967c532d43377fe38",
    "1415799bc10b5a1aae81ac862fde795b"
  ]
}
```

This endpoint allows the user to delete one or many widgets of his own. 

<aside class="warning">
Attention! This operation can't be reverted, so use with care!
</aside>

### Request

`http://<kronometrix_url>/api/delete_widgets`

Parameter | Details
--------- | -------
widgets | An array of widget IDs

### Response

The list of widget IDs that have been deleted

Field | Details
----- | -------
deleted | An array of widget IDs that have been deleted

## Get widget embed code

> Request

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "widget_id": "7a550181d038429b1dd4f8ccbc1ac758"
    }
}' "http://<kronomentrix_url>/api/get_widget_embed_code"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/get_widget_embed_code');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'token' => '<api_token>'
));

$request->setBody('{
    "params": {
        "widget_id": "7a550181d038429b1dd4f8ccbc1ac758"
    }
}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}
```

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/octet-stream");
RequestBody body = RequestBody.create(mediaType, "{\n    \"params\": {\n        \"widget_id\": \"7a550181d038429b1dd4f8ccbc1ac758\"\n    }\n}");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/get_widget_embed_code")
  .post(body)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```html
<iframe width="600" height="300" frameborder="0" src="http://<kronometrix_url>/widgets/<uid>/7a550181d038429b1dd4f8ccbc1ac758"></iframe>
```

This endpoint provides the possibility to easily obtain the HTTP code to embed the widget on another web page.

### Request

`http://<kronometrix_url>/api/get_widget_embed_code`

Parameter | Details
--------- | -------
widget_id | The ID of the widget to get the embed code for

### Response

The HTML code to use in your web page to embed this widget

# Info

## Get data analytics platform information

> Request

```shell
curl -X POST -H "Token: <api_token>" "http://<kronomentrix_url>/api/get_kinfo"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronomentrix_url>/api/get_kinfo');
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

MediaType mediaType = MediaType.parse("application/octet-stream");
Request request = new Request.Builder()
  .url("http://<kronomentrix_url>/api/get_kinfo")
  .post(null)
  .addHeader("token", "<api_token>")
  .build();

Response response = client.newCall(request).execute();
```

> Response

```json
[
    {
        "Type": "K500"
    },
    {
        "ID": "NA"
    },
    {
        "Version": "1.6.4"
    },
    {
        "Architecture": "x64"
    },
    {
        "Name": "K500 Development"
    },
    {
        "Last Updated": "NA"
    },
    {
        "Modules": [
            "authenticator: kauth 1.6.4",
            "kernel: kkernel 1.6.4",
            "messenger: kmesg 1.6.4",
            "monitor: kmon 1.6.4"
        ]
    },
    {
        "Manufactured": "Mon Jul 24 12:10:18 UTC 2017"
    },
    {
        "Vendor": "SDR Dynamics, Helsinki, Finland"
    },
    {
        "Registrant": "NA"
    },
    {
        "Email": "NA"
    },
    {
        "License": "NA"
    }
]
```

This endpoint provides the possibility to list details about the Kronometrix installation.

### Request

`http://<kronometrix_url>/api/get_kinfo`

*No parameters needed*

### Response

A JSON-encoded array with various details about the Kronometrix installation.