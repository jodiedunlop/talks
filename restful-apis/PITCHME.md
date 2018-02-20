# RESTful API's

## About me

- Senior backend developer at REX Software
- API development infrastructure issues
- Mostly Laravel

## Overview

The term RESTful has emerged for web APIs that use the native concepts and 
techniques of the HTTP protocol which REST defines, and blend in other concepts.  

HTTP and REST are precisely defined, but the term RESTful is less opinionated.

---

## What does it mean to be RESTful

A RESTful API does the following:

- Uses HTTP methods: OPTIONS, GET, PUT, POST, and DELETE etc.
- Provides a base URL for resources `https://api.mydomain.com/v1/books`
- Provides a content/media type: `application/json`, `atom` etc.
- Implements the REST standard ...

---

## REST

REST is an architectural style, not a protocol:

The REST standard stipulates the following architectural constraints:

- Statelessness
- Cacheability
- Layered system
- Uniform interface


Note:
What does it mean to be restful, or implement REST standards.

REST is a standard proposed by Roy Fielding in year 2000.

It dictates that the HTTP protocol verbs are used for requests, plus the 
following architectural constraints:

Statelessness:

 All of the information necessary to service the request is contained within the
  client request and the server response. It does not allow any stored server state to be maintained between requests.  This makes scaling much simpler.

Cacheability:

 Responses should define themselves as cacheable or not cacheable to improve 
 scalability and performance

Layered system:

 Intermediate services can be used to help handle the request
 Load balancers, proxy services, security layers

Uniform interface:

 resource identification
 resource representation
 self-describing (eg. content types)
 hyperlinks

Resource identification:
 Resource representation: Resources can be identified eg. by a URI
 The data within a resource representation is enough for the client to manipulate
  the data
 The resource can provide a self-description such as sending `Content-Type` 
  headers so the client knows how to parse the format
 Clients can discover related actions or resources by embedding links HATEOAS
  style

RESTful API's often only adhere to some of these constraints.

---

## Plus your own standard

The _rest_ is up to you:

- How you should format/encode your data
- How properties and meta information for your resource is represented
- How you structure your URI's
- Which cache headers you use

---

## Why REST

REST has gained popularity because:

- REST is pervasive; easy to implement in any language, many tooling
- REST provides a uniform interface
- REST adheres to client/server architecture (seperation of concerns)
- REST resources are cacheable

---

## REST is good

- Uniform interface
- Uses HTTP protocol: GET, POST, PUT, PATCH, DELETE ...
- Stateless
- Open to interpretation

Note:

^ HTTP protocol, can easily be inspected in a browser

^ State can introduce more complexity and makes scaling more difficult

^ Open to interpretation; Developers can set their own standards - REST has few architectural constraints the remaining choices are up to the developer

^ The definition of REST can be a bit vague or interpreted differently

---

## REST is bad

- Uniform interface
- Uses HTTP protocol: GET, POST, PUT, PATCH, DELETE ...
- Stateless
- Open to interpretation

Note:

^ Uniform interface: Your models/entities don't always map nicely to a RESTful interface (more in this later)

^ HTTP Protocol: Sometimes your API doesn't awlays fall nicely into an HTTP verb.

^ Stateless: In many web applications we can use sessions and cookies to maintain short-lived data which allows us to avoid making extra queries.

^ Developers can set their own standards: most developers have a different idea of how REST api's should be implemented, which STATUS code is returned, how the HTTP verb (eg. PUT) is interpreted, how links should be embedded. etc.

^ Since RESTful is really just a philosophy on top of the core REST principals, there are not alot of strict rules surrounding what a RESTful API looks like.

---

## REST vs SOAP

SOAP used to be the defacto standard for web API's. 

- XML is verbose, and requires more bandwidth
- REST can implement many data formats
- SOAP tooling had obscure bugs in many platforms
- SOAP performs operations, REST is resource/data driven
- SOAP can't be cached, REST can

---



### Good REST API

Good RESTful API's are:

- Well documented
- Using the HTTP verbs with a data-orientated approach
- Using nouns not verbs for resource locations
- Express their results via HTTP status codes
- Use JSON for requests/response payloads

---

### Good REST API

Good RESTful API's also:

- Provide permanent id's that do not change
- Include links to other resources
- Easy to consume by frontend devs, customers, mobile apps
- Provides informative errors
- Implement a versioning scheme

---

### Good REST API

Good RESTful API's are:

- Let the server do the heavy-lifting
- Implement rate limiting
- Are terminated via SSL

Note:

^ Well documented using Swagger OpenAPI, API blueprint or some other standard

^ Use SSL always - simplifies authentication and ensures data is not eavesdropped.
That means not allowing non-SSL, don't redirect non-ssl

^ Use JSON () unless there is a great reason not to - XML is verbose, larger payloads, doesn't always map nicely to your data, not as intuitive 

^ Version your API major (eg. URL) and minor (eg. HEADERS)

^ Server should do the heavy listing

---

### Nice things

- Sparse field sets
- Embedded resources
- Links beyond pagination
- Rate limiting

---


## HTTP VERBS

---

### GET
The GET method requests a representation of the specified resource. Requests using GET should only retrieve data and should have no other effect. 

- Calling GET must have NO side effects
- Responds with 200 status code
- Returns an individual resource or collection
- A `GET` request should be cacheable

Note:

^ No Side effects:
The GET method is a safe method (or nullipotent), meaning that calling it produces no side-effects: retrieving or accessing a record does not change it.
This means, you should never implement an action, or fire a job or task from a GET call.

^ Is cacheable:
A call to GET for a resource that has not been modified should not change it's
Don't return any properties which could change 

---

### POST
The POST method requests that the server accept the entity enclosed in the request
as a new instance of the web resource identified by the URI. 

---

### PUT
The PUT method requests that the enclosed entity be stored under the supplied URI. 
If the URI refers to an already existing resource, it is modified;
if the URI does not point to an existing resource,  then the server can create
 the resource with that URI.

---

### Examples

Replace the resource "1234" using `PUT`:

```http
PUT /resources/1234
```

Replace the entire collection `PUT`:

```http
PUT /resources
```

---

## HEAD
The HEAD method asks for a response identical to that of a GET request, but without the response body. This is useful for retrieving meta-information written in response headers, without having to transport the entire content.

---

## PATCH
The PATCH method applies partial modifications to a resource. Usually results in a 200 or 201 response.

Note:

^ From RFC 5789:
The difference between the PUT and PATCH requests is reflected in the way the server processes the enclosed entity to modify the resource identified by the Request-URI. In a PUT request, the enclosed entity is considered to be a modified version of the resource stored on the origin server, and the client is requesting that the stored version be replaced. With PATCH, however, the enclosed entity contains a set of instructions describing how a resource currently residing on the origin server should be modified to produce a new version. The PATCH method affects the resource identified by the Request-URI, and it also MAY have side effects on other resources; i.e., new resources may be created, or existing ones modified, by the application of a PATCH.

---

## DELETE
The DELETE method deletes the specified resource. Usually an empty content body is returned with a 204 HTTP STATUS.

---


## OPTIONS
The OPTIONS method returns the HTTP methods that the server supports for the specified URL. This can be used to check the functionality of a web server by requesting \* instead of a specific resource.

---

## Use HTTP Status codes

* 200 - OK: everything "good"
* 400 - BAD REQUEST: something bad with the client request
* 404 - NOT FOUND: missing resource
* 500 - INTERNAL SERVER ERROR: something bad on the server

---

## Moarr HTTP Status codes

### 200 – OK 
The request was successful (eg. when fetching a resource)

### 201 - CREATED
Created new resource

### 204 - NO CONTENT
The resource was successfully deleted, no response body.

## Moarr HTTP Status codes

### 304 - NOT MODIFIED
Resource has not changed (use cached data).

### 400 - BAD REQUEST
The request was invalid or cannot be served. 

### 401 - UNAUTHORIZED
The user is currently unauthorized and needs to authenticate. It might also be used for a blacklisted IP address.

## Moarr HTTP Status codes

### 403 - FORBIDDEN
The user may be performing an operation that requires authorisation, or they do not have the correct permissions.

### 404 - NOT FOUND
There is no resource behind the URI.

### 405 - METHOD NOT ALLOWED
When an HTTP method is being requested that isn't allowed for the authenticated user. Or the resource exists at the URI but the HTTP verb used isn't permitted for that resource.

## Moarr HTTP Status codes

### 415 - UNSUPPORTED MEDIA TYPE
If your API can respond in multiple formats (eg. JSON and XML), you might implement this status code when the client sends a different `Accept:` header than you expect.

### 422 - Unprocessable Entity
Use this to indicate validation errors from the request payload

### 429 - TOO MANY REQUESTS
Return this when a client reaches their rate limit.

## Moarr HTTP Status codes

### 500 - INTERNAL SERVER ERROR
Return this when something goes wrong on the server and you don't have a better status code to represent the problem

### 503 - SERVICE UNAVAILABLE
The server is currently unavailable (because it is overloaded or down for maintenance).

### 504 - GATEWAY TIMEOUT
Did not receive a timely response from the upstream server. You might implement this status code if your API depends on a third-party, and a timeout occurs while trying to perform the operation on the remote server.

Note:

^ If you are implementing more status codes than this and you don't have a great reason to, then you may be complicating your API more than is needed.

^ The exact error should be explained in the error payload. 

---

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

---

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
			"created_at":  "2018-01-01 10:10:10"
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
		    "previous": "https://api.example.com/v1/books?before=NDMyNzQyODI3OTQw",
		    "next": "https://api.example.com/v1/books?after=MTAxNTExOTQ1MjAwNzI5NDE="
	    }
	}
}
```

---

## What should you standardise?

Everything!

- Documentation
- Authentication
- Versioning
- Types
- Transformations
- Links
- Errors
- Pagination
- Compression


Note:

^ Documentation - Beyond which spec Swagger OAS, Api Blueprint, RAML,  what sections do you include. 

^ Links - embedding links to other resources, do you use HATEOAS, are they implemented as headers like Github

^ Types - ids, constants, dates, booleans, phone numbers, addresses, currency

^ Transformations - how you represent your resources. Envelopes etc. snake_case, camelCase
what about whitespace is your output prettified (not much byte difference once gzipped)

^ Errors - do you always error-out early and only return one error, do you provide an array

^ Pagination -  do you use cursors, or paginate, how is it represented, headers, links

---

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

### Transforming your data

- Models don't always map 1:1 with a resource
- Leak information
- Changes to implementation don't break your API
- Some resources should be transformed differently under circumstances: search, as an include
- More control: eg. sparse field sets
- Easier to merge related entities

## REST data formats


- Plain old json
- HATEOAS
- JSON-LD
- Hydra (JSON-LD + Hydra Core Vocab)
- JSON API
- collection+json (everything's a collection!)
- siren


Note:

^ REST/RESTful doesn't force the data format you have to use, or even that you need to use JSON.

^ These are the most popular data formats or extensions for JSON

---

### HATEOAS

Hypermedia As The Engine Of Application State

- A constraint/extension of REST architecutre.
- Hypermedia links are the focus of HATEOAS.
- The client transitions through application states by selecting from the links within a representation.
- Driven by hypermedia, rather than out-of-band information.

TL;DR - Add links to your responses so the consumer knows where to go next.

### HAL

- Links (to URIs)
- Embedded Resources (i.e. other resources contained within them)
- State (your bog standard JSON or XML data)


---

### JSON-API

---

### SIREN

---

### Plain old JSON

- We prefer to use plain-old-json
- We implement our own standards and document them well
- We find some of the other data formats more
- We will most likely implement the HATEOAS extension for our

---

## Building a RESTful application

Let's build a cryptocurrency exchange (note: don't do this!):

- Coins  (BTC, Garlicoin, Dogecoin)
- Markets (coin->coin exchange)
- Buys/Bids
- Asks/Sells
- Users

Note:

^ There are several coins listed on a cryptocurrency exchange. In our case we'll have 3

^ There are markets for each coin which define what the coin can be exchanged for.  For example you might only be able to
buy and sell BTC for USD currency.

^ Users can place both buy orders and sells for a particular coin market.


---

## Crypto Currency Features

Our MVP will have the following features:

- Get a list of coins
- Create a new coin
- Update an existing coin
- Lookup an individual coin
- Delete a coin
- View available markets for a coin
- Buy a coin
- Sell a coin

Note:

^ In our MVP any authenticated user will be able to perform any of the functions.

---

## RPC Style

An RPC style API for our MVP features might look like this:

- /getCoins
- /createNewCoin
- /updateExistingCoin?coinId={coinId}
- /getCoin?coinId={coinId}
- /getCoinMarkets?coinId={coinId}
- /sellCoin?marketId={marketId}
- /buyCoin?marketId={marketId}

They might all just be POST methods.

## RESTful style

Resource based URL's:

- GET    /coins
- POST   /coins
- GET    /coins/{coinId}
- PATCH  /coins/{coinId}
- DELETE /coins/{coinId}
- GET    /coins/{coinId}/markets
- POST   /coins/{coinId}/sell
- POST   /coins/{coinId}/buy

Note:

^ So our users can list and retrieve coins. They can update and delete coins. They 
can list the available markets for a coin.  And they can even submit a Buy or a SELL order.
Cool.

^ This is a pretty good first stab at a RESTful API, even though there's a couple of problems.

^ The good part is that we've made the resource data-centric

^ We're supporting only supporting a `PATCH` operation because our updates are going to be partial modifications

^ `PUT` operations should be idempotent 

## Refining our API

- POST   /coins/{coinId}/sell
- POST   /coins/{coinId}/buy

^ When we look at these two requests, we can see a problem. If your RESTful endpoints contains verbs (ie. actions)
you probably need to rethink.

^ There is an opportunity here to turn these actions into resources.

## Refining our API

### Good

- POST   /coins/{coinId}/sell-orders
- POST   /coins/{coinId}/buy-orders

Note:

^ So here we create two new resources "sell-orders" and "buy-orders", which are nested
under 

### Better

- POST   /coins/{coinId}/orders   `{ "type_id": "sell", "market_id": ... }` 
- POST   /coins/{coinId}/orders

Note:

^ What we need to do is turn those actions into resources.  So we can have
a new resource nested under a coin; sell-orders and buy-orders

^ Since our sell and buy orders share the same semantics, we might as well
just call them orders, and include the type in the payload.

^ Now we can even introduce additional methods, such as the ability to DELETE an order

## Refining our API

### Even better

- GET       /markets
- GET       /markets/{marketId}
- POST      /markets/{marketId}/orders
- GET       /markets/{marketId}/orders/{orderId}
- DELETE    /markets/{marketId}/orders/{orderId}

Note:

^ Since you're buying and selling a market it makes sense to introduce a new top-level
market resource.

^ We can now extend our API to include the ability to place buy and sell orders for a market
and also get market information

## GET /v1/coins


```json
{
    "data": [
      {
        "id": "33eb4782-15fe-11e8-b642-0ed5f89f718b",
        "name": "Bitcoin",
        "ticker": "BTC",
        "created_at": "2017-01-02 16:17:20"
      },
      {
         "id": "bc9f54d0-1606-11e8-b642-0ed5f89f718b",
         "name": "Garlicoin",
         "ticker": "GRLC",
         "created_at": "2017-01-02 16:17:20"
      },
      {
         "id": "c4ca952a-1606-11e8-b642-0ed5f89f718b",
         "name": "Dogecoin",
         "ticker": "DOGE",
         "created_at": "2017-01-02 16:17:20"
      }
    ]
}
```


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

---

## Good RESTful apis

- Github
- Twilio

---

## Bad apis

- Facebook
- Twitter

---

## Thanks


Note:

^ http://transmission.vehikl.com/theres-a-model-hiding-in-your-rest-api/

^ https://savvyapps.com/blog/how-to-build-restful-api-mobile-app

^ http://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api#ssl

^ https://developers.facebook.com/docs/graph-api/using-graph-api/#paging

^ https://developer.github.com/v3/

^ https://stripe.com/docs/api

^ http://blog.restcase.com/5-basic-rest-api-design-guidelines/

^ https://en.m.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods

^ https://swaggerhub.com/blog/api-documentation/best-practices-in-api-documentation/

^ http://dev.bitly.com/authentication.html

^ https://sookocheff.com/post/api/on-choosing-a-hypermedia-format/

^ https://pages.apigee.com/rs/351-WXY-166/images/Web-design-the-missing-link-ebook-2016-11.pdf

^ https://swaggerhub.com/blog/api-documentation/best-practices-in-api-documentation/

^ https://classroom.udacity.com/courses/ud388/lessons/4592928861/concepts/52457425850923

^ https://slack.engineering/evolving-api-pagination-at-slack-1c1f644f8e12

^ https://www.upwork.com/hiring/development/soap-vs-rest-comparing-two-apis/

^ http://stateless.co/hal_specification.html

^ http://jsonapi.org/