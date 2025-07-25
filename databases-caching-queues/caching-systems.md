Caching Systems: Redis vs Memcached
“A cache hit is the cheapest performance boost you can get.”

In large-scale systems, caching improves speed and reduces load on databases or services. Two popular open-source tools are Redis and Memcached — each with strengths and trade-offs.

⚡ What Is Caching?
Caching is the practice of storing frequently accessed data in fast, in-memory storage so you can avoid expensive computations or database reads.

🧱 Where Caching Is Used (System Diagram)
text
Copiar
Editar
[Client]
   ↓
[Cache Layer] ← fast path (Redis/Memcached)
   ↓
[Backend Service]
   ↓
[Database] ← slow path
If the cache has the data = ✅ cache hit
If not = ❌ cache miss → fetch from DB → store in cache

🔁 Common Caching Strategies
Strategy	Description
Write-Through	Data is written to cache + DB simultaneously
Write-Behind	Data is written to cache, DB is updated later
Cache Aside	App checks cache first, if miss → DB → cache
Read-Through	Cache fetches from DB automatically on miss

✅ Most real-world systems use cache-aside due to simplicity.

🧠 Redis vs Memcached – Key Comparison
Feature	Redis	Memcached
Data Structure Support	Rich (lists, sets, hashes, etc.)	Simple key-value only
Persistence	✅ Optional disk persistence	❌ In-memory only
Replication & Clustering	✅ Yes (master-replica, sentinel)	⚠️ Limited
Pub/Sub Support	✅ Yes	❌ No
TTL per Key	✅ Yes	✅ Yes
Data Eviction Policies	✅ Multiple (LRU, LFU, TTL)	✅ LRU, TTL only
Memory Efficiency	⚠️ Slightly heavier	✅ Lightweight
Language	Written in C	Written in C

🔧 When to Use Redis vs Memcached
Use Case	Recommended Tool
Session store, leaderboard	Redis
Simple caching (HTML, query)	Memcached
Counters, queues, sets	Redis
Real-time analytics	Redis
Large ephemeral objects	Memcached
Chat pub/sub or streaming	Redis

🧪 Example: Redis as a Leaderboard
Redis supports sorted sets, which makes it ideal for tracking top scores.

bash
Copiar
Editar
ZADD leaderboard 1000 "Alice"
ZADD leaderboard 950 "Bob"
ZRANGE leaderboard 0 -1 WITHSCORES
You can't do this in Memcached — it's key-value only.

🔐 Expiration and Eviction
Both tools support TTL (Time-To-Live):

bash
Copiar
Editar
SET user:123 "John" EX 60  # expires in 60s
Eviction when memory is full:

LRU (Least Recently Used)

LFU (Least Frequently Used) – Redis only

TTL-based

No eviction (fail if full)

💬 What to Say in Interviews
“I use Redis when I need advanced data types, durability, and features like pub/sub or sorted sets. For lightweight, read-heavy caching of flat objects, Memcached is a simpler fit.”

“Most of the time, I go with cache-aside strategy — it's simple, and puts cache control in the app layer.”

⚠️ Common Pitfalls
Mistake	Solution
Stale data after DB update	Use write-through or invalidate
Cache stampede (thundering herd)	Use locks, async refresh
Cache inconsistency	TTL + regular invalidation
Over-caching everything	Cache only what’s reused often

✅ Summary Table
Feature	Redis	Memcached
Data Types	✅ Advanced (sets, lists)	❌ Basic only
Persistence	✅ Optional (AOF, RDB)	❌ None
Use Cases	Real-time, session, counters	Page cache, API response
Scale	Clustering, Sentinel	Limited horizontal scaling
TTL / Eviction	LRU, LFU, TTL	LRU, TTL

🏁 Final Takeaway
"Caching is not an afterthought — it’s a core part of system design."

Use Redis when you need power. Use Memcached when you need speed and simplicity.