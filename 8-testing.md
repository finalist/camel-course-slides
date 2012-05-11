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

	!java
	...
	import org.apache.camel.test.junit4.CamelTestSupport;
	...
	
	public class FirstTest extends CamelTestSupport {
		@Override
		protected RouteBuilder createRouteBuilder() throws Exception {
			return new RouteBuilder() {
				@Override
				public void configure() throws Exception {
					from("file://target/inbox")
						.to("file://target/outbox");
			};
		}
		@Test
		public void testMoveFile() throws Exception {
			template.sendBodyAndHeader("file://target/inbox", 
				"Hello World", Exchange.FILE_NAME, "hello.txt");
			Thread.sleep(1000);
			File target = new File("target/outbox/hello.txt");
			assertTrue("File not moved", target.exists());
		}
	}
	
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

	!java
	package camelinaction;
	import org.apache.camel.builder.RouteBuilder;
	import org.apache.camel.component.mock.MockEndpoint;
	import org.apache.camel.test.junit4.CamelTestSupport;

	public class FirstMockTest extends CamelTestSupport {
		@Override
		protected RouteBuilder createRouteBuilder() throws Exception {
	        return new RouteBuilder() {
	            @Override
	            public void configure() throws Exception {
	                from("jms:topic:quote").to("mock:quote");
			}};
		}
		@Test
	    public void testQuote() throws Exception {
	        MockEndpoint quote = getMockEndpoint("mock:quote");
	        quote.expectedMessageCount(1);
	        template.sendBody("jms:topic:quote", "Camel rocks");
	        quote.assertIsSatisfied();
		}
	}

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

