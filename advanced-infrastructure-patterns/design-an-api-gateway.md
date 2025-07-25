Designing an API Gateway
“An API Gateway is the front door to your services — it handles requests, enforces policies, and shapes how clients interact with your backend.”

🧠 What Is an API Gateway?
An API Gateway is a single entry point that sits between clients and backend services. It routes, secures, transforms, and monitors API calls — making it crucial for microservice-based systems.

🧱 Responsibilities of an API Gateway
Responsibility	Explanation
Routing	Routes incoming requests to the correct backend service
Authentication	Verifies identity (JWT, OAuth2, API keys)
Rate Limiting	Prevents abuse with request quotas
Load Balancing	Distributes traffic evenly across instances
Caching	Reduces latency by caching frequent responses
Request Transformation	Modifies headers, parameters, or formats
Monitoring & Logging	Tracks metrics like latency, error rate, traffic
Security	Prevents injection, DDoS, CORS abuse, and SSL termination

🖼 Text-Based Diagram – API Gateway in Action
text
Copiar
Editar
+-----------+       +-----------------+        +---------------------+
|  Clients  | --->  |   API Gateway   | -----> |   Microservices      |
+-----------+       +-----------------+        +---------------------+
                        |       |       \
                  AuthZ &  Caching   Logging
                   AuthN
✅ Why Use an API Gateway?
Simplifies client logic

Enables centralized security & logging

Shields services from breaking changes

Abstracts multiple microservices behind one URL

Supports versioning and A/B testing

🔍 Use Cases
Use Case	Gateway Feature
Mobile & web frontends	Request transformation, BFF pattern
Rate limiting & billing APIs	Throttling, metering
Public APIs with tiers	Key-based authentication
Multi-versioning APIs	URI rewriting, version routing

🚀 Common API Gateway Products
Tool	Type	Notes
Kong	Open Source	Lua plugins, scalable
NGINX	Config-based	Lightweight, fast, powerful
AWS API Gateway	Managed	Integrated with AWS Lambda & IAM
Apigee	Enterprise	Advanced analytics, monetization
Traefik	Dynamic	Auto-discovers services via Docker/K8s

🔐 Authentication Options
JWT Tokens – Validate user identity with stateless tokens

OAuth2 – For delegated auth via third parties (Google, GitHub)

API Keys – For public or tiered usage plans

HMAC / Signature – For secure data integrity between services

📦 Request Transformation
Gateways often modify requests/responses for compatibility:

json
Copiar
Editar
Client sends:
{
  "firstName": "John",
  "lastName": "Doe"
}

Gateway transforms to:
{
  "full_name": "John Doe"
}
Also used for:

Header injection

URL rewriting

Format conversion (XML ↔ JSON)

🧱 Backend-for-Frontend (BFF) Pattern
In modern apps, different clients (web, mobile) may need different APIs.

BFF creates client-specific gateways to reduce over-fetching/under-fetching:

nginx
Copiar
Editar
Mobile App → Mobile BFF
Web App → Web BFF
Each BFF aggregates or simplifies microservices data per client’s needs.

⚖️ API Gateway vs Reverse Proxy
Feature	API Gateway	Reverse Proxy
Protocol awareness	HTTP + business logic	Low-level TCP/HTTP
Transformations	Yes	Minimal
Authentication	Yes	No
Rate Limiting	Yes	No
Request routing	Based on API logic	Based on hostname

💬 What to Say in Interviews
“I’d use an API Gateway to centralize concerns like auth, rate limiting, and versioning. It allows us to decouple clients from internal service structure.”

“If we expect multiple client types, I’d apply the BFF pattern to optimize payloads and avoid coupling UI logic with backend contracts.”

🧠 Best Practices
Use JWT validation at the edge to avoid unnecessary backend calls

Implement rate limiting per IP / user / API key

Log latency, success/failure rate, and upstream errors

Always enable HTTPS termination

Use path-based routing for microservices (/user/, /order/, etc.)

Add timeouts and retries for upstream services

🧪 Bonus: Sample NGINX Gateway Config
nginx
Copiar
Editar
server {
  listen 443 ssl;
  
  location /user/ {
    proxy_pass http://userservice.local;
  }

  location /auth/ {
    proxy_pass http://authservice.local;
  }

  # Rate limiting
  limit_req zone=api_limit burst=10 nodelay;
}
📘 Summary Box
✅ API Gateway is the control plane for your APIs

🎯 Provides routing, auth, logging, rate limiting, caching

🛡️ Shields services from traffic spikes and security threats

🧱 Supports microservice patterns, especially BFF and zero-trust