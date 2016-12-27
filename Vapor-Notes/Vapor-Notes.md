## Disable Cache

You'd want to disable the cache so that you won't have to build every time to see a new change. Instead of building, all you'd need to do is refresh the page.

~~~swift
(drop.view as? LeafRenderer)?.stem.cache = nil
~~~

***

## Page redirection

~~~swift
Response(redirect: "path you want to redirect to")
~~~

***

## Database lookup from route

Remember, all routes are of this format:

~~~swift
func routeName(request:Request) throws -> RsponseRepresentable { }
~~~

With a route like the following:

~~~swift
func routeName(request:Request, acronym:Acronym) throws -> ResponseRepresentable {} 
~~~

Vapor will automatically look up the `id` of the `Acronym` that was passed to this endpoint.

***

**Store**`Droplet` **as a property on each**`Controller`.

***

## Import / Export

In order to have one template be substituted into another as an import, you must **extend** the container template in the sub template. You then **export** the parts of the HTML that you want to add into the container.

**master.leaf**

~~~html
<!DOCTYPE>
<html lang="en">
		<title>
			#import("title")
		</title>
		
		<body>
			#import("body")
		</body>
</html>

~~~

**other.leaf**

~~~html
#extend("master")

#export("head") {
	#embed("title")
}

#export("body") {
	<h1>Hello, World!</h1>
}
~~~

***


## If-Else Leaf

~~~html
#if(booleanCheck) {
	<p>Do something</p>
	} ##else() {
		<p> Do something else </p>
	}
}
~~~

## Nodes

Make sure to use the function `.makeNode()` on properties before sending them to `View`s.

## Grouped

~~~swift
let basic = drop.grouped("basic")
basic.get("endpoint", handler: handler)
~~~

Now all routes are prefixed with the word "basic"