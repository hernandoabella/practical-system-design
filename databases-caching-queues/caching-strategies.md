Caching Strategies: Write-through, TTL, Lazy Loading
“Caching isn't just about storing data — it’s about knowing when and how to fetch, update, and expire it.”

Different caching strategies help balance performance, consistency, and freshness. Choosing the right strategy depends on how frequently your data changes, how critical consistency is, and how much latency you can tolerate.

🔍 Why Caching Strategy Matters
Caching reduces latency and offloads backends, but:

If the cache isn’t updated properly → stale data

If it misses often → waste of memory

If it's overused → hard to maintain consistency

Let’s break down the core strategies:

📌 1. Write-Through Cache
How it works: Every time data is written to the database, it's also written to the cache immediately.

text
Copiar
Editar
[Client] → [App] → [DB]
                    ↘
                    [Cache]
✅ Pros:
Data in cache is always fresh

Simplifies reads — cache always has the latest value

⚠️ Cons:
Slower writes (write to both cache and DB)

Might cache data that’s never read

🔧 Use When:
Strong consistency is critical (e.g., financial data)

Reads heavily follow writes

📌 2. Write-Behind (Write-Back) Cache
How it works: Write is done to cache first. Then it's asynchronously flushed to the database.

text
Copiar
Editar
[Client] → [Cache]
            ↘
            [Async Flush → DB]
✅ Pros:
Fast writes

DB is offloaded significantly

⚠️ Cons:
Risk of data loss if cache crashes before flushing

Complex to manage

🔧 Use When:
Low write latency is key

Eventual consistency is acceptable

📌 3. Cache-Aside (Lazy Loading)
How it works: The application checks the cache first. If it’s not there (a miss), it loads data from the database and stores it in the cache.

text
Copiar
Editar
[Client] → [Cache?]
     ↓ Miss       ↘ Hit
    [DB] → [Cache]
✅ Pros:
Simple & efficient — only caches data that is actually used

Cache warm-up happens automatically

⚠️ Cons:
First access is slow (miss penalty)

Might result in cache stampede under heavy load

🔧 Use When:
Read-heavy systems

You want control over what is cached

📌 4. Read-Through Cache
How it works: Like cache-aside, but the cache automatically loads data from the DB when a miss occurs — app doesn’t talk to DB directly.

text
Copiar
Editar
[Client] → [Cache]
               ↘ Miss → [DB → Cache]
✅ Pros:
Cleaner abstraction (app doesn't manage cache logic)

Keeps cache logic centralized

⚠️ Cons:
More complex to implement

Less flexibility than cache-aside

⏳ 5. TTL (Time-To-Live)
TTL is used in all caching strategies to automatically expire keys.

bash
Copiar
Editar
SET user:123 "John" EX 300  # Expires in 5 minutes
✅ Benefits:
Automatically prevents stale data

Controls memory usage

⚠️ Challenges:
Choosing the right TTL is hard

Too short = more cache misses

Too long = stale data

💥 Bonus: Eviction Policies
When memory is full, caches evict keys:

Policy	Description
LRU	Least Recently Used
LFU	Least Frequently Used (Redis only)
Random	Evict a random key
TTL	Based on expiration time

🧠 Strategy Comparison
Strategy	Write Speed	Read Speed	Complexity	Consistency	Notes
Write-Through	Slow	Fast	Medium	Strong	Always updated cache
Write-Behind	Fast	Fast	High	Weak	Risk of data loss
Cache-Aside	Medium	Fast	Low	Eventual	App manages miss logic
Read-Through	Medium	Fast	Medium	Eventual	Abstracts cache from app
TTL	N/A	Fast	Low	Weak	Best for expiring stale data

💬 What to Say in Interviews
“In most systems, I go with cache-aside because it’s simple and efficient. I control what gets cached and when. I combine it with TTL to avoid stale data.”

“For systems that need strong consistency, I’d use write-through or read-through caching. Write-through ensures that the cache always reflects the latest DB state.”

🛠️ Real-World Use Cases
Scenario	Recommended Strategy
User sessions / JWT tokens	Write-through + TTL
Product catalog / Search results	Cache-aside + TTL
Real-time game scores	Write-behind
Payment processing	Write-through
Config values / feature flags	Read-through

🏁 Final Takeaway
“Caching strategy isn't one-size-fits-all. It’s a design decision that depends on access patterns, consistency needs, and failure tolerance.”