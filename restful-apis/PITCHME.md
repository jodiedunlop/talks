

REST

## In this talk


- Meaning of REST
- documenting your api
- response codes and verbs
- nested resources
- rpc style
	- how to map actions to rest
- transformations
- cache headers; etags
- errors
- content type
	- json, xml, soap
- json format
	- plain json
	- json-ld - augment with additional properties
	- hydra (json-ld + hydra core)
	- json-api
	- hateoas (supplement with media links)
	- hal (links, embedded resources, )
	- collection+json (for collections: links, templates, queries, )
	- siren
- links and pagination
- establish your own standards
	- How are resource collections represented
	- how is a date encoded
	- how are dates accepted
	- how do you version
	- query params
	- filter params
	- ordering params

---

## Overview

The term RESTful has emerged for web APIs that use the native concepts and techniques of the HTTP protocol, and blend in other concepts.  HTTP and REST are precisely defined, but the term RESTful is less opinionated.

REST has gained popularity because:

- REST is pervasive; easy to implement in any language, many tooling
- REST provides a uniform interface
- REST adheres to client/server architecture (seperation of concerns)
- REST resources are cacheable

---

## REST is good

- Uniform interface
- Uses HTTP protocol: GET, POST, PUT, PATCH, DELETE ...
- stateless
- Developers can set their own standards
- Open to interpretation

---

## REST is bad

- Uniform interface
- Uses HTTP protocol: GET, POST, PUT, PATCH, DELETE ...
- Stateless
- Developers can set their own standards
- Open to interpretation

---

## Speaker Notes:

^ Uniform interface: Your models/entities don't always map nicely to a RESTful interface (more in this later)

^ HTTP Protocol: Sometimes your API doesn't awlays fall nicely into an HTTP verb.

^ Stateless: In many web applications we can use sessions and cookies to maintain short-lived data which allows us to avoid making extra queries.

^ Developers can set their own standards: most developers have a different idea of how REST api's should be implemented, which STATUS code is returned, how the HTTP verb (eg. PUT) is interpreted, how links should be embedded. etc.

^ Since RESTful is really just a philosophy on top of the core REST principals, there are not alot of strict rules surrounding what a RESTful API looks like.

---

# RESTful architecture

An API that implements RESTFul principals does the following:


- Uses HTTP methods: OPTIONS, GET, PUT, POST, and DELETE etc.
- Provides a base URL for resources eg. https://api.mydomain.com/v1/books
- Provides a  content/media type: Eg. application/json, atom

---


The REST standard stipulates the following architectural constraints:

- Statelessness
- Cacheability
- Layered system
- Uniform interface
	- resource identification
	- resource representation
	- self-describing (eg. content types)
	- hyperlinks

Many RESTful API's adhere to some of these constraints.

The _rest_ is up to you:

- How you should format your data.
- How you represent your entities.
- How you structure your URI's.
- What information you should include within a resource.
- Which cache headers you use.
- How meta information is represented.
- How fields are formatted.


### Speaker notes

What does it mean to be restful, or implement REST architecure.

It's a standard suggested by Roy Fielding in year 2000.

It dictates that the HTTP protocol verbs are used for requests, plus the following architectural constraints


The REST definition requires four constraints:

Statelessness:

- All of the information necessary to service the request is contained within the client request and the server response. It does not allow any stored server state to be maintained between requests.  This makes scaling much simpler.

Cacheability:

- Responses should define themselves as cacheable or not cacheable to improve scalability and performance

Layered system:

- Intermediate services can be used to help handle the request
- Load balancers, proxy services, security layers

Uniform interface:

- Resources can be identified eg. by a URI
- The data within a resource representation is enough for the client to manipulate the data
- The resource can provide a self-description such as sending `Content-Type` headers so the client knows how to parse the format
- Clients can discover related actions or resources by embedding links HATEOAS style

# HTTP VERBS

Straight from the HTTP specification:

## GET
The GET method requests a representation of the specified resource. Requests using GET should only retrieve data and should have no other effect. 

## HEAD
The HEAD method asks for a response identical to that of a GET request, but without the response body. This is useful for retrieving meta-information written in response headers, without having to transport the entire content.

### POST
The POST method requests that the server accept the entity enclosed in the request as a new subordinate of the web resource identified by the URI. 

### PUT
The PUT method requests that the enclosed entity be stored under the supplied URI. If the URI refers to an already existing resource, it is modified; if the URI does not point to an existing resource, then the server can create the resource with that URI.

`PUT /resources/1234` - Replace the resource "1234"`
`PUT /resources` - Replace the entire collection`


## PATCH
The PATCH method applies partial modifications to a resource. Usually results in a 200 or 201 response.


## DELETE
The DELETE method deletes the specified resource. Usually an empty content body is returned with a 204 HTTP STATUS.


## OPTIONS
The OPTIONS method returns the HTTP methods that the server supports for the specified URL. This can be used to check the functionality of a web server by requesting '*' instead of a specific resource.


### Talk notes:

From RFC 5789:
The difference between the PUT and PATCH requests is reflected in the way the server processes the enclosed entity to modify the resource identified by the Request-URI. In a PUT request, the enclosed entity is considered to be a modified version of the resource stored on the origin server, and the client is requesting that the stored version be replaced. With PATCH, however, the enclosed entity contains a set of instructions describing how a resource currently residing on the origin server should be modified to produce a new version. The PATCH method affects the resource identified by the Request-URI, and it also MAY have side effects on other resources; i.e., new resources may be created, or existing ones modified, by the application of a PATCH.




# HTTP Status codes

A minimal approach to HTTP status codes:

* 200 - Return this when it was successful



# Better HTTP Status codes

* 200 - OK: everything "good"
* 400 - BAD REQUEST: something bad with the client request
* 404 - NOT FOUND: missing resource
* 500 - INTERNAL SERVER ERROR: something bad on the server


# Better HTTP Status codes

## 200 – OK 
The request was successful (eg. when fetching a resource)

## 201 - CREATED
Created new resource

## 204 - NO CONTENT
The resource was successfully deleted, no response body.

## 304 - NOT MODIFIED
Resource has not changed (use cached data).

## 400 - BAD REQUEST
The request was invalid or cannot be served. 

## 401 - UNAUTHORIZED
The user is currently unauthorized and needs to authenticate. It might also be used for a blacklisted IP address.

## 403 - FORBIDDEN
The user may be performing an operation that requires authorisation, or they do not have the correct permissions.

## 404 - NOT FOUND
There is no resource behind the URI.

## 405 - METHOD NOT ALLOWED
When an HTTP method is being requested that isn't allowed for the authenticated user. Or the resource exists at the URI but the HTTP verb used isn't permitted for that resource.

## 415 - UNSUPPORTED MEDIA TYPE
If your API can respond in multiple formats (eg. JSON and XML), you might implement this status code when the client sends a different `Accept:` header than you expect.

## 422 - Unprocessable Entity
Use this to indicate validation errors from the request payload

## 429 - TOO MANY REQUESTS
Return this when a client reaches their rate limit.

## 500 - INTERNAL SERVER ERROR
Return this when something goes wrong on the server and you don't have a better status code to represent the problem

## 503 - SERVICE UNAVAILABLE
The server is currently unavailable (because it is overloaded or down for maintenance).

## 504 - GATEWAY TIMEOUT
Did not receive a timely response from the upstream server. You might implement this status code if your API depends on a third-party, and a timeout occurs while trying to perform the operation on the remote server.

### Speaker Notes:

^ If you are implementing more status codes than this and you don't have a great reason to, then you may be complicating your API more than is needed.




The exact error should be explained in the error payload. 





# Building a good RESTful API


- Start with the documentation: Swagger/OpenAPI, API Blueprint, RAML etc.
	
- Take a data-orientated approach to building your API

- Use JSON as your transport format for Requests and Responses

- Keep your JSON as simple as possible

- Use permanent, opaque, ids

- Provide links

- Use nouns not verbs for resource locations

-

## Individual Book Resource 

`GET /v1/books/xAbG22gQ`
```json
{
	"id": "xAbG22gQ",
	"name": "Some Book",
	"author": {
		"id": "Pv4gTTy9",
		"name": "Some Author"
	},
	"meta": {
		"links": {
			"self": "https://api.example.com/v1/books/xAbG22gQ",
			"author": "https://api.example.com/v1/authors/Pv4gTTy9"
		}
	}
}
```

## Collection of Book Resources

`GET /v1/books`
```json
{
	"data": [
		{
			"id": "xAbG22gQ",
			"name": "Some Book",
			"author": {
				"id": "Pv4gTTy9",
				"name": "Some Author"
			},
			"created_at": 
		}
	],
	"meta": {
		"links": {
			"self": "https://api.example.com/v1/books/xAbG22gQ",
			"author": "https://api.example.com/v1/authors/Pv4gTTy9"
		},
		"paging": {
		    "cursors": {
		      "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
		      "before": "NDMyNzQyODI3OTQw"
		    },
		    "previous": "https://api.example.com/v1/books?before=NDMyNzQyODI3OTQw"
		    "next": "https://api.example.com/v1/books?after=MTAxNTExOTQ1MjAwNzI5NDE="
	    }
	}
}

```


### Talk notes

Documentation:
- Take a documentation first approach
- Easier to change documentation, than change your API
- Use one of the document specifications: Swagger/OpenAPI, API Blueprint, RAML
- Determine what your resources are
- What's your authentication scheme
- Plan your endpoints
- We used to use API Blueprint
	- Markdown: Easier for a human to read in raw format 
	- More verbose
	- More copy and paste

- We prefer Swagger/OpenAPI
	- More structured approach
	- Easier to convert
	- Available in yaml/json flavours


Takes a data-orientated approach Data orientated approach: Model your API on representing the resource, rather than RPC functional style. The beauty of REST and a data-oriented approach is that using the API becomes very intuitive.  You can know where an individual resource lives, you can infer where the collection is located, and how to modify the resource.

Just use JSON: JSON is lightweight, has great support amongst programming languages, is much less verbose than XML, it's very readable, and maps well to your models and entities.  It comes in many flavours such as JSON-API and there are various extensions to make it more


### What to document

- Authentication
- Request and Response format
- Example error output
- API Errors
- HTTP Status Codes
- Resource endpoints





## Mapping Resources to Models

- Models don't necessarily map 1:1 with a resource
- 


To envelope or not to envelope

```json
{
	"data": { 
		"id": 1234,
		"name": "Pride and Prejudice"
	}

}
```

vs

{
	"id": 1234, 
	"name": "Pride and Prejudice"
}

- Links
- jsonp?
- 

#### JSONP

curl https://api.github.com?callback=foo

```
/**/foo({
  "meta": {
    "status": 200,
    "X-RateLimit-Limit": "5000",
    "X-RateLimit-Remaining": "4966",
    "X-RateLimit-Reset": "1372700873",
    "Link": [ // pagination headers and other links
      ["https://api.github.com?page=2", {"rel": "next"}]
    ]
  },
  "data": {
    // the data
  }
})
```


## Caching

- Etag header
- If-None-Match, If-Modified-Since, Last-Modified
- CORS


### Etag

When generating a response, 

- Include a HTTP header `ETag` 
- Containing a hash or checksum of the representation. 
- Etag only changes when the resource/representation changes.
- Client can send `If-None-Match: <etag>` to only receive content when there is a match
- 304 Not Modified can be returned for an etag match

Etags don't combine well with embedded resources.

### Last-Modfied

Last-Modified: This basically works like to ETag, except that it uses timestamps. The response header Last-Modified contains a timestamp in RFC 1123 format which is validated against If-Modified-Since. Note that the HTTP spec has had 3 different acceptable date formats and the server should be prepared to accept any one of them.


PHP Api Client
Resource extends ArrayObject

- Headers
- getHttpStatusCode()
- getPayload()
- getContentType()
- isJson()
- isOk()
- isError()
- isCreated()
- isModified()
- isClientError()
- isServerError()
- data()  -> ArrayObject
- __call() -> delegate to ArrayObject
- __get	-> delegate to ArrayObject
- __set -> delegate to ArrayObject


## What should you standardise?

Everything!

- Documentation
- Authentication
- Constants
- How are id's serialized
- What is the format of an id
- Dates
- Booleans
- Versioning
- Response envelope `data:`
- Individual Resources
- Collection Resources
- Pagination: Cursors, Links, Pages
- Errors
- How is real-time data transmitted
- snake_case or camelCase
- Whitespace - pretty print
- Links
- User-Agent




Speaker notes;

How are links defined, are they implemented as headers, or encoded in the payload
See: https://tools.ietf.org/html/rfc5988#page-6

### Speaker notes

snake_case or camelCase;

If you're working with JSON notation, it probably makes some sense to use camelCase. Personally, we use snake_case because we don't like the idea that case could effect the operation, and we think it's easier to read by human-parsers.

whitespace:

It's easier to debug, and look at when your responses are whitespace formatted.
With GZIP compression there will only be a few bytes difference.


## Security concerns

- SSL
- Token expiration
- limiting
- Obscuring id's
- Opaque pagination tokens
- Headers: HTSTS
- Input sanitization: Don't trust the user!
- Don't leak dumps/stack traces. Return an error message (use an environment flag)
- Errors

### SSL

- Simplifies authentication credentials (can be passed as basic auth)

By always using SSL, the authentication credentials can be simplified to a randomly generated access token that is delivered in the user name field of HTTP Basic Auth. The great thing about this is that it's completely browser explorable - the browser will just popup a prompt asking for credentials if it receives a 401 Unauthorized status code from the server.


## Authentication

- OAuth2
- Users can use the OAUTH2 web flow
- Applications can request basic access tokens from an OA

OAuth 2 should be used to provide secure token transfer to a third party. OAuth 2 uses Bearer tokens & also depends on SSL for its underlying transport encryption.


## Why transform

- Models don't always map 1:1 with a resource
- Leak information
- Changes to implementation don't break your API
- Some resources should be transformed differently under circumstances: search, as an include
- More control: eg. sparse field sets
- Easier to merge related entities


## Searching

- Sub-resource of a collection
- Can have a different format to collection/resource
A search is a sub-resource of a collection. As such, its results will have a different format than the resources and the collection itself. This allows us to add suggestions, corrections and information related to the search. 
Parameters are provided the same way as for a filter, through the query-string, but they are not necessarily exact values, and their syntax permits approximate matching.


## Link based

Headers


See https://developer.github.com/v3/#pagination

https://tools.ietf.org/html/rfc5988#page-6

Link: <https://api.github.com/user/repos?page=3&per_page=100>; rel="next", <https://
api.github.com/user/repos?page=50&per_page=100>; rel="last"

## Pagination

The right way to include pagination details today is using the Link header introduced by RFC 5988.

POST

PUT being indempotent

- Replace (or create) a resource
- 200 (OK) or 201 (Resource created)


PATCH
POST
GET
DELETE

## GET

- Calling GET must have NO side effects
- Responds with 200 status code
- Returns an individual resource or collection
- Is cacheable

### Speakers 

No Side effects:
The GET method is a safe method (or nullipotent), meaning that calling it produces no side-effects: retrieving or accessing a record does not change it.
This means, you should never implement an action, or fire a job or task from a GET call.

Is cacheable:
A call to GET for a resource that has not been modified should not change it's
Don't return any properties which could change 

### General

- Build your API for your consumers; frontend devs, customers, mobile apps
- Server should do the heavy listing
- Use SSL always - simplifies authentication and 
- Don't allow non-SSL, don't redirect non-ssl
- Version your API major (eg. URL) and minor (eg. HEADERS)
- Sparse field sets
- JSON only responses


#### Speaker notes

JSON only responses:

XML is verbose, larger payloads, not as similar to how data is modelled on the server side.


### Rate limiting

To prevent abuse, it is standard practice to add some sort of rate limiting to an API. RFC 6585 introduced a HTTP status code 429 Too Many Requests to accommodate this.

However, it can be very useful to notify the consumer of their limits before they actually hit it. This is an area that currently lacks standards but has a number of popular conventions using HTTP response headers.

At a minimum, include the following headers (using Twitter's naming conventions as headers typically don't have mid-word capitalization):

X-Rate-Limit-Limit - The number of allowed requests in the current period
X-Rate-Limit-Remaining - The number of remaining requests in the current period
X-Rate-Limit-Reset - The number of seconds left in the current period


```bash
curl -i https://api.github.com/users/octocat
HTTP/1.1 200 OK
Date: Mon, 01 Jul 2013 17:27:06 GMT
Status: 200 OK
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 56
X-RateLimit-Reset: 1372700873
```

403 Forbidden



### HATEOAS

Hypermedia As The Engine Of Application State
The client transitions through application states by selecting from the links within a representation.
Driven by driven by hypermedia, rather than out-of-band information.

TL;DR - Add links to your responses so the client knows where to go next.


### Library example

GET    /books
POST   /books
GET    /books/{book}
PUT    /books/{book}
PATCH  /books/{book}
DELETE /books/{book}
GET    /authors
POST   /authors
...
GET    /books/{book}/authors
GET    /users/{user}
GET    /users/{user}/favoriteBooks


/getBooks
/createNewBook
/checkOutBook/{bookId}
/returnBook/{bookId}
/addBookToFavorites
/addNewMemberToBookClub
/changeBookClubMeetingTime
/changeBookClubMeetingLocation
/removeBookClubMember


## Other things to consider

- Monitoring
- Logging
- Deployment
- Sandboxing
- Changelogs
- Correlation between your API's
- Testing your documentation against your server responses
- How you publish documentation
- Quickstart guide
- Client SDK's
- 

## Good apis
- github?
- facebook?
- twilio?


## Bad apis
- Facebook
- Twitter?


References:

http://transmission.vehikl.com/theres-a-model-hiding-in-your-rest-api/
https://savvyapps.com/blog/how-to-build-restful-api-mobile-app
http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#ssl
https://developers.facebook.com/docs/graph-api/using-graph-api/#paging
https://developer.github.com/v3/
https://stripe.com/docs/api
http://blog.restcase.com/5-basic-rest-api-design-guidelines/
https://en.m.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods
https://swaggerhub.com/blog/api-documentation/best-practices-in-api-documentation/
http://dev.bitly.com/authentication.html
https://sookocheff.com/post/api/on-choosing-a-hypermedia-format/
https://pages.apigee.com/rs/351-WXY-166/images/Web-design-the-missing-link-ebook-2016-11.pdf
https://swaggerhub.com/blog/api-documentation/best-practices-in-api-documentation/
https://classroom.udacity.com/courses/ud388/lessons/4592928861/concepts/52457425850923
https://slack.engineering/evolving-api-pagination-at-slack-1c1f644f8e12