#Driver
***

To use Fluent with Vapor you will need to import Providers.

## Creating a Driver

~~~ swift
public protocol Driver {
	var idKey: String { get }
	func query<T: Entity>(_ query: Query<T>) throws -> Node
	func scheme(_ schema: Schema) throws
	func raw(_ raw:String, _ values: [Node]) throws -> Node
}
~~~


# Model
***

`Model` is the base protocol for any of your application's models.

## Example

Creating a simple `User` model.

~~~ swift
final class User {
	var name: String
	
	init(name: String) {
		self.name = name
	}
}
~~~

To make this usable in Fluent you must first conform to `Model`

~~~swift 
import Vapor
import Fluent
~~~

Then add the conformance to the class

~~~
final class User: Model { ... }
~~~

### ID

~~~swift
final class User: Model {
	var id: Node?
	...
}
~~~


###Node initializable

~~~swift
final class User: Model {
	init(node: Node, in context: Context) throws {
		id = try node.extract("id")
		name = try node.extract("name")
	}
	
	...
}
~~~

### Node representable

For showing the model how to save itself back into the database. Model uses `NodeRepresentable` to achieve this.

~~~swift
final class User:Model {
	func makeNode(context: Context) throws -> Node {
		return try Node(node: [
			"id":id,
			"name":name
		])
	}
}
~~~


## Full Model

~~~swift
import Vapor
import Fluent

final class User:Model {
	var id: Node?
	var name: String
	
	init(name:String) {
		self.name = name
	}

	init(node: Node, in context: Context) throws {
		id = try node.extract("id")
		name = try node.extract("name")
	}
	
	func makeNode(context: Context) throws -> Node {
		return try Node(node: [
			"id":id,
			"name":name
		])
	}
	
	static func prepare(_ database: Database) throws {
		try database.create("users") { users in
			users.id()
			users.string("name")
		}
	}
	
	static func revert(_ database: Database) throws {
		try database.delete("users")
	}
}
~~~

### Fetch

~~~
let user = try User.find(42)
~~~

### Save 

~~~
var user = User(name: "Vapor")
try user.save()
print(user.id)
~~~

### Delete
~~~
try user.delete()
~~~

# Query
***

## Querying Models

Every type that conforms to Model gets a static `.query()` method.

~~~
let query = try User.query()
~~~

## Filter

The most common types of queiries involve filtering data.

~~~swift
let smithsQuery = try User.query().filter("last_name", "Smith")
~~~

### Scope
Filters can be run on sets

~~~swift
let coolPets = try Pet.query().filter("type", .in, ["Dog", "Ferret"])
~~~

## Retrieving

There are two methods for running a query.

### All

ALl of the matching entities can be fetched. This returns an array of `[Model]`

~~~
let usersOver21 = try User.query().filter("age", .greaterThanOrEquals, 21).all()
~~~

### First

Returns the first matching entity. This returns an optional `Model?`

~~~
let firstSmith = try User.query().filter("last_name", "Smith").first()
~~~



