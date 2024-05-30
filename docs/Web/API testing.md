---
tags:
  - web
  - api
---
## Find Documentation

Some common documentation endpoints:
```
/api
/api/v1
/swagger/
/api/swagger/
/api/swagger/v1
/openapi.json
```

Complete wordlist [here](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/swagger.txt)

OpenAPI can be parsed by Burp using *OpenAPI Parser* extension.

## Identify HTTP methods

With `OPTIONS` method **and** by fuzzing:
```
GET
HEAD
POST
PUT
DELETE
TRACE
CONNECT
PATCH
COPY
MOVE
```

Larger list [here](https://raw.githubusercontent.com/OWASP/AppSec-Browser-Bundle/master/utilities/wfuzz/wordlist/fuzzdb/attack-payloads/http-protocol/http-protocol-methods.txt)

## Identify Content-Types

Either manually or with *Content type converter* extension.

Big wordlist [here](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/web-all-content-types.txt)

## Fuzz for Hidden Endpoints

Either by deduction, for instance if there is a `PUT /api/user/update` endpoint there might be a `POST /api/user/add`.

Or by fuzzing using common API naming convention wordlists such as [this one](https://raw.githubusercontent.com/danielmiessler/SecLists/master/Discovery/Web-Content/api/actions.txt).

More relevant wordlists [here](https://github.com/danielmiessler/SecLists/tree/master/Discovery/Web-Content/api)

## Find Hidden Parameters

Fuzz using a wordlist of common parameters, adding relevant names from the application.

Or use the *Param Miner* extension, which guesses relevant names based on information in the scope.

## Check for Mass Alignment

Also known as auto-binding, it occurs when the request parameters are bound to an internal object.

Identify hidden parameters, for instance in a `PATCH /api/users/` request like:
```json
{
	"username": "wiener",
	"email": "wiener@example.com",
}
```

If the object has a internal `isAdmin` property in the backend, we could craft a `PATCH /api/users/` request like:
```json
{
	"username": "wiener",
	"email": "wiener@example.com",
	"isAdmin": true,
}
```

## Test for Server-Side Parameter Pollution

In addition to techniques below, the *Burp Scanner* can partially detect suspicious input transformations.

The *Backslash Powered Scanner* extension can also identify server-side parameter pollution.
### In the Query Strings

Try to append a URL-encoded **#** to the query string and observe the behaviour. This has for potential effect to strip the rest of the query on the server's side.

Using a URL-encoded **&** could have for effect to add a second parameter on the server's side.

If a second parameter can be injected, try and add a second **valid** parameter. Fuzzing can also be a solution, for instance with Burp's *Server-side variable names* wordlist.

Try to override an existing parameter by adding a second parameter with the same name but a different value and observe the response:
- PHP parses the last parameter only
- ASP.NET combines both parameters
- Node.js / express parses the first parameter only

### In REST Paths

URL-encoded special characters can be used to trigger errors or abuse a path traversal on the server's side.

For instance, a request made to the server like:
```http
GET /edit_profile.php?name=peter
```

Will result in this server-side request:
```http
GET /api/private/users/peter
```

And can be abused by appending a URL-encoded path traversal payload, for instance `peter%2f..%2fadmin` which would give:
```http
GET /api/private/users/peter/../admin
```

### In Structured Data Formats

Same principle as previous ones but adapted for JSON for instance:
```http
POST /myaccount name=peter","access_level":"administrator
```

Would result in:
```http
PATCH /users/7312/update {name="peter","access_level":"administrator"}
```