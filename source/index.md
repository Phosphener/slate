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

Time used is Unix timestamp in milliseconds.

# Authentication

```shell
N/A
```

```javascript
N/A
```

N/A. Certain routes may not require authentication.

# key

## GET /key/participant

```shell
curl -X GET http://api.pinkpineapple.me/key/participant
```

> Reply of JSON Structure:

```json
{
    "participant_key": "a5d0be16-e39e-4b06-bee4-29126915cca1"
}
```

This endpoint obtains a participant key to identify the participant.

### HTTP Request
`GET http://api.pinkpineapple.me/key/participant`

## GET /key/session

```shell
curl -X GET http://api.pinkpineapple.me/key/session
```

```javascript
N/A
```

> Reply of JSON Structure:

```json
{
    "session_key": "a5d0be16-e39e-4b06-bee4-29126915cca1"
}
```

This endpoint obtains a session key.

### HTTP Request
`GET http://api.pinkpineapple.me/key/session`

# data

## POST /data

```shell
curl -H "Content-Type: application/json" -X POST \
    -d '{
            "session_key": "abc",
            "domain":"www.example.com",
            "url":"www.example.com/test",
            "data": "BASE64_COMPRESSED_STRING"
        }'\
    http://api.pinkpineapple.me/data
```

```javascript
N/A
```

> BASE64_COMPRESSED_STRING must contain:

```json
{
    "dataIndex": 1
}
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

### Body Parameters

Parameter | Description
--------- | -----------
session_key | Session key as obtained in /key
domain | Domain of site
url | Url in site
data | BASE64 Compressed String (Secret Structure) (I'm just lazy to write it down right now)

<aside class="warning">
An invalid `data` string will throw an error!
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
Not for use. Only testing purposes.

### HTTP Request

`GET http://api.pinkpineapple.me/data/:id?`

### URL Parameters
Parameter | Description
--------- | -----------
id | *OPTIONAL* Document id of data in database. Testing purposes only.

# clicks

## POST /clicks

```shell
curl -H "Content-Type: application/json" -X POST \
    -d '{
            "domain": "www.example.com",
            "url": "www.example.com/page",
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

`POST http://api.pinkpineapple.me/clicks/:website_id/:page_id?/:component_id?`

### Body Parameters
Parameter | Description
--------- | -----------
domain | Domain of site
url | Url of page in site
session_key | Session key as obtained in /key
clicks | Clicks data. Too lazy to describe whole structure right now.

## GET /clicks

```shell
curl -X GET http://api.pinkpineapple.me/clicks/www.example.com/www.example/com%2Fpage?start_time=0&end_time=123456789123456789
```

```javascript
N/A
```

> The above command returns JSON structured like this:

```json
{
    "count":2,
    "rows":[
        {
            "x":1,
            "y":2,
            "click_timestamp":1,
            "domain":"www.example.com",
            "url":"www.example.com",
            "session_key":"TEST_SESSION_KEY"
        },
        {
            "x":1.5,
            "y":2.5,
            "click_timestamp":2,
            "domain":"www.example.com",
            "url":"www.example.com",
            "session_key":"TEST_SESSION_KEY"
        }
    ]
}
```

Gets clicks based on website/page/component

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site. No id defaults to global stats for current user
page_id   | *OPTIONAL* URL of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | Start date range. Unix timestamp in milliseconds
end_time  | Current Time | End date range. Unix timestamp in milliseconds

# site

## GET /site

```shell
curl -X GET http://api.pinkpineapple.me/site
```

```javascript
N/A
```

> The above command returns JSON structured like this:

```json
{
    "sites": [
        {
            "site_url": "www.website1.com",
            "pages": [
                {
                    "page_url": "www.website1.com/page1",
                    "components": [
                        {
                            "component_label": "top_banner"
                        },
                        {
                            "component_label": "top_banner"
                        }
                    ]
                }
            ]
        },
        {
            "site_url": "www.website2.com",
            "pages": [
                {
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
`GET http://api.pinkpineapple.me/site/:website_id?`

### Url Parameters
Parameter | Description
--------- | -----------
website_id | *OPTIONAL* Domain of a website. Returns site structure for a single site. Else, returns all available sites.

## POST /site

```shell
curl -H "Content-Type: application/json" -X POST \
    -d '{
        "site_url": "www.example.com",
        "pages": [
            {
                "page_url": "www.example.com/page"
                "components": [
                    {
                        "component_label": "Component One"
                    }                
                ]
            }
        ]
    }' http://api.pinkpineapple.me/site
```

```javascript
N/A
```

> JSON Site object for created website is returned

```json
{
    "site_url": "www.example.com",
    "pages": [
        {
            "page_url": "www.example.com/page",
            "components": [
                {
                    "component_label": "Component One"
                }
            ]
        }
    ]
}
```

This route add a website to database.

### HTTP Request
`POST http://api.pinkpinepple.me/site`

### Body Parameters
Parameter | Description
--------- | -----------
site_url | Website domain
pages[] | Array containing pages in website domain
pages[i].page_url | Url of page
pages[i].components[] | Array containing components in page pages[i]
pages[i].components[j].component_label | Name of component

## PUT /site

```shell
curl -H "Content-Type: application/json" -X PUT \
    -d '{
        "pages": [
            {
                "page_url": "www.example.com/page2"
                "components": [
                    {
                        "component_label": "Component Two"
                    }                
                ]
            }
        ]
    }' http://api.pinkpineapple.me/site/www.example.com/?overwrite=false
```

```javascript
N/A
```

> Above example adds the page www.example.com/page2 to the domain www.example.com. A component named Component Two is also added to the page. JSON Object containing data of entire site is returned.

```json
{
    "site_url": "www.example.com",
    "pages": [
        {
            "page_url": "www.example.com/page",
            "components": [
                {
                    "component_label": "Component One"
                }
            ]
        },
        {
            "page_url": "www.example.com/page2",
            "components": [
                {
                    "component_label": "Component Two"
                }
            ]
        }
    ]
}
```

This route updates a website. Careful with overwrite flag.

### HTTP Request
`PUT http://api.pinkpineapple.me/site/:website_id`

### Url Parameters
Parameter | Description
--------- | -----------
website_id | Domain of website to be updated.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
overwrite | false | If set to true, object sent in body will overwrite entire object in database. If set to false, merely appends to object in database.

### Body Parameters
Parameter | Description
--------- | -----------
site_url | *OPTIONAL* Website domain
pages[] | *OPTIONAL* Array containing pages in website domain
pages[i].page_url | *OPTIONAL* Url of page
pages[i].components[] | *OPTIONAL* Array containing components in page pages[i]
pages[i].components[j].component_label | *OPTIONAL* Name of component

## DELETE /site

```shell
curl -X DELETE http://api.pinkpineapple.me/site/www.example.com
```

```javascript
N/A
```

> JSON reply on success of:

```json
{
    "done": true
}
```

Deletes a site.

### HTTP Request
`GET http://api.pinkpineapple.me/site/:site_id`

### URL Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.

# fixation

## GET /fixation

```shell
curl -X GET http://api.pinkpineapple.me/stats/www.example.com/www.example/com%2Fpage?start_time=0&end_time=123456789123456789
```

```javascript
N/A
```

> Return JSON of structure

```json
{
    "count":1499,
    "rows": [
        {
            "start_time":1440034850000,
            "end_time":1440034850100,
            "domain":"www.example.com",
            "url":"www.example.com/page",
            "component":"Component One",
            "session_key":"zcbmadgjlqetup02468"
        },
        {
            "start_time":1440034850225,
            "end_time":1440034850300,
            "domain":"www.example.com",
            "url":"www.example.com/page",
            "component":"Component One",
            "session_key":"zcbmadgjlqetup02468"
        }
    ]
}
```

Use this to grab fixation information. Higher level parsing needed to obtain useful data.

### HTTP Request
`GET http://api.pinkpineapple.me/fixation/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site. No id defaults to global stats for current user
page_id   | *OPTIONAL* URL of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | Start date range. Unix timestamp in milliseconds
end_time  | Current Time | End date range. Unix timestamp in milliseconds

# participant

## GET /participant

```shell
curl -X GET http://api.pinkpineapple.me/participant/www.example.com
```

```javascript
N/A
```

> Returns JSON of structure:

```json
{
    "participants": [
        {
            "participant_key": "abc123456789",
            "sessions": [
                {
                    "session_key": "aabbcc"
                },
                {
                    "session_key": "bbccdd"
                }  
            ]
        }
    ]
}
```

Gets participants and their list of sessions

### HTTP Request
`GET http://api.pinkpineapple.me/site_id`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | *OPTIONAL* Domain of a site.

# stats

## GET /stats

```shell
curl -X GET http://api.pinkpineapple.me/stats/www.example.com/www.example.com%2Fpage/Component%20One?start_date=12345&end_date=54321
```

```javascript
N/A
```

> Returns JSON of structure:

```json
{
    "total_sessions": 23,
    "total_fixations": 3000,
    "average_fixation_duration": 6,
    "average_time_to_first_fixation": 2,
    "sessions_with_fixation": 119,
    "total_clicks": 5534,
    "sessions_with_clicks": 4400
}
```

Of use in the "Dashboard" section.

### HTTP Request
`GET http://api.pinkpineapple.me/stats/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* URL of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

# charts

## GET /charts/session_time

```shell
curl -X GET http://api.pinkpineapple.me/charts/session_time/www.example.com/www.example.com%2Fpage/Component%20One?start_time=12345&end_time=54321
```

```javascript
N/A
```

> Reply of JSON:

```json
{
    "chart": [
        { "time": 1, "sessions": 226 },
        { "time": 2, "sessions": 431 },
        { "time": 3, "sessions": 542 },
        { "time": 4, "sessions": 074 },
        { "time": 5, "sessions": 193 }
    ]
}
```

Gets chart data for sessions versus time.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/session_time/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/clicks_time

```shell
curl -X GET http://api.pinkpineapple.me/charts/clicks_time/www.example.com
```

```javascript
N/A
```

> Reply of JSON:

```json
{
    "chart": [
        { "time": 1, "clicks": 226 },
        { "time": 2, "clicks": 431 },
        { "time": 3, "clicks": 542 },
        { "time": 4, "clicks": 074 },
        { "time": 5, "clicks": 193 }
    ]
}
```

Gets chart data for clicks versus time.

### HTTP Request
`GET http://api.pinkpineaple.me/charts/fixation_time/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/fixation_time

```shell
curl -X GET http://api.pinkpineapple.me/charts/fixation_time/www.example.com/www.example.com%2Fpage/Component%20One?start_time=12345&end_time=54321
```

```javascript
N/A
```

> Reply of JSON:

```json
{
    "chart": [
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
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/average_time_to_fixation

```shell
curl -X GET http://api.pinkpineapple.me/charts/average_time_to_fixation/www.example.com/www.example.com%2Fpage/Component%20One?start_time=12345&end_time=54321
```

```javascript
N/A
```

> Reply of JSON:

```json
{
    "chart": [
        { "time": 1, "average_time_to_fixation": 226 },
        { "time": 2, "average_time_to_fixation": 431 },
        { "time": 3, "average_time_to_fixation": 542 },
        { "time": 4, "average_time_to_fixation": 074 },
        { "time": 5, "average_time_to_fixation": 193 }
    ]
}
```

Gets chart data for average time to fixations versus time.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/average_time_to_fixation/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Haour, Day, Week, Month


## GET /charts/average_fixation_duration_time

```shell
curl -X GET http://api.pinkpineapple.me/charts/average_fixation_duration_time/www.example.com
```

```javascript
N/A
```

> Reply of JSON

```json
{
    "chart": [
        { "time": 1, "average_fixation_duration": 226 },
        { "time": 2, "average_fixation_duration": 431 },
        { "time": 3, "average_fixation_duration": 542 },
        { "time": 4, "average_fixation_duration": 074 },
        { "time": 5, "average_fixation_duration": 193 }
    ]
}

```

Gets chart data for average fixation duration versus time.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/average_fixation_duration_time/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/fixation_count

```shell
curl -X GET http://api.pinkpineapple.me/charts/fixation_count/www.example.com
```

```javascript
N/A
```

> Reply of JSON

```json
{
    "chart": [
        { "count": 1, "fixations": 32 },
        { "count": 3, "fixations": 76 },
        { "count": 4, "fixations": 2 },
        { "count": 7, "fixations": 8 },
        { "count": 9, "fixations": 3 }
    ]
}
```
For ring chart? Gets chart data for fixation counts.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/fixation_count/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key

## GET /charts/fixations_end_with_click
```shell
curl -X GET http://api.pinkpineapple.me/charts/fixations_end_with_click/www.example.com
```

```javascript
N/A
```

> Reply of JSON

```json
{
    "chart": [
        { "time": 1, "fixations_end_with_click": 226 },
        { "time": 2, "fixations_end_with_click": 626 },
        { "time": 3, "fixations_end_with_click": 100 },
        { "time": 4, "fixations_end_with_click": 694 },
        { "time": 5, "fixations_end_with_click": 138 }
    ]
}

```

Gets chart data for average fixation duration versus time.

### HTTP Request
`GET http://api.pinkpineapple.me/charts/fixations_end_with_click/:site_id/:page_id?/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | Domain of a site.
page_id   | *OPTIONAL* Url of a page in site.
component_id | *OPTIONAL* Name of a component in page.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
start_time| Epoch Time | *OPTIONAL* Start date range
end_time  | Current Time | *OPTIONAL* End date range
session_key | null | *OPTIONAL* Session Key
participant_key | null | *OPTIONAL* Participant Key
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

