Latency vs. Throughput
“Fast isn’t the same as scalable. That’s why latency and throughput are different.”

Understanding latency and throughput is essential for building performant and scalable systems. They measure different aspects of performance and are often misunderstood or used interchangeably.

🧠 Definitions
Term	Description
Latency	The time it takes to process a single request from start to finish.
Throughput	The number of requests processed per unit time (e.g., RPS or QPS).

🔄 Real-Life Analogy
Imagine a coffee shop:

Latency: How long it takes to serve one customer

Throughput: How many customers you can serve in 1 hour

You can have:

Low latency but low throughput (fast service, only 1 barista)

High throughput but high latency (many baristas, longer lines)

Optimized both (many baristas, fast service)

📊 Quick Comparison Table
Metric	Latency	Throughput
Unit	Milliseconds (ms)	Requests per second (RPS/QPS)
Focus	Individual experience	System-wide performance
Goal	Reduce response time	Maximize processed volume
Example Tool	Ping, curl, response timers	Load testing tools (k6, JMeter)
Bottleneck	Slow computation, network delay	Thread pool size, CPU limits
Optimization	Caching, prefetching, compression	Queues, batching, parallelism

🎯 When to Optimize for Each
Scenario	Optimize For	Why
Real-time video conferencing	Latency	Every delay impacts user experience
Bulk email sender	Throughput	You can send slower, but need to handle volume
User login/authentication	Latency	Must feel instant
Analytics event processor	Throughput	Can process in batches or eventually
Online checkout system	Both	Users expect speed, business needs scale

🧪 Sample Use Case: E-Commerce Checkout
Layer	Latency Focus	Throughput Focus
Web frontend	✅ Reduce user wait	❌ Not a bottleneck
Backend API	✅ Faster responses	✅ Handle flash sale spikes
Payment gateway	✅ Must be fast	✅ Needs scaling for events
Inventory DB	✅ Real-time stock	✅ Many concurrent buyers

🔀 Tradeoffs in System Design
Sometimes you have to choose between low latency or high throughput:

Caching improves latency, but may not help with throughput bottlenecks

Queues increase throughput via async processing, but add latency

Batch processing increases throughput, but sacrifices immediacy

⚠️ Design systems with SLA-based tradeoffs: "95% of requests under 200ms, 10K RPS"

📏 Measuring Tools
Metric	Tool	Notes
Latency	Postman, curl, Datadog	Track average, p95, p99
Throughput	Apache Bench, k6, JMeter	Simulate load, requests per second

✅ Summary
Concept	Latency	Throughput
Definition	Time per request	Requests per time unit
Goal	Speed up each user experience	Handle more requests efficiently
Metric	ms, p95, p99	RPS, TPS, QPS
Tradeoff	More work per request increases latency	Too many requests may throttle throughput

💬 What to Say in Interviews
“I’d optimize for latency to make the UX feel responsive, but also build in throughput scalability to survive a product launch spike.”

“We’ll decouple components with a queue. It adds latency, but improves throughput and fault tolerance.”

🏁 Final Thought
“Latency is user pain. Throughput is business gain. Design for both—but know when to choose.”