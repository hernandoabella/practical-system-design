## Scalability, Availability, and Reliability
“These are the pillars of system design—if one falls short, users feel it.”

🧠 Overview
Every modern system must be able to grow, stay online, and recover from failure. These three core concepts—Scalability, Availability, and Reliability—guide nearly every architectural decision in system design.

Let’s break them down and see how they influence real-world systems.

1️⃣ Scalability
Scalability is a system’s ability to handle increased load—more users, more data, more traffic—without a drop in performance.

Types of Scalability:
Vertical Scaling (Scale Up): Add more power to a single server (e.g., more RAM, CPU).

✅ Simple to implement

❌ Expensive and limited

Horizontal Scaling (Scale Out): Add more servers and distribute the load.

✅ Cost-effective at scale

✅ Enables fault tolerance

❌ Requires more complex architecture (load balancing, partitioning, etc.)

Real Example:
Instagram feed: needs to scale horizontally to serve millions of users at peak times without slowing down.

2️⃣ Availability
Availability refers to a system’s ability to remain accessible and operational at all times, even during failures or updates.

Key Terms:
High Availability (HA): Design strategies to minimize downtime, usually measured in 9s (e.g., 99.9% = ~9 hours/year downtime).

Redundancy: Having backup components (servers, DB replicas, etc.).

Failover: Automatic switch to a backup when primary fails.

Health Checks: Monitor components and remove unhealthy ones from the pool.

Real Example:
Netflix uses multi-region deployments and redundancy so that even if a data center goes down, users don’t notice.

3️⃣ Reliability
Reliability is the system’s ability to perform consistently and correctly over time—not just up, but working as expected.

Characteristics of Reliable Systems:
Resilience to failures (e.g., retries, circuit breakers)

Data integrity and correctness

Graceful degradation: When part of the system fails, the rest still works

Monitoring and alerting: Detect problems before users do

Reliability ≠ Availability:
A system might be available but unreliable (e.g., returns wrong data or crashes intermittently).

Real Example:
Banking systems must be extremely reliable—losing or corrupting transaction data is unacceptable.

🧩 How They Interact
Tradeoff	Example
Scalability vs Consistency	Large-scale systems often sacrifice strong consistency for partition tolerance (CAP Theorem).
Availability vs Cost	99.999% uptime is expensive—evaluate business needs vs cost of failure.
Reliability vs Complexity	Adding retries, backups, failovers adds reliability but also architectural complexity.

✅ Summary
Concept	Definition	Real Goal
Scalability	Handle growth without performance loss	Scale out (or up) seamlessly
Availability	System is up and reachable	Minimize downtime
Reliability	System performs correctly and predictably	Ensure trust in data and behavior
