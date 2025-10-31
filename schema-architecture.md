Section 1: Architecture summary (EN)

This Spring Boot application combines MVC (for server-rendered Thymeleaf dashboards) and REST APIs (for modular client access). User requests hit controllers that delegate business logic to a centralized Service Layer, which coordinates validation, workflows, and transactions. Data access is abstracted via Repository interfaces.

The system uses two databases behind a single service boundary: MySQL (via Spring Data JPA) for structured entities like Patient, Doctor, Appointment, and Admin; and MongoDB (via Spring Data MongoDB) for flexible Prescription documents. Retrieved data is bound into Java model classes—JPA entities for MySQL and @Document models for MongoDB—and returned either to Thymeleaf views (HTML) or serialized as JSON for REST clients. This separation of Presentation, Application, and Data tiers improves maintainability, scalability, and CI/CD-friendly deployment (Docker/K8s).

Section 2: Numbered flow of data and control (EN)

1. User Action → A user opens an admin/doctor dashboard (Thymeleaf) or calls a REST endpoint (appointments/patient modules).

2. Routing to Controller → Spring routes the request to the matching Thymeleaf Controller (HTML view) or REST Controller (JSON 22API).

3. Validation & Delegation → The controller validates input (path/query/body) and delegates to the Service Layer.

4. Business Logic → The service applies rules/workflows (e.g., doctor availability before scheduling) and determines which repositories are needed.

5. Repository Access → Services call MySQL Repositories (JPA) for relational entities or MongoDB Repository for prescription documents.

6. Database I/O & Model Binding → Data is persisted/fetched in MySQL or MongoDB, then mapped to JPA entities or @Document models/DTOs.

7. Response Rendering → For MVC, models are added to a Thymeleaf view and rendered as HTML; for REST, results are serialized to JSON and returned to the client.
