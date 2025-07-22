Back-of-the-Envelope Estimation
“You don’t need exact numbers. You need good guesses—fast.”

Back-of-the-envelope (BOTE) estimation is a quick way to get rough numbers on scale, storage, bandwidth, and capacity. It’s a must-have skill in system design interviews and in early-stage architecture planning.

This technique helps you answer:

“How much data are we dealing with?”

“Can this system handle 10 million users?”

“Do we need a CDN, cache, or a queue?”

🧭 Why It Matters
Interviewers want to see:
✅ You can make realistic assumptions
✅ You’re thinking like an architect
✅ You can justify your decisions

💬 You’re not graded on precision—but on your approach.

🧮 Common Estimation Inputs
Element	Typical Assumption
Request Size	1 KB – 100 KB (JSON API, static HTML)
File Upload	1–10 MB per upload (images, PDFs)
Database Row	100–1000 bytes
Daily Active Users	1% of total registered users
Average Requests	5–20 requests/user/day
Cache Hit Ratio	70%–90%
CDN Offload	50%–90% of static content delivery

🧪 Estimation Example: Designing a Video Platform
📌 Problem:
Estimate bandwidth and storage for a YouTube-like app.

Step 1: Define Assumptions
1M daily active users

Each user watches 5 videos/day

Average video size: 50MB

Video length: 5 minutes

Uploads: 100K new videos/day

Retention: 5 years

Step 2: Estimate Bandwidth
bash
Copiar
Editar
Total videos watched/day = 1M × 5 = 5M
Bandwidth/day = 5M × 50MB = 250,000,000 MB = ~238 TB/day
Peak bandwidth = (5M views / 24h / 3600s) × 50MB ≈ ~2.9 GB/sec
Step 3: Estimate Storage
java
Copiar
Editar
Daily uploads = 100K × 50MB = ~4.7 TB/day
5 years of uploads = 4.7 TB × 365 × 5 = ~8.5 PB (Petabytes)
Step 4: Estimate Requests Per Second (QPS)
bash
Copiar
Editar
Assume 1M DAU, 10 requests/day → 10M requests/day
QPS = 10M / (24 × 60 × 60) ≈ 115 QPS
🔢 Handy Cheat Table
Task	Estimation Formula
QPS	Requests/day ÷ 86,400
Bandwidth/day	Requests/day × Payload Size
Storage/day	Writes/day × Size per write
Total Storage	Storage/day × Retention Days
Peak Concurrency	1–5% of DAU (rule of thumb)

💡 Tips to Succeed in Interviews
✅ Say your assumptions aloud
✅ Estimate with round numbers (e.g., 1M users, 100 bytes)
✅ Justify decisions based on output (e.g., “that’s too much for a single DB”)
✅ Use BOTE to decide between cache, queue, batch, async, etc.

🔁 Reuse This Process
BOTE estimation can help you:

Pick between SQL or NoSQL

Decide whether to shard or not

Justify caching or CDN

Calculate latency impacts of global vs regional infra

🏁 Final Thought
“Architects don’t guess. They estimate.”

Knowing how to estimate fast and confidently turns your design into an engineering story, not just a diagram.

