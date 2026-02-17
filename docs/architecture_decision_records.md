# Architecture Decision Record (ADR): Operations Unification Tool

## 1. Context & Problem Statement
An operations analyst needs to unify Production, Inspection, and Shipping data currently siloed in multiple spreadsheets. The solution must provide immediate, consistent answers for meetings (AC7, AC9) without manual reconciliation, while preventing **Scope Creep** through a clearly defined technical boundary.

---

## 2. Architecture Inputs Mapping

| Input | Value | Architectural Impact |
| :--- | :--- | :--- |
| **Users** | Operators, Engineers, Managers | Requires role-based access and simple, intuitive UX. |
| **Usage Context** | Office & Factory Floor | Favors a web-based UI usable on standard PCs and laptops. |
| **User Scale** | Department-wide | Supports a few to several concurrent users without complex scaling. |
| **Data Volume** | Thousands to millions of rows | Necessitates a database-backed solution with indexing and efficient joins. |
| **Concurrency** | A few simultaneous users | Single application server and single database instance are sufficient initially. |
| **Reliability** | Retry on failure, no data loss for stored data | Requires robust error handling and database backups; spreadsheet read failures must be retried or surfaced clearly. |
| **Security** | Role-based access | Requires login plus role-based authorization (operators vs engineers vs managers). |
| **Team Size & Background** | 1–2 developers, experienced | Favors a monolithic architecture over microservices to reduce overhead. |
| **Project Timeline** | Weeks to a few months | Avoids over-engineered distributed systems; optimizes for fast delivery and maintainability. |

---

## 3. Selected Architecture Dimensions

### D1: System Roles & Communication → **Client–Server**
- **Decision:** Use a synchronous request–response client–server model.
- **Rationale:** Users require immediate feedback when searching for a Lot ID or date range (AC1, AC3), especially in meetings.
- **Engineering Benefit:** Simplifies debugging and state management compared to event-driven models, with clear request and response boundaries.
- **Technology Example:** Web client (e.g., React/Vue/etc.) consuming a REST API backend.

### D2: Deployment & Evolution → **Monolith (Modular)**
- **Decision:** Build and deploy as a single application (modular monolith).
- **Rationale:** Given the small team size and strong alignment needs across Production, Inspection, and Shipping data (AC2), a monolith reduces network, deployment, and operational complexity.
- **Engineering Benefit:** Keeps the “Lot ID” join logic within a single memory space, while structuring code into clear modules (production, inspection, shipping, reporting) to preserve a future path to further decomposition if required.

### D3: Code Organization → **Layered Architecture**
- **Decision:** Organize server code into Presentation, Business Logic, and Data Access layers.
- **Rationale:** Protects the system from “dirty data”: the Data Access layer handles spreadsheet ingestion and mapping, while the Business Logic layer handles multi-functional alignment and rules (AC2, AC4, AC10).
- **Engineering Benefit:** High maintainability; if spreadsheet formats or locations change, only the Data Access layer requires updates, minimizing impact on UI and business rules.

### D4: Data & State Ownership → **Single Database**
- **Decision:** Use a centralized relational database (e.g., PostgreSQL or SQL Server).
- **Rationale:** This is a reporting-heavy application; joining thousands to millions of rows across Production, Inspection, and Shipping (AC1, AC5, AC6) is most efficient in a single database using SQL joins and indexes.
- **Engineering Benefit:** Provides strong consistency (AC9); users see a single source of truth, avoiding conflicting views of lot d
