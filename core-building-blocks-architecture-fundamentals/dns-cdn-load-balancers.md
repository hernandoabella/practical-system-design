🌐 DNS, CDN, & Load Balancers – The Frontline of Scalable Systems
“Before your app gets to shine, these three silently make sure your users even reach it — quickly, reliably, and globally.”

When designing large-scale systems, these foundational components — DNS, CDN, and Load Balancers — play critical roles in performance, availability, and global reach. Let’s break each down.

🧭 1. Domain Name System (DNS)
Converts a human-readable domain name (like www.example.com) into an IP address that computers use to locate services.

🔧 How It Works
User enters www.example.com

The browser asks the local DNS resolver

Resolver queries:

Root name server (.)

Top-Level Domain (e.g. .com)

Authoritative DNS (example.com)

DNS resolver returns the IP address, and it's cached for future requests.

✅ Key DNS Techniques
Technique	Benefit
Caching	Speeds up repeat lookups
TTL tuning	Balance between freshness and availability
Anycast	Routes request to closest DNS node
GeoDNS	Returns region-specific server IPs

📌 Real-World Example
Netflix uses Anycast DNS + GeoDNS to direct traffic to the nearest region for lower latency.

🚀 2. Content Delivery Network (CDN)
A globally distributed network of edge servers that cache and serve static content (like JS, CSS, images, and videos) as close to the user as possible.

⚙️ What CDNs Do
Reduce latency by shortening the distance content travels

Offload origin servers to reduce load

Provide failover and DDoS mitigation

🔁 Static Assets Typically Served by CDN:
Images & videos

Fonts

JavaScript/CSS

PDFs

Static HTML pages (for SSG sites)

✅ Popular CDN Providers
Cloudflare

Fastly

Akamai

Amazon CloudFront

📊 CDN Features Comparison
Feature	Benefit
Edge Caching	Speeds up global access
Asset Versioning	Avoids broken caches
DDoS Protection	Blocks massive attack traffic
SSL Termination	Offloads HTTPS load from backend

⚖️ 3. Load Balancers
Distribute incoming traffic across multiple backend servers to ensure availability, reliability, and scalability.

🛠️ Common Load Balancing Algorithms
Type	Description
Round Robin	Cycles through all servers evenly
Least Connections	Routes traffic to the server with the fewest active users
IP Hashing	Routes based on IP (sticky sessions)
Weighted Routing	Sends more traffic to stronger machines

❤️ What Load Balancers Handle
Route traffic across multiple servers

Perform health checks

Terminate SSL/TLS

Enable horizontal scaling

Handle failover if one server crashes

📌 Load Balancer Types
Type	Use Case	Example Tools/Services
L4 (Transport)	Balances based on IP & port	Nginx (TCP), HAProxy, AWS NLB
L7 (Application)	Balances based on URL, cookies	Nginx, Envoy, AWS ALB

🧩 How They Work Together (Diagram in Text)
text
Copiar
Editar
User
 ↓
DNS → Returns IP address of CDN/Load Balancer
 ↓
CDN → Serves static assets (cached) or forwards to origin
 ↓
Load Balancer → Chooses optimal backend server
 ↓
Web Server → Handles request, queries DB/cache
 ↓
Response returns via same path
🧪 Example: Global E-commerce Site
Problem
Serve users from North America, Europe, and Asia with <100ms latency.

Solution
GeoDNS: Routes to nearest region (US/EU/APAC)

CDN (e.g., Cloudflare): Caches product images/videos

L7 Load Balancer (e.g., AWS ALB): Distributes traffic to app instances

Auto Scaling: Spin up instances during Black Friday

💬 What to Say in an Interview
“To minimize latency and support millions of users globally, I’d use GeoDNS + CDN in front of a load balancer that distributes traffic to horizontally-scaled app servers. The CDN handles static assets, reducing backend load, and the load balancer ensures failover and availability.”

✅ Summary Table
Component	Role in Architecture	Real-World Tool Examples
DNS	Resolves domain to IP	Route53, Cloudflare DNS
CDN	Caches static content near users	Cloudflare, Fastly, Akamai
Load Balancer	Routes traffic to backend servers	AWS ELB, HAProxy, Nginx

🏁 Final Takeaway
“DNS gets you to the front door, the CDN shows you around quickly, and the Load Balancer decides who answers.”