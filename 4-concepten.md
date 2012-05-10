# **Camel** Concepten

---

# Message

Wat is een message?

![message](resources/message.png)

.notes: messages zijn entiteiten die worden gebruikt om data van het ene naar het andere systeem te sturen

Waaruit bestaat een message?

![headers-attachments-body](resources/headers-attachments-body.png)

.notes: messages hebben een body (payload), headers en optionele attachments en worden uniek geidentifieerd met behulp van een identifier. Messages kunnen ook een 'fault flag' bevatten (e.g. voor SOAP en JBI)

---

# Exchange

![exchange](resources/exchange.png)

---

# Camel concepts

* CamelContext
* Routing Engine
* Routes
* DSL: opmerking instinkers: 1) scope van DSL, 2) volgorde
* Processor
* Component
* Endpoint
* Producer
* Consumer

Inclusief bean registry (afhankelijk van container)

---

# Camel oefening

.notes: Camel oefening met Testcase (Ton)

eenvoudige processor