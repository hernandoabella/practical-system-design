Defining Bottlenecks & Tradeoffs in System Design
“Every design is a set of tradeoffs—there’s no perfect solution, only informed decisions.”

A key skill in system design is identifying where your system may struggle under load and what compromises you're willing to make to meet business goals, user needs, and engineering constraints.

🔍 What Is a Bottleneck?
A bottleneck is a component or process that limits the overall system’s performance, scale, or availability.

Think: “If everything else is fast, what’s the one thing slowing it all down?”

🚨 Common Bottlenecks in Systems
Layer	Bottleneck Example	Cause
Database	Slow queries, lock contention	Poor indexing, high write load
Network	High latency or packet loss	Region distance, traffic spikes
Cache Layer	Low hit ratio	Bad eviction policy or cold start
API Gateway	Overloaded from traffic bursts	No rate limiting or throttling
Disk I/O	Slow file reads/writes	Saturated volume or SSD limits
Message Queue	Queue backlog delays processing	Consumers too slow
Authentication Layer	Token verification delay	JWT decoding or DB checks

⚙️ Tradeoffs in System Design
Every system is a balancing act across several dimensions:

Tradeoff	Choices	Example Decision
Consistency vs Availability	Strong consistency or availability under failure?	Eventual consistency in chat timestamps
Latency vs Throughput	Fast response or bulk processing?	Stream video in chunks vs batch download
Durability vs Performance	Sync every write or cache in memory first?	Write-behind caching for better speed
Cost vs Scalability	Cheap infra or easily scalable infra?	Use S3 instead of block storage
Simplicity vs Flexibility	Easy-to-build or easy-to-extend?	Monolith for MVP, microservices later
Developer Velocity vs Safety	Release faster or reduce bugs?	Use feature flags + canary deployments

🧠 How to Identify Bottlenecks During Design
Ask yourself:

🔸 What part of the system gets called the most?

🔸 Where do I perform heavy computation?

🔸 What if one component fails? What breaks first?

🔸 What layer depends on external services or APIs?

🧪 Case Example: Messaging App
Scenario: You’re designing WhatsApp backend.

Component	Bottleneck Risk	Mitigation Strategy
Message storage DB	Write-heavy workload	Use write-optimized DB (e.g., Cassandra)
Delivery system	High fan-out for group messages	Use pub/sub model (e.g., Kafka)
Read receipts	Extra DB updates per message	Batch updates or async processing
Push notifications	Limited by platform rate limits	Throttle, retry, queue delivery

✅ Tradeoff Matrix Template
text
Copiar
Editar
┌─────────────────────────────┐
│      TRADEOFF MATRIX        │
├─────────────┬───────────────┤
│ Factor      │ Consideration │
├─────────────┼───────────────┤
│ Consistency │ Strong / Eventual?            │
│ Latency     │ Sub-200ms?                     │
│ Availability│ 99.9% or 99.999%?              │
│ Cost        │ Free tier or enterprise-grade? │
│ Dev speed   │ MVP or production ready?       │
└─────────────┴───────────────┘
Use this during interviews to demonstrate your thinking process.

💬 Phrases to Use in Interviews
“This component might become a bottleneck at scale, so let’s isolate it behind a queue.”

“There’s a tradeoff here between latency and cost, and I’d optimize based on usage patterns.”

“We could favor availability over consistency since it's a feed, not a transaction system.”

🔁 Summary
Concept	Description
Bottlenecks	Points where system slows or fails under load
Tradeoffs	Decisions between competing system qualities
Why it matters	Drives realistic, resilient, cost-effective design

🏁 Final Thought
“Your system is only as strong as its weakest point. Find it—and fix it before it breaks.”