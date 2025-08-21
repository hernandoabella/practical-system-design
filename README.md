# Practical System Design
## [Introduction to System Design](./introduction-to-system-design)
- What Is System Design?
- Interview vs Real-World Design
- Scalability, Availability, Reliability
- Types of Systems: OLTP vs OLAP, Batch vs Real-Time
- A Framework for System Design Interviews
- 4-Step Framework for Solving Any System Design Question
- What Interviewers Really Look For (Insider’s View)
## [System Design Process & Core Concepts](./system-design-process-core-concepts/process-&-core-concepts.md)
- Requirement Gathering
- Estimating Load & Traffic
- Back-of-the-Envelope Estimation
- Designing APIs & Interfaces
- Defining Bottlenecks & Tradeoffs
- CAP Theorem in Practice
- Consistency vs Availability
- Latency vs Throughput
- Data Freshness vs Cost
- System Cost vs User Experience
## [Core Building Blocks & Architecture Fundamentals](./core-building-blocks-architecture-fundamentals)
- DNS, CDN, Load Balancers
- Web Servers & Reverse Proxies
- Monolith vs Microservices
- Containers (Docker, Kubernetes)
- CI/CD Pipelines
- Auto Scaling & Blue/Green Deployment
- Service Discovery (Consul, etcd)
## [Databases, Caching & Queues](./databases-caching-queues/)
- SQL vs NoSQL vs NewSQL
- Caching Systems (Redis, Memcached)
- Caching Strategies (Write-through, TTL, Lazy loading)
- Queues (Kafka, RabbitMQ, SQS)
- Database Sharding & Replication
- Indexing & Search (Elasticsearch)
## [Storage Systems](./storage-systems/)
- File Storage (S3, GCS)
- Blob vs Block vs Object Storage
- Data Lakes & Warehouses
## [Scalability Techniques](./scalability-techniques/)
- Horizontal vs Vertical Scaling
- Asynchronous Processing (Workers, Queues)
- Rate Limiting & Throttling
- Designing Unique ID Generators
- Consistent Hashing in Distributed Systems
## [Advanced Infrastructure Patterns](./advanced-infrastructure-patterns/)
- Design a Rate Limiter
- Design an API Gateway
- Feature Flag Systems
- Key-Value Store Design
## [Designing Real-World Systems](./designing-real-world-systems/)
Each includes: Requirements, APIs, Component Breakdown, Bottlenecks, Tradeoffs
- URL Shortener (Bitly)
- Instagram (Posts, Feed, Stories)
- WhatsApp (Messaging, Delivery Receipts)
- Uber (GPS, Matching, Pricing)
- Twitter (Fanout, Timelines)
- YouTube (Upload, Processing, Streaming)
- Google Drive / Dropbox (Cloud Storage)
- E-commerce Platform (Product, Cart, Orders)
- News Feed System
- Notification System (Email, Push, SMS)
- Chat System (WebSockets, Real-Time)
- Search Autocomplete
- Proximity Service / Nearby Friends
- Google Maps
- Real-Time Gaming Leaderboard
- Payment System (Stripe/PayPal)
- Digital Wallet
- Distributed Message Queue
- Metrics Aggregation System
- Hotel Booking Engine
- Distributed Email Service
- Web Crawler
- S3-like Object Store
- Stock Exchange
## [Observability & Monitoring](./observability-monitoring/)
- Metrics Collection (Prometheus, Grafana)
- Log Aggregation (ELK Stack, Loki)
- Distributed Tracing (Jaeger, Zipkin)
- Health Checks & Alerting
## [Security & Authentication](./security-authentication/)
- OAuth2, JWT, Session vs Token
- Data Encryption (At-Rest, In-Transit)
- Secure Design Patterns
- Threat Modeling & Zero Trust
## [Cloud & DevOps](./cloud-devops/)
- Infrastructure as Code (Terraform, Pulumi)
- Blue/Green & Canary Deployments
- Disaster Recovery & Fault Tolerance
- Autoscaling Infrastructure
- Multi-Cloud & Hybrid Cloud
## [Design Tradeoffs & Deep Dives](./design-tradeoffs-deep-dives/)
- Case Study: Instagram Reels Architecture
- Case Study: Netflix Global Infrastructure
- Cost-Aware Architecture
- Multi-Tenancy
- Global-Scale Architecture

---

## Interview Questions
**Grouped by Category**:
### Classic Problems
- Twitter, WhatsApp, Instagram, YouTube, Uber, Dropbox
### Service-Oriented
- Rate Limiter, Notification System, Feature Flags, Logging, Search
### Scalability Challenges
- 1M RPS System, Black Friday Surge, Read-heavy Scale, DB Sharding
### Reliability Engineering
- Disaster Recovery, Partial Failures, Retry Strategies, Fault Injection
### Architecture Decisions
- SQL vs NoSQL, Push vs Pull, Use of Caching/Queue
### Cloud Infrastructure
- AWS Deployments, CDN for Video, Kubernetes Deployment
### Behavioral / Meta
- Debugging Slow APIs, Handling Traffic Spikes, Real System Failures
