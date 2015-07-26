---
title: API Reference

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='http://xlabsgaze.com'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the xLabsAnalytics API! You can use our API to access xLabsAnalytics API endpoints.

Language bindings are available in Shell and Javascript. Code examples available in the dark area to the right.

# Authentication

```shell
N/A
```

```javascript
N/A
```

N/A. Certain routes may not require authentication.

# key

## GET /key

```shell
curl -X GET http://api.pinkpineapple.me/key 
```

```javascript
N/A
```

> Reply of JSON Structure:

```json
{"session_key":"a5d0be16-e39e-4b06-bee4-29126915cca1"}
```

This endpoint obtains a session key.

### HTTP Request
`GET http://api.pinkpineapple.me/key`

# data

## POST /data

```shell
curl -H "Content-Type: application/json" -X POST \
    -d '{
            "session_key": "abc",
            "url":"www.example.com/test",
            "domain":"www.example.com",
            "data": "BASE64_COMPRESSED_STRING"
        }'\
    http://api.pinkpineapple.me/data
```

```javascript
N/A
```

> On success, reply of:

```json
{
    "done": true
}
```

This endpoint submits eye gaze data to the server.

### HTTP Request

`POST http://api.pinkpineapple.me/data`

<aside class="warning">
An invalid data string will throw an error!
</aside>

## GET /data

```shell
curl -X GET http://api.pinkpineapple.me/data/x3DeknkJpoe2maj93
```

```javascript
N/A
```

> The above command returns JSON structured like this:

```json
{
    "id": "ed30ls92nkjs093",
    "timestamp": "2015-07-26T00:26:57.999Z",
    "gazeObj": {
        "x": 123.45,
        "y": 234.56
    },
    "browser_screen": {
        "width": 1920, 
        "height": 1080
    },
    "domain": "www.example.com",
    "url": "www.example.com/page",
    "component": "Component One",
    "session_key": "800a827b-679c-42be-85c0-b87a8744faed"
}
```

This endpoint retrieves a specific eye gaze data object.


### HTTP Request

`GET http://api.pinkpineapple.me/data/:id?`

# clicks

## POST /clicks

```shell
curl -H "Content-Type: application/json" -X POST \
    -d '{
            "url": "www.example.com/page",
            "domain": "example.com",
            "session_key": "800a827b-679c-42be-85c0-b87a8744faed",
            "clicks": [
                {
                    "timestamp": "2015-07-26T00:26:57.999Z"
                    "x": 50.0,
                    "y": 25.0
                    "tags": [
                        {
                            "name": "news",
                            "classes": "news sports"
                        }
                    ]
                },
                {
                    "timestamp": "2015-07-26T00:26:59.999Z"
                    "x": 50.0,
                    "y": 85.0,
                    "tags": []
                }
            ]
        }' http://api.pinkpineapple.me/clicks
```

```javascript
N/A
```

> On success, reply of:

```json
{
    "done": true
}
```

Note that clicks data is sent seperately from eye gaze date (in the /data route)

### HTTP Request

`POST http://api.pinkpineapple.me/clicks`

# site

## GET /site

```shell
curl -X GET http://api.pinkpineapple.me/site/
```

```javascript
N/A
```

> The above command returns JSON structured like this:

```json
{
    "sites": [
        {
            "site_id": 1,
            "site_url": "www.website1.com",
            "sessions_today": 10000,
            "pages": [
                {
                    "page_id": 43,
                    "page_url": "www.website1.com/page1",
                    "components": [
                        {
                            "component_id": 342,
                            "component_label": "top_banner"
                        },
                        {
                            "component_id": 342,
                            "component_label": "top_banner"
                        }
                    ]
                }
            ]
        },
        {
            "site_id": 2,
            "site_url": "www.website2.com",
            "sessions_today": 10000,
            "pages": [
                {
                    "page_id": 21,
                    "page_url": "www.website1.com/page1",
                    "components": []
                }
            ]
        }
    ]
}

```

This route returns a JSON object detailing the sites along with their respective pages/components managed

### HTTP Request
`GET http://api.pinkpineapple.me/site/:user_id?`

### Url Parameters

Parameter | Description
--------- | -----------
user_id   | *OPTIONAL* ID of the user. No id defaults to current user.

# stats

## GET /stats

```shell
curl -X GET http://api.pinkpineapple.me/stats/1?start_date=12345&end_date=54321
```

```javascript
N/A
```

> Returns JSON of structure:

```json
{
    "total_sessions": 15000,
    "page_reviews": 5379534,
    "session_increasement": 4400,
    "average_session_time": 119
}
```

Of use in the "Dashboard" section.

### HTTP Request
`GET http://api.pinkpineapple.me/stats/:site_id?/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | *OPTIONAL* ID of a site. No id defaults to global stats for current user
page_id   | *OPTIONAL* ID of a page in site.
component_id | *OPTIONAL* ID of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_date| Epoch Time | Start date range
end_date  | Current Time | End date range

# charts

## GET /charts/session_time

```shell
curl -X GET http://api.pinkpineapple.me/charts/session_time/1?start_date=12345&end_date=54321
```

```javascript
N/A
```

> Reply of JSON:

```json
{
    "fixation_time": [
        { "time": 1, "fixations": 226 },
        { "time": 2, "fixations": 431 },
        { "time": 3, "fixations": 542 },
        { "time": 4, "fixations": 074 },
        { "time": 5, "fixations": 193 }
    ]
}
```

Gets chart data for sessions versus time.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/session_time/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | ID of a site.
page_id   | *OPTIONAL* ID of a page in site.
component_id | *OPTIONAL* ID of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_date| Epoch Time | Start date range. Unix Time.
end_date  | Current Time | End date range. Unix Time.

## GET /charts/fixation_time

```shell
curl -X GET http://api.pinkpineapple.me/charts/fixation_time/1?start_date=12345&end_date=54321
```

```javascript
N/A
```

> Reply of JSON:

```json
{
    "fixation_time": [
        { "time": 1, "fixations": 226 },
        { "time": 2, "fixations": 431 },
        { "time": 3, "fixations": 542 },
        { "time": 4, "fixations": 074 },
        { "time": 5, "fixations": 193 }
    ]
}
```

Gets chart data for fixations versus time.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/fixation_time/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | ID of a site.
page_id   | *OPTIONAL* ID of a page in site.
component_id | *OPTIONAL* ID of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_date| Epoch Time | Start date range. Unix Time.
end_date  | Current Time | End date range. Unix Time.
