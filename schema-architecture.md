# Smart Clinic Management System - Architecture Design Document

## Section 1: Architecture Summary
The Smart Clinic Management System is built on a robust three-tier web application architecture utilizing the Spring Boot framework to ensure clean separation of concerns, scalability, and ease of deployment. The Presentation Tier supports a hybrid delivery model: server-rendered HTML pages powered by Spring MVC and Thymeleaf are dedicated to the Admin and Doctor dashboards, while lightweight, JSON-based REST APIs handle public integration modules like patient registration, authentication, and appointment scheduling.

At the core of the Application Tier, inbound web and API requests are managed by their respective controllers, which delegate all core business rules and cross-entity workflows to a centralized Service Layer. To support the application's diverse data requirements, the Data Tier employs a dual-database polyglot persistence design strategy. Highly structured, relational data (such as Patient, Doctor, Appointment, and Admin accounts) is managed securely via Spring Data JPA within a MySQL instance. Concurrently, flexible, document-based health records (such as highly variable Patient Prescriptions) are handled via Spring Data MongoDB using dynamic BSON/JSON structures. 

---

## Section 2: Numbered Flow of Data and Control

1. **User Interface Interaction:** A user initiates an action (such as loading a dashboard or submitting an appointment request) either through a server-side Thymeleaf UI (Admin/Doctor dashboards) or an external client consuming the system's REST APIs.
2. **Controller Routing:** The request hits the application backend, where the Spring Boot framework routes it to either a traditional Thymeleaf Controller (returning an HTML view) or a REST Controller (returning a serialized JSON response) based on the URL context and HTTP verb.
3. **Business Logic Processing:** The assigned controller validates the incoming payload data and passes the execution context downstream to the Service Layer, which processes business logic, rules, and cross-module scheduling rules.
4. **Repository Delegation:** The Service Layer determines the storage requirements of the transaction and communicates with the Repository Layer, querying either a MySQL JPA repository for core relational records or a MongoDB repository for clinical document records.
5. **Database Execution:** The respective repository accesses the underlying physical data engines—performing relational transactions on the MySQL instance or document read/write routines on the MongoDB collection.
6. **Model Binding:** Database rows and collections returned by the engines are mapped back into Java objects. Relational MySQL entries are bound into standard JPA `@Entity` classes, while unstructured MongoDB files are bound into Java `@Document` objects.
7. **Response Serialization & Rendering:** The bound data models travel back up to the Controller layer. For dashboard requests, the model properties are injected into Thymeleaf templates to output dynamic HTML. For API requests, the models are formatted as JSON payloads and sent back across the network to the consuming client.
