Designing a URL Shortener (e.g., Bitly)
“Turn long, messy URLs into tiny, beautiful links — reliably and at scale.”

A URL shortener maps long URLs to short, unique identifiers. It’s deceptively simple but involves many architectural tradeoffs around scalability, consistency, and performance.

📌 Requirements
✅ Functional:
Shorten a long URL and return a unique short link

Redirect to original long URL when short link is visited

Support custom aliases (e.g., /myproduct)

Track click analytics (optional)

🚫 Non-Functional:
High availability and low latency

Handle massive read traffic

Minimal collision rate

Short links should not expire (or expire after X days)

🧪 Example
http
Copiar
Editar
POST /shorten
BODY: { "long_url": "https://example.com/my/long/path" }

Response: { "short_url": "https://sho.rt/abc123" }

GET /abc123 → HTTP 302 Redirect to original URL
⚙️ Core Components
pgsql
Copiar
Editar
+--------+       +-------------+       +-------------------+
| Client | <---> | API Gateway | <---> | URL Service       |
+--------+       +-------------+       | - Encode/Decode   |
                                       | - Store/Retrieve  |
                                       +-------------------+
                                                 |
                                            +----------+
                                            | Database |
                                            +----------+
🧠 Key Design Elements
1. Encoding Scheme
Use a Base62 (0–9, a–z, A–Z) encoding for compact IDs:

Example: 123456 → bUKZ

ID comes from an auto-increment DB ID or a random string

2. Storage
Schema:

sql
Copiar
Editar
CREATE TABLE urls (
  id SERIAL PRIMARY KEY,
  long_url TEXT,
  short_code VARCHAR(10) UNIQUE,
  created_at TIMESTAMP
);
Add an index on short_code for fast lookups.

3. Redirection Logic
When a user accesses /abc123:

Decode abc123 to an internal ID or look it up in DB

Retrieve long_url

Respond with HTTP 302 Redirect

🏗️ System Design Flow
🔁 Option A: Auto-increment ID + Base62
Use DB auto-increment ID (e.g., 1, 2, 3…)

Encode ID to Base62 → 1 → a, 2 → b, etc.

Simple, fast — but requires central coordination

🔀 Option B: Random Code Generation
Generate a random 6–8 char string

Check for collisions in DB

Use retry on conflict

Scales better for distributed systems

🧱 Scaling the System
Component	Scaling Strategy
Read-heavy traffic	Add caching with Redis/Memcached
Writes	Partition DB by hash of long URL or user ID
Link generation	Use distributed ID generator or UUID
High availability	Replication + Load balancing + Multi-region

🔐 Security & Abuse Prevention
Rate limit API to avoid spam

Allow link expiration or domain-level restrictions

Block malicious domains with a denylist

🧪 Optional: Analytics
Track metrics like:

Click count

Referrer

Country / IP

Device/browser

Schema:

sql
Copiar
Editar
CREATE TABLE clicks (
  short_code VARCHAR,
  timestamp TIMESTAMP,
  referrer TEXT,
  user_agent TEXT,
  ip_address TEXT
);
💬 What to Say in Interviews
“I’d design the service using a Base62-encoded auto-increment ID. Redis would cache recent short→long URL mappings for fast lookups. I’d use PostgreSQL with a unique constraint on short codes and consider Snowflake IDs if we scale horizontally.”

“To handle 100M+ links, we can shard based on user ID and store click data in a separate analytics system.”

✅ Pros & Cons of ID Approaches
Method	Pros	Cons
Base62 from DB ID	Simple, compact, ordered	Hard to scale write node
Random code	No coordination needed	Collision check required
Hash of URL	Same input → same output	Hard to support custom aliases

🧰 Tools & Tech Stack
Component	Tool Options
Backend	Node.js, Python, Go, Java
DB	PostgreSQL, MySQL, DynamoDB
Caching	Redis, Memcached
CDN	Cloudflare, Fastly (optional)
Deployment	Docker, K8s, AWS/GCP/Azure

🧠 Bonus Enhancements
Support private/internal links

Link expiration (TTL)

User accounts to manage links

QR code generation

Preview page before redirection

🧭 Summary
Feature	Implementation
URL shortening	Base62 encoding / random key
Storage	Relational DB or KV store
Redirection	302 HTTP redirect
Scaling	Caching + ID generation
Monitoring	Click logs + analytics