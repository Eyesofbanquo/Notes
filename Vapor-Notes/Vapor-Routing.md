# Basic Routing

## Register

The most basic route includes a **method**, **path**, and **closure**

~~~swift
drop.get("welcome") { request in
	return "Hello"
}
~~~

The standard HTTP methods are available including `get`, `post`, `put`, `patch`, `delegte`, and `options`.

## Nesting

To nest paths (addding `/`s in the URL)

~~~swift
drop.get("foo", "bar", "baz") { request in
	return "You requested /foo/bar/baz"
}
~~~

## Alternate

An alternate syntax that accepts a `Method` (.GET, .POST, etc) as the first parameter is also available.

~~~swift
drop.add(.trace, "welcome") { request in
	return "Hello"
}
~~~

## Request

Each route closure is given a single `Request`. This contains all the data associated wtih the request that led to your route closure being called.

## Response Representable

A route closure can return in three ways:

* `Response`
*  `ResponseRepresentable`
*  `throw`

### Response

A custom `Response` can be returned

~~~swift
drop.get("vapor") { request in
	return Response(redirect: "http://vapor.codes")
}
~~~

### Response Representable

Types in Vapor that conform to this protocol by default:

* String
* Int
* JSON
* Model

~~~ swift
drop.get("json") { request in
	return try JSON(node: [
		"number":123,
		"text":"unicorns",
		"bool":false
	])
}
~~~

# Route Groups

Grouping routes together makes it easy to add common prefixes, middleware, or hosts to multiple routes

Route groups have 2 different forms: Group and Grouped.

### Group

Group takes a closure taht is passed a `GroupBuilder`

~~~swift
drop.group("v1") { v1 in
	v1.get("users") { request in
		//get the users
	}
	v1.post("users") { request in
	}
}
~~~

###Grouped
Grouped returns a GroupBuilder that you can pass around.

~~~swift
let v1 = drop.grouped("v1")
v1.get("users") { request in
	//get the users
}
~~~