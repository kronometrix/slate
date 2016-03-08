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

## List subscription's datasources

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
    "devices": {
      "cpu6": [ "win-cpurec" ],
      "e1iexpress": [ "win-nicrec" ],
      "cpu2": [ "win-cpurec" ],
      "cpu1": [ "win-cpurec" ],
      "diskD": [ "win-diskrec" ],
      "cpu7": [ "win-cpurec" ],
      "cpu0": [ "win-cpurec" ],
      "system": [ "win-sysrec", "win-hdwrec" ],
      "cpu4": [ "win-cpurec" ],
      "diskC": [ "win-diskrec" ],
      "cpu3": [ "win-cpurec" ],
      "cpu5": [ "win-cpurec" ]
    },
    "id": "527debbd-c89b-5ea9-8052-ea8a4ae93cee",
    "name": "ds-name1",
    "monitoring_object": "cpd"
  },
  {
    "devices": {
      "dm-0": [ "linux-diskrec" ],
      "cpu2": [ "linux-cpurec" ],
      "eth0": [ "linux-nicrec" ],
      "cpu1": [ "linux-cpurec" ],
      "sda2": [ "linux-diskrec" ],
      "dm-1": [ "linux-diskrec" ],
      "cpu0": [ "linux-cpurec" ],
      "sda1": [ "linux-diskrec" ],
      "system": [ "linux-sysrec", "linux-hdwrec" ],
      "sda5": [ "linux-diskrec" ],
      "sda": [ "linux-diskrec" ],
      "cpu3": [ "linux-cpurec" ]
    },
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
devices | A hash-table of devices available for each datasource; each element contains the name of the device as key and an array of message IDs as the value of the hash

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
        "SUM": [ ... ],
        "COUNT": [ ... ],
        "MIN": [ ... ]
      }
    },
    ...
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
stats | An array of objects, each object (hash-table) having the fields "name" representing the parameter name and the field "functions" containing an array of functions (hash table). Each element of the "functions" hash has the aggregation function as key ("MIN", "MAX", "SUM", "COUNT", "LAST", "PERCENTILE") and an array of intervals as value. Each interval is an array of 2 elements, first element representing the interval width in seconds, and the second element representing the resolution in seconds.

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
