Designing a Rate Limiter
“A rate limiter is a system's way of saying: ‘Slow down, but don’t stop.’”

Rate limiting ensures a user or client doesn’t exceed a defined number of requests in a specific time frame — protecting APIs, preserving system stability, and providing fair usage.

🧠 When Do You Need Rate Limiting?
Prevent brute-force or spam (e.g. login pages)

Protect backend services or APIs from overload

Ensure fair usage of shared resources (public APIs)

Prevent billing spikes in pay-per-use systems

✅ Requirements
Requirement	Why It Matters
Accuracy	Enforce exact or near-exact limits
Low latency	Add minimal delay to request flow
Scalability	Handle high request volumes
Distributed-safe	Must work across multiple servers
Burst handling	Allow temporary spikes, then enforce cap

⏱ Rate Limiting Strategies
1. Fixed Window
text
Copiar
Editar
Allow N requests per fixed window (e.g. 100 reqs/min)
Simple to implement

May allow burst traffic near window edges

2. Sliding Window Log
text
Copiar
Editar
Store timestamps of each request and evict old ones
Accurate but memory-intensive (per-user logs)

3. Sliding Window Counter
text
Copiar
Editar
Use two windows to interpolate counts and smooth limits
Balances precision and performance

4. Token Bucket (most common)
text
Copiar
Editar
- Tokens are added at a fixed rate
- Each request consumes a token
- Allows bursts if tokens are available
5. Leaky Bucket
text
Copiar
Editar
- Requests enter a queue (bucket)
- Requests are processed at a steady rate
🖼️ Architecture Diagram (Text-Based)
text
Copiar
Editar
          +-------------------+
Client -->|  Rate Limiter     |--> Backend Service
          +-------------------+

Rate limiter checks:
✅ Has this client exceeded allowed quota?
❌ If yes, return HTTP 429 (Too Many Requests)
🛠 Implementation Approaches
Option 1: In-Memory (Local)
Fastest (no network)

Not distributed-safe

python
Copiar
Editar
# Python example: Fixed Window per IP
limits = defaultdict(list)

def is_allowed(ip):
    now = time.time()
    window = 60  # seconds
    limits[ip] = [ts for ts in limits[ip] if now - ts < window]
    if len(limits[ip]) >= 100:
        return False
    limits[ip].append(now)
    return True
Option 2: Distributed – Redis-Based
✅ Central state

✅ Works across nodes

Ideal for APIs or microservices

python
Copiar
Editar
# Redis-backed Token Bucket
def is_allowed(user_id):
    key = f"rate_limit:{user_id}"
    current = redis.incr(key)
    if current == 1:
        redis.expire(key, 60)  # 60 sec window
    return current <= 100
📊 Where to Apply Rate Limiting
Layer	Example Tools
API Gateway	AWS API Gateway, NGINX, Kong
Load Balancer/CDN	Cloudflare, Akamai
Application Layer	Custom logic (Redis, DB, memory)

📬 API Behavior
Response on limit exceeded:
http
Copiar
Editar
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Optional Headers:
makefile
Copiar
Editar
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 20
X-RateLimit-Reset: 1695774000
🧪 Use Cases
Scenario	Strategy
API Platform (per token)	Token Bucket (Redis)
Login route protection	Fixed Window
Messaging / Chat	Leaky Bucket
Expensive endpoint (PDF gen)	Sliding Counter

🆚 Rate Limiting vs Throttling
Feature	Rate Limiting	Throttling
Action	Rejects requests	Delays or slows them down
Use case	Prevent abuse	Manage gradual load handling

💬 What to Say in Interviews
“I’d implement a token bucket algorithm using Redis for a globally consistent rate limiter across all service nodes. Each request consumes a token, and tokens refill at a steady rate.”

“To prevent abuse of login endpoints, I’d use a fixed window per IP and introduce exponential backoff on violations.”

🧠 Final Takeaways
🔒 Rate limiting protects systems from overuse and abuse

🚀 Use Redis for scalable, distributed rate limiting

⚖️ Choose algorithms based on burst tolerance and fairness

✅ Design with fail-safes and user feedback (HTTP 429, retry-after)