Estimating Load & Traffic
“Before you design a system, know what it’s expected to handle.”

Estimating load is one of the most critical and overlooked steps in system design. It helps you choose the right architecture, databases, caching strategies, and scaling approach.

In interviews and real-world planning, showing that you can roughly calculate system load signals maturity and foresight.

🧮 Why Estimate Load?
Helps identify bottlenecks early

Guides database capacity, cache sizing, and queue depth

Informs decisions on horizontal vs vertical scaling

Makes your design more realistic and measurable

🛠 Common Metrics to Estimate
Here’s a breakdown of what to estimate and why:

pgsql
Copiar
Editar
┌────────────────────┐
│ 1. QPS (Queries/sec)│
└────────────────────┘
- How many API requests per second?
- e.g., 100M requests/day ≈ ~1,157 QPS

┌────────────────────┐
│ 2. Read/Write Ratio │
└────────────────────┘
- Are users mostly reading (browsing), or writing (posting)?
- Common ratios: 80/20 or 90/10

┌────────────────────────┐
│ 3. Storage Requirements │
└────────────────────────┘
- Average data per user? File size? Message size?
- Multiply by # of users to estimate total storage

┌──────────────────────┐
│ 4. Bandwidth Usage    │
└──────────────────────┘
- How much data is transferred per second?
- (QPS × avg response size)

┌──────────────────────┐
│ 5. Concurrent Users   │
└──────────────────────┘
- Peak traffic: how many active users at once?
- Rule of thumb: ~1% of total users online at once

┌────────────────────────────┐
│ 6. Daily Active Users (DAU)│
└────────────────────────────┘
- How many users use the system per day?
- Important for cache hit rates & background jobs

┌────────────────────────────┐
│ 7. Data Retention Duration │
└────────────────────────────┘
- How long do we store user data?
- Affects storage, backup, compliance
📊 Sample Estimation (URL Shortener)
Suppose you're building a system like Bitly.

Metric	Assumption	Result
Daily requests	100 million	~1,157 QPS
Average URL size	100 bytes	~10 GB new data per day
Read/write ratio	90% reads, 10% writes	Focus on read path performance
Retention duration	5 years	Plan for ~18 TB total storage
Peak users	1% of 1B users = 10M online	High concurrency readiness

🧠 Estimations don’t have to be exact—ballpark is better than nothing.

⚙️ Use These Equations
text
Copiar
Editar
QPS = (Total Requests per Day) / (24 × 60 × 60)
Storage = (Data per User × # of Users × Retention Period)
Bandwidth/sec = QPS × Avg Payload Size
You can sketch these out during your interview or whiteboarding session to show your process.

🚨 Estimation Traps to Avoid
❌ Ignoring peak load (always design for spikes, not averages)

❌ Forgetting uploads/downloads in bandwidth

❌ Not considering data growth over time

❌ Assuming 100% cache hit (never happens)

✅ Estimation Checklist
Metric	Covered?
Daily Users	✅
Peak Users	✅
Read/Write Ratio	✅
Storage Size	✅
Bandwidth	✅
QPS	✅
Retention Period	✅

🧪 Tip for Interviews
“Even if you don't know the exact numbers, say your assumptions out loud.”

Example:

"Let’s assume 10M daily users, and each user sends 5 requests per day—so that’s 50M requests/day, or about ~578 QPS."

This shows confidence, reasoning, and real-world thinking.