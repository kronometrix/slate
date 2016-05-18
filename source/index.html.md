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

# Summary statistics

## List available statistics

```shell
curl -X POST -H "Token: <api_token>" -d '{
    "params": {
        "message": "<message_id>"
    }
}' "http://<kronometrix_url>/api/get_stats"
```

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://<kronometrix_url>/api/get_stats');
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
  .url("http://<kronometrix_url>/api/get_stats")
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

`http://<kronometrix_url>/api/get_stats`

Parameter | Details
--------- | -------
message | The message ID for which to obtain the available statistics and intervals

### Response

A JSON-encoded hash table with these fields:

Field | Details
----- | -------
monitoring_object | The ID of the monitoring object
message | The message ID (same with the "message" parameter sent)
stats | An array of objects, each object (hash-table) having these fields: "name" = parameter name; "functions" = an array of functions (hash table).<br/>Each element of the "functions" hash has the aggregation function as key ("MIN", "MAX", "SUM", "COUNT", "LAST", "PERCENTILE") and an array of intervals as value.<br/>Each interval is an array of 2 elements, first element representing the interval width in seconds, and the second element representing the resolution in seconds.

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
RequestBody body = RequestBody.create(mediaType, "{\n  }\n}");
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
  "widget_id": "5a332f210e37bb8c2a928bccd5423e33"
}
```

This endpoint allows the user to create a new widget of a specified type, for a specified subscription.

### Request

`http://<kronometrix_url>/api/create_widget`

Parameter | Details
--------- | -------


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