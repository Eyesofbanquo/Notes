# Request

The most commont part of the `HTTP` library is the `Request` type.

Commonly used attributes:

~~~swift
public var method: Method
public var uri: URI
public var parameters: Node
public var headers: [HeaderKey: String]
public var body: Body
public var data: Content
~~~

## Method

The method property of type `Method` can have values like `GET`, `POST`, `PUT`, `PATCH`, `DELETE`.

## URI

The associated `URI` of the request. It can be used to access attributes about the `uri` the request was sent to.

given: `http://vapor.codes/example?query=hi#fragments-too`

~~~swift
let scheme = request.uri.scheme //http
let host = request.uri.host //vapor.codes

let path = request.uri.path //example
let query = request.uri.query //query=hi
let fragment = request.uri.fragment //fragments-too
~~~

## Route Parameters

The url parameters associated with the request

example: If we have a path registered to 

`hello/:name/age/:age` we can access those in our request like so:

~~~swift
let name = request.parameters["name"] //type = String?
let age = request.parameters["age"]?.int //Int?
~~~

To automatically throw `nil` or invalid variable, you can use the function `.extract()` on the `Request`.

~~~swift
let name = try request.parameters.extract("name") as String

let age = try request.parameters.extract("age") as Int
~~~

## Body

This is the body associated with the request and represents the general data payload.

For incoming requests, you'll often pull out the associated bytes like so:

`let rawBytes = request.body.bytes`

## Content

The `Content` represents the data passed through the request. Vapor provides a convenient `data` variable associated with the request taht prioritizes content in a consistent way

Example: say I receive a request to `http://vapor.codes?hello=world`.

~~~swift 
let world = request.data["hello"]?.string
~~~

This same code will work if I receive a JSON request

~~~ 
{
	"hello":"world"
}
~~~

## JSON

~~~swift
let json = request.json["hello"]
~~~

## Query Parameters

~~~swift
let query = request.query?["hello"] //String?
let name = request.query?["name"]?.string //String?
let age = request.query?["age"]?.int //Int?
let rating = request.query?["rating"]?.double //Double?
~~~

***

# Response

~~~swift
public let status: Status
public var headers: [HeaderKey: String]
public var body: Body
public var data: Content
~~~

## Status

The http status associated with the event, for example `.ok` == 200

##Content

The most common way to access content from a request:

~~~swift
let pokemonResponse = try drop.client.get("http://pokeapi.co/api/v2/pokemon/")

let names = pokemonResponse.data["results", "name"]?.array
~~~

## JSON

~~~swift
let json = request.response["hello"]
~~~

***

# Conforming to ResponseRepresentable

~~~swift
import Foundation

struct BlogPost {
	let id: String
	let content: String
	let createdAt: NSDate
}
~~~

Now make this `Struct` conform to `ResponseRepresentable`

~~~swift
import HTTP
import Foundation

extension BlogPost: ResponseRepresentable {
	/* The only required function */
	func makeResponse() throws -> Response {
		let json = try JSON(node: 
			[
				"id":id,
				"content":content,
				"created-at":createdAt.timeIntervalSince1970
			]
		)
		return try json.makeResponse()
	}
}
~~~

Now that we've modeled our BlogPost, we can return it directly in route handlers.

~~~swift
drop.post("post") { req in
	guard let content = request.data["content"] else { throw Error.missingContent }
	let post = Post(content: content)
	try post.save(to: database)
	return post
}
~~~

***

# Responder
The `Responder` is a simple protocol defining the behavior of objects that can accept a `Request` and return a `Response`. It is the core api endpoint that connects the `Droplet` to the `Server`

~~~swift
public protocol Responder {
	func respond(to request: Request) throws -> Response
}
~~~
***

# Client

The client provided by `HTTP` is used to make outgoing requests to remote servers

## QuickStart

~~~swift
let query = ...
let spotifyResponse = try drop.client.get("https://api.spotify.com/v1/search?type=artist&q=\(query)")
print(spotifyResponse)
~~~

## Clean Up

A cleaner way to create `spotifyResponse`

~~~swift
let spotifyResponse = try drop.client.get("http://api.spotify.com/v1/search", query: ["type":"artist", "q":query])
~~~

## Continued

In addition to `GET` requests, Vapor's client provides support for most common HTTP functions

~~~swift
let bytes = myJSON.makeBytes()
try drop.client.post("http://some-endpoint/json", headers: ["Auth":"Token my-auth-token"], body: .data(jsonBytes))
~~~

