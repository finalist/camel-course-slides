# Extra's

---

# Extra's

* Camel's Scala DSL
* Camel en Akka

---

# Camel's Scala DSL

Code voorbeeld:

	!scala
	
	class MyRouteBuilder extends RouteBuilder {
		"direct:a" --> "mock:a"
		"direct:b" to "mock:b"
	}
	
* Er is geen <code>configure()</code> die je moet overriden
* Een route begint direct met een URI in plaats van <code>from(uri)</code>
* <code>--></code> is gewoon een alias voor <code>to</code>

---

# Camel en Akka

Wat is Akka?

"Akka is a toolkit and runtime for building highly concurrent, distributed and fault tolerant event-driven applications on the JVM"

(van de [website](http://akka.io/))

