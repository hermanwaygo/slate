# Errors

> The standard error response is structured like this:

```json
{
	"code": 418,
	"message": "error message",
	"fields": ["field1", "field2"]
}
```

The `fields` field will only be set if there is an error related to a specific field in the request, otherwise it may be left out. The `message` field will give a human-readable description of the error.

The error code will match the HTTP response code, and possible codes are:

Error Code | Meaning
---------- | -------
400 | Bad Request
401 | Unauthorized
403 | Forbidden
404 | Not Found
418 | I'm a teapot
429 | Too Many Requests
500 | Internal Server Error
503 | Service Unavailable
