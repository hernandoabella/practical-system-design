Designing a Payment Processing System (e.g., Stripe, PayPal)
A robust payment system handles user transactions, ensures security, guarantees consistency, and complies with financial regulations. Think: Stripe, PayPal, Razorpay, etc.

🧾 Functional Requirements
✅ Core Features
Process credit/debit card payments

Support wallets, bank transfers, BNPL, etc.

Handle refunds, cancellations, and chargebacks

Manage payment status (pending, success, failed)

Tokenize and securely store card data (PCI compliance)

Webhooks for notifying merchant systems

Dashboard for merchants to manage transactions

🚫 Non-Functional Requirements
High availability (critical for business uptime)

Strong security and fraud prevention

Regulatory compliance (PCI-DSS, GDPR, AML, KYC)

Global scale and currency support

Idempotency for safe retries

🎯 System Goals
Property	Target
Availability	99.999% (critical infrastructure)
Latency	< 200ms for most requests
Consistency	Strong (ACID transactions)
Throughput	Millions of transactions per day
Compliance	PCI-DSS, GDPR, SOC2, etc.

🧠 High-Level Architecture
plaintext
Copiar
Editar
+--------------+       +------------------+       +------------------+
|  User Client | <---> |  Payments API    | <---> |  Payment Processor|
+--------------+       +------------------+       +------------------+
                           |           |                    |
                           |           ↓                    ↓
                           |   +---------------+    +-----------------+
                           |   | Vault / Token |    |  Fraud Detection|
                           |   +---------------+    +-----------------+
                           ↓
                   +------------------+
                   |   Ledger System  |
                   +------------------+
                           |
                   +------------------+
                   | Bank / Card APIs |
                   +------------------+
🔐 Key Components
1. 🔐 Tokenization & Vault
Never store raw card data

Use tokenization (replace PAN with a random token)

Use secure vault (like HashiCorp Vault or HSM) for mapping

json
Copiar
Editar
{
  "card_token": "tok_1Gd5sE...zJ9",
  "last4": "4242",
  "expiry": "12/28"
}
2. 🧾 Payments API Gateway
Handles:

Authentication and rate limiting

Routing to correct flow (card, wallet, BNPL)

Validations (amount, currency, idempotency key)

Idempotency Key Example:

http
Copiar
Editar
POST /charge
Headers: Idempotency-Key: 63f8f6aa-e924-4f3f-b122
3. 🧠 Fraud Detection
Uses rules + ML models

Features: geolocation mismatch, velocity checks, blacklisted IPs/devices

Tools: Stripe Radar, Sift, custom scoring

4. 📓 Ledger System
Core of the system. Tracks all transactions.

Immutable. All operations are append-only.

Follows double-entry bookkeeping:

Debit customer

Credit merchant

Example:

diff
Copiar
Editar
+100 → merchant_abc
−100 → customer_xyz
5. 🔄 Retry & Webhooks
Payment retries handled via exponential backoff

Merchant notified via webhooks (e.g., /webhook/payment_success)

Must be idempotent and signed with HMAC-SHA256

6. 🏦 Third-party Integrations
Bank APIs (ACH, SEPA, UPI)

Card processors (Visa, MasterCard, AMEX)

Wallets (PayPal, Apple Pay, etc.)

Use abstract interface so switching vendors is easy

🗃️ Database Design (Simplified)
sql
Copiar
Editar
users(id, name, email)

cards(id, user_id, card_token, last4, brand)

transactions(id, user_id, amount, status, currency, created_at)

ledger(id, transaction_id, entry_type, account_id, amount)

webhooks(id, event_type, payload, status)
🧠 Interview Points
“I would store all monetary flows in an append-only ledger system to ensure consistency and traceability. Sensitive card data would be tokenized and stored securely via an HSM-backed vault. I’d use idempotent APIs and queue-based retries to prevent duplicate charges. Fraud prevention would be ML-assisted and multi-layered.”

⚙️ Scaling Techniques
Problem	Solution
Idempotency & retries	Unique keys per transaction + Redis cache
Real-time fraud detection	Stream data to ML models (e.g., Kafka + Flink)
Slow external APIs (banks)	Async queue + webhook-based update
Global latency	Regional edge gateways + distributed vaults

✅ Summary Table
Component	Tech Stack / Strategy
API Gateway	Rate limiting, idempotency keys
Token Vault	HSM, Hashicorp Vault
Ledger System	ACID-compliant store (e.g., PostgreSQL)
Queueing	Kafka / SQS
Fraud Detection	ML scoring + rule engine
Persistence	PostgreSQL / DynamoDB
Notification	Webhooks with retry + signing

Bonus: Real-World Features You Could Add
3D Secure (OTP verification)

Multi-currency support

Instant payout options

Dispute/Chargeback dashboard

Audit trail for compliance

Subscription & Recurring Billing