Rate Limiting & Throttling
"Rate limiting is how you say no nicely when a client is asking too often or too fast."

In system design, rate limiting and throttling help protect your infrastructure from abuse, ensure fair usage, and maintain performance under load.

🧠 Why Rate Limit?
🔒 Security:
Prevent brute-force attacks (e.g., login attempts)

Block scraping or DDoS activity

🌐 API Protection:
Control traffic from third parties

Avoid overwhelming backend systems

💸 Cost Control:
Prevent overuse of resources like storage, compute, or bandwidth

🧮 Common Strategies
Strategy	Description
Fixed Window	Allow N requests per time window (e.g., 100 reqs per minute)
Sliding Window	Smoothes traffic over overlapping windows
Token Bucket	Tokens are added at a rate; each request consumes a token (bursty traffic OK)
Leaky Bucket	Like token bucket, but drains at a steady rate (ensures steady traffic)

📦 Algorithm Comparison
Algorithm	Burst Allowed?	Precision	Complexity
Fixed Window	Yes	Low	Low
Sliding Window	Yes (less)	Medium	Medium
Token Bucket	Yes	High	Medium
Leaky Bucket	No	High	High

🖼️ Architecture (Text Diagram)
text
Copiar
Editar
           +------------+
Client --> | Rate Limit | --> Server
           |  Layer     |
           +------------+

You can apply this at:
- API Gateway
- Load Balancer
- Backend Service
🛠️ Where to Implement
Layer	Example Tools
API Gateway	Kong, NGINX, AWS API Gateway
CDN Layer	Cloudflare, Akamai
Backend Application	Custom logic (e.g., Redis-based limiter)

🔧 Sample Rate Limiting Logic (Python + Redis)
python
Copiar
Editar
# Allow max 100 requests/user per minute
def is_allowed(user_id):
    key = f"rate_limit:{user_id}"
    current = redis.incr(key)
    if current == 1:
        redis.expire(key, 60)
    return current <= 100
🆚 Rate Limiting vs Throttling
Concept	Description
Rate Limiting	Enforces a maximum number of requests over time
Throttling	Intentionally delays or slows down excessive traffic

Example:

Rate limiting returns HTTP 429 (Too Many Requests)

Throttling slows down response time or queues tasks

📊 Real-World Use Cases
Use Case	Strategy
Public APIs	Token Bucket
Login endpoints	Fixed or Leaky Bucket
Expensive operations (PDF export)	Sliding Window
Chat systems	Rate limit messages

🛑 Error Handling
HTTP Response
http
Copiar
Editar
429 Too Many Requests
Retry-After: 60
Best Practices
Return clear retry hints (headers)

Provide rate usage in response headers

Allow burst but protect backend

💬 What to Say in Interviews
“I’d implement token bucket rate limiting using Redis to allow controlled bursts while enforcing an overall traffic limit.”

“To protect login routes, I’d use fixed windows with a backoff strategy and possibly CAPTCHA on repeated failures.”

🏁 Final Takeaway
“Rate limiting protects your system. Throttling smooths the traffic. Together, they ensure your architecture stays responsive — even under attack or abuse.”