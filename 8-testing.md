# Testen met **Camel**

---

# Camel JUnit Extensions

JUnit 3.x classes:

	!java
	org.apache.camel.test.TestSupport
	org.apache.camel.test.CamelTestSupport
	org.apache.camel.test.CamelSpringTestSupport
	
JUnit 4.x classes:

	!java
	org.apache.camel.test.junit4.TestSupport
	org.apache.camel.test.junit4.CamelTestSupport
	org.apache.camel.test.junit4.CamelSpringTestSupport

---

# Camel JUnit Extensions

Insert Listing 6.1

---

# Using the Mock component

De Mock component is bruikbaar wanneer de 'echte' component:

* Niet voorhanden is
* Te langzaam is
* Gemodificeerd moet worden om deze testbaar te maken
* Nondeterministische resultaten teruggeeft

Voorbeeld:

	!java
	from("jms:topic:quote").to("mock:quote")
	
---

# MockEndpoint methods

	!java
	expectedMessageCount(int count)
	expectedMinimumMessageCount (int count)
	expectedBodiesReceived (Object... bodies)
	expectedBodiesReceivedInAnyOrder (Object... bodies)
	assertIsSatisfied()
	
---

# MockEndpoint voorbeeld

Listing 6.6

---

# Simuleren van componenten

	!java
	@Override
	protected CamelContext createCamelContext() throws Exception {
		CamelContext context = super.createCamelContext(); 
		context.addComponent("jms", context.getComponent("seda")); 
		return context;
	}

---

# Simulating errors

De Mock component is ook bruikbaar om netwerkfouten of 'faults' te simuleren

Voorbeeld, de Camel route:

	!java
	errorHandler(defaultErrorHandler()
	    .maximumRedeliveries(5)
		.redeliveryDelay(1000));
	onException(IOException.class).maximumRedeliveries(3)
	    .handled(true)
	    .to("mock:ftp");
	from("direct:file")
		.process(new Processor()) {
			public void process(Exchange exchange) throws Exception { 
				throw new ConnectException(
					"Simulated connection error");
		}})
		.to("mock:http");

---

# Simulating errors

En de test method:

	!java
	@Test
	public void testSimulateConnectionError() throws Exception {
	getMockEndpoint("mock:http").expectedMessageCount(0);
	    MockEndpoint ftp = getMockEndpoint("mock:ftp");
	    ftp.expectedBodiesReceived("Camel rocks");
	    template.sendBody("direct:file", "Camel rocks");
	    assertMockEndpointsIsSatisfied();
	}

