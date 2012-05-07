# Hands-on

---

# Hands-on

Topics:

* Routing
* Testing
* Behavior
* Error Handling

---

# Routing - HTTP proxy voorbeeld

Een eenvoudige route voorbeeld (HTTP proxy):

	<route>
     <from uri="jetty:http://0.0.0.0:8080/myapp?
     	matchOnUriPrefix=true"/>
     <to uri="jetty:http://realserverhostname:8090/myapp?
     	bridgeEndpoint=true&amp;throwExceptionOnFailure=false"/>
    </route>

---

# Testing

* CamelTestSupport
* Integratietesten
* Zelf Camel bootstrappen voor tests

---

# Behavior

* Hoe voeg je maatwerk-logica toe?
* Beans, POJO's
* Processors
* Componenten

---

# Error Handling

* Error Handlers
* Exceptions