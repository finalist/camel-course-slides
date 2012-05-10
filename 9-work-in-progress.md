# Wat is Camel?

Een open source integratie framework

* **Integratie** door middel van routing en mediation engine 
* **Framework** bevat routing engine builder en components
* **Open Source** ondergebracht bij Apache 

.notes: routing en mediation: define routing rules, decide from which sources to accept messages, determine how to process and send those messages to other destinations.

.notes: components: ondersteuning voor meer dan 80 protocollen en datatypes voor transport en transformaties

http://camel.apache.org/

![Camel](resources/apache-camel.png)

.notes: Camel is géen ESB, hoogstens een lightweight ESB (wel monitoring en orchestratie, geen container of message bus)

---

# Waarom Camel?

**Routing and mediation engine**

.notes: routes worden geconfigureerd d.m.v. een combinatie van EIP's en een DSL

**Enterprise Integration Patterns (EIP)**

.notes: laat EIP boek zien - een catalogus van diverse integratieproblemen en hun oplossingen

* Zie ook: http://www.eaipatterns.com/

**Domain-specific languages (DSL)**

.notes: Camel biedt (interne) DSL's in verschillende host programmeertalen, a.k.a. fluent interfaces

* Zie ook: http://martinfowler.com/bliki/DomainSpecificLanguage.html

---

# DSL voorbeelden

Java

	!java
	from("file:data/inbox").to("jms:queue:order");

Spring DSL
	
	!xml
	<route>
		<from uri="file:data/inbox"/>
		<to uri="jms:queue:order"/>
	</route>

Scala
	
	!scala
	from "file:data/inbox" -> "jms:queue:order"

---

# Waarom Camel?

Payload-agnostic router

POJO model

Automatic type converters

Test kit

Extensive component library
Modular and pluggable architecture
Easy configuration
Lightweight core
Vibrant community

