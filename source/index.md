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

> Authorization is as such:

```shell
curl -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/
```

```javascript
N/A
```

Basic HTTP Authentication is used in the Instamelb API. 
Credentials are passed as username:password in all routes.
Server is stateless, we DO NOT maintain session information.
Hence, username:password must be passed at every route.

Most routes require authentication. Certain public routes do not.

<aside class="notice">
You must replace <code>USERNAME</code> and <code>PASSWORD</code> with your respective credentials.
</aside>

## POST /login

```shell
curl -X POST \
	 -u USERNAME:PASSWORD \
	 http://api.pinkpineapple.me/login
```

```java
N/A
```

> On successful logged in, the above command returns JSON structured like this:

```json
{
	"credentials_valid": true
}
```

> In event of failed logged in, we just return a 401 error

This endpoints authenticates a user's username and password then return the user the result of login.

### HTTP Request
`POST http://api.pinkpineapple.me/login`

# register

## POST /register

```shell
curl -H "Content-Type: application/json" -X POST \
    -d '{
            "username": "NEW_USERNAME",
            "email": "USER@NEW_EMAIL.COM",
            "password": "NEW_PASSWORD",
            "phone": "01234567898765",
            "address": "100 Bourke Street, 1000 Melbourne"
        }'\ 
    http://api.pinkpineapple.me/register
```

```java
N/A
```
> On successful registration, the above command returns JSON structured like this:

```json
{
    "registered": true
}
```

> In event of failed registration, we return:

```json
{
    "registered": false,
    "error": "Email already registered"
}
```

This endpoint registers a new user in the system.

### HTTP Request
`POST http://api.pinkpineapple.me/register`

### Body Parameters

Parameter | Description
--------- | -----------
username | Username of account.
email | Email to be registered.
password | Password of account
phone | *OPTIONAL* Phone Number
address | *OPTIONAL* Address

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

# script

## GET /script

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/script/www.example.com
```

```javascript
N/A
```

> Get JSON object of structure:

```json
{
    "script": "<script type='text/javascript' src='js/xLabsAnalytics.js'>\
        </script><script type='text/javascript'>xa.start('API_KEY');</script>"
}
```

Get snippet for embedded xLabs Analytics script.


### HTTP Request
`GET http://api.pinkpineaple.me/script/:site_id`

### Body Parameters

Parameter | Description
--------- | -----------
site_id | Domain of a site

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
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/clicks/www.example.com/www.example/com%2Fpage?start_time=0&end_time=123456789123456789
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
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/site
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
            "token": "asdsacdsac",
            "extension_enabled": true,
            "categories": [],
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
            "token": "asdsacdsac",
            "extension_enabled": true,
            "categories": ["Math"],
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
    -u USERNAME:PASSWORD\ 
    -d '{
        "site_url": "www.example.com",
        "categories": ["sports", "news"]
    }' http://api.pinkpineapple.me/site
```

```javascript
N/A
```

> JSON Site object for created website is returned

```json
{
    "site_url": "www.example.com",
    "categories": ["sports", "news"],
    "token": "1235-2134-12lkdmlk-23e",
    "extension_enabled": true,
    "pages": []   
}
```

This route add a website to database.

### HTTP Request
`POST http://api.pinkpinepple.me/site`

### Body Parameters
Parameter | Description
--------- | -----------
site_url | Website domain
categories | Array of strings of categories for site.

## DELETE /site

```shell
curl -X DELETE -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/site/www.example.com
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
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/stats/www.example.com/www.example/com%2Fpage?start_time=0&end_time=123456789123456789
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
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/participant/www.example.com
```

```javascript
N/A
```

> Returns JSON of structure:

```json
{
    "participants": {
        "participant_key1": {
            "sessions": [
            	"session_key1",
				"session_key2"
            ],
            "session_count": 2
        },
        "participant_key2": {
            "sessions": [
            	"session_key3",
				"session_key4",
				"session_key5"
            ],
            "session_count": 3
        }
    }
}
```

Gets participants and their list of sessions


### HTTP Request
`GET http://api.pinkpineapple.me/site_id`

### Url Parameters
Parameter | Description
--------- | -----------
site_id   | *OPTIONAL* Domain of a site.


## GET /participant/component_fixation

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/participant/component_fixation/www.example.com?participant_key=123456
```

```javascript
N/A
```

> Returns JSON object of structure:

```json
{
   "participants":
       {
           "participant_key1":
               {
                   "page1": {
                       "Component1": {
                            "count": 20,
                            "classes": ["Class1"]
                        },
                       "Component2": {
                            "count": 30,
                            "classes": ["Class2", "Class3"]
                        }
                   }
               }
       }
} 
```

Component fixation for a participant

### HTTP Request
`GET /participant/component_fixation/:site_id/:page_id?`

### Url Parameters
Parameter | Description
--------- | -----------
site_id | Domain of a site.
page_id | *OPTIONAL* Page in a site.

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
participant_id | null | Participant key
start_time| Epoch Time | Start date range. Unix timestamp in milliseconds
end_time  | Current Time | End date range. Unix timestamp in milliseconds


## GET /participant/punch_card

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/participant/punch_card/www.example.com
```

```javascript
N/A
```

> Returns JSON of structure:

```json
{
    "participants":
        {
            "Sunday": {"0": 65, "3": 34, "23": 19 },
            "Saturday": {"0": 65, "3": 34, "23": 19 }
        }
}
```

Daily punch cards. Like in GitHub graphs.

### HTTP Request
`GET http://api.pinkpineapple.me/participant/punch_card/:site_id`

### Url Parameters
Parameter | Description
--------- | -----------
site_id | Domain of website

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
participant_id | null | Participant key.
start_time | Epoch time | Unix time. Start time.
end_time | Current Time | Unix time. End time.

# stats

## GET /stats

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/stats/www.example.com/www.example.com%2Fpage/Component%20One?start_date=12345&end_date=54321
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
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

# charts

## GET /charts/session_time

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/session_time/www.example.com/www.example.com%2Fpage/Component%20One?start_time=12345&end_time=54321
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
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/clicks_time

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/clicks_time/www.example.com
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
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/fixation_time

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/fixation_time/www.example.com/www.example.com%2Fpage/Component%20One?start_time=12345&end_time=54321
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
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/average_time_to_fixation

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/average_time_to_fixation/www.example.com/www.example.com%2Fpage/Component%20One?start_time=12345&end_time=54321
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
period | Day | *OPTIONAL* Time period. Haour, Day, Week, Month


## GET /charts/average_fixation_duration_time

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/average_fixation_duration_time/www.example.com
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
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

## GET /charts/fixation_count

```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/fixation_count/www.example.com
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

## GET /charts/fixations_end_with_click
```shell
curl -X GET -u USERNAME:PASSWORD\ 
    http://api.pinkpineapple.me/charts/fixations_end_with_click/www.example.com
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
period | Day | *OPTIONAL* Time period. Hour, Day, Week, Month

# component_path

## GET /component_path
```shell
curl -X GET -u USERNAME:PASSWORD \
    http://api.pinkpineapple.me/component_path/www.example.com/www.example.com%2Fpage/CompOne
```

```javascript
N/A
```

> Sucessful request of component path returns JSON of:

```json
{
    "component_id": null,
    "jump_count": -1,
    "total_jumps": 666,
    "depth": 2,
    "jump_to": [
        {
            "component_id": "Comp One",
            "jump_count": 111,
            "total_jumps": 100,
            "depth": 1,
            "jump_to": [
                {
                    "component_id": "Comp Two",
                    "jump_count": 50,
                    "total_jumps": -1,
                    "depth": 0,
                    "jump_to": []
                },
                {
                    "component_id": "Comp Three",
                    "jump_count": 50,
                    "total_jumps": -1,
                    "depth": 0,
                    "jump_to": []
                }
            ]
        },
        {
            "component_id": "Comp Two",
            "jump_count": 222,
            "total_jumps": -1,
            "depth": 0
        },
        {
            "component_id": "Comp Three",
            "jump_count": 333,
            "total_jumps": -1,
            "depth": 0
        }
    ]
}
```

Gets component pass info for a component in a page. Unspecified component_id returns tally results for the entire page instead.

### HTTP Request
`GET http://api.pinkpineapple.me/component_path/:website_id/:page_id/:component_id?`

### Url Parameters
Parameter | Description
--------- | -----------
website_id | Domain of a website
page_id | Url of a page
component_id | *OPTIONAL* Component id

### Query Parameters
Parameter | Default | Description
--------- | ------- | -----------
depth | 1 | Depth of component pass
