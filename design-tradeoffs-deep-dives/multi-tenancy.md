Multi-Tenancy in System Design
“One app, many customers — securely and efficiently.”

🔍 What is Multi-Tenancy?
Multi-tenancy is a software architecture pattern where a single instance of an application serves multiple tenants (customers). Each tenant's data is isolated, but they all share the same infrastructure and codebase.

🧠 Definitions:
Tenant: An individual customer or organization using the app

Instance: The running version of your application or infrastructure

Isolation: Ensuring that tenants cannot access each other's data

🏗️ Types of Multi-Tenancy Architectures
1. Shared Everything
One DB, one app instance

Tenants are identified via a tenant_id column

✅ Pros:

Simplest to manage and deploy

Efficient use of resources

❌ Cons:

Risk of data leakage

Harder to scale per tenant

2. Shared App, Isolated Database
One app codebase

Each tenant has a separate database

✅ Pros:

Stronger data isolation

Easier tenant-specific backups

❌ Cons:

Slightly more complex ops

May increase costs with more DBs

3. Fully Isolated (Single-Tenant)
Each tenant has their own app instance and DB

✅ Pros:

Maximum data and performance isolation

Tenant-specific SLAs or customizations

❌ Cons:

High infrastructure cost

Complicated deployment

🧰 Example: SaaS CRM for Companies
Tier	Description
Free Tier	Shared DB and shared app
Pro Tier	Shared app, isolated DB
Enterprise	Dedicated app + DB + region (fully isolated)

📊 Architecture Diagram (Text Form)
text
Copiar
Editar
                   ┌──────────────┐
                   │  Users       │
                   └─────┬────────┘
                         │
                  ┌──────▼───────┐
                  │ Multi-Tenant │
                  │ Application  │
                  └──────┬───────┘
            ┌────────────┼────────────┐
            │            │            │
      ┌─────▼────┐ ┌─────▼────┐ ┌─────▼────┐
      │Tenant A DB│ │Tenant B DB│ │Tenant C DB│
      └──────────┘ └──────────┘ └──────────┘
🛡️ Security & Isolation Considerations
Use Row-Level Security in shared DBs

Enforce tenant_id scoping in every query

Encrypt tenant data at rest & in transit

Apply per-tenant rate limits and quotas

⚙️ Key Technical Decisions
Area	Consideration
Tenant Routing	Route traffic via subdomain (acme.app.com) or token
Data Isolation	Row-level, schema-level, or DB-level isolation
Billing	Track usage per tenant for metered billing
DevOps	Use IaC templates to spin up isolated tenant stacks
Monitoring	Segment metrics per tenant (Grafana, Prometheus)

📦 When to Choose Which Model?
Situation	Best Approach
MVP, early-stage SaaS	Shared DB & App
Medium-size SaaS with growing clients	Shared App, separate DBs
Enterprise-focused or regulated domain	Fully isolated stacks

💬 What to Say in an Interview
"I’ve designed multi-tenant systems that scale from startups to enterprise. I start with shared databases for simplicity, then move to isolated databases for larger tenants to meet SLA, data locality, and compliance needs."

