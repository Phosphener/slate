# Errors

The xLabs Analytics API uses the following error codes:

Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks. Check your JSON object.
401 | Unauthorized -- Your API key is wrong. Authentication error.
403 | Forbidden -- Not for your eyes.
404 | Not Found -- Route probably doesn't even exist. Check URL Parameters.
406 | Not Acceptable -- You requested a format that isn't JSON
429 | Too Many Requests -- You're requesting too many kittens! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.
