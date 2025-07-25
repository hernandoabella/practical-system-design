Database Sharding & Replication
“You scale reads with replication and writes with sharding.”

Both replication and sharding are foundational techniques for building scalable, reliable, and high-performance data systems.

🔄 What Is Replication?
Replication is the process of copying the same data to multiple servers.

🔧 Common Use Cases:
High availability

Disaster recovery

Read scaling

🧩 Types of Replication
Type	Description
Master–Slave	One primary node handles writes; replicas serve reads
Multi-Master	Multiple nodes accept writes (conflict resolution required)
Synchronous	Waits for replica write confirmation before success
Asynchronous	Primary doesn't wait for replicas — higher performance, possible lag

📤 What Is Sharding?
Sharding is breaking large datasets into smaller chunks (shards), distributed across servers.
Each shard holds only part of the data.

🔧 Common Use Cases:
Horizontal scaling

Write throughput

Storage scalability

🔍 Example of Sharding
Let’s say you have 3 shards, and you store user data based on user ID:

text
Copiar
Editar
Shard 1 → user_id % 3 == 0
Shard 2 → user_id % 3 == 1
Shard 3 → user_id % 3 == 2
Each shard only holds a subset of the entire dataset.

🧠 Combining Sharding and Replication
In real-world systems, you often shard your data and then replicate each shard:

text
Copiar
Editar
        +------------------+
        |   Query Router   |
        +--------+---------+
                 |
       +---------+---------+---------+
       |                   |         |
   [Shard 1]           [Shard 2]   [Shard 3]
    /     \              /    \      /    \
 [R1]    [R2]         [R1]  [R2]   [R1]  [R2]
Each shard is replicated for fault tolerance.

📊 Comparison Table
Feature	Replication	Sharding
Purpose	High availability, fault-tolerance	Scale storage & writes
Data Type	Same data on each node	Partitioned data per shard
Scaling Type	Read scalability	Write & storage scalability
Complexity	Low to moderate	High
Consistency Challenges	Read-after-write lag	Cross-shard joins, hotspotting

✅ When to Use
Scenario	Technique
Heavy read traffic	✅ Replication
Need for redundancy & uptime	✅ Replication
Write-heavy workloads	✅ Sharding
Large-scale data (10M+ users)	✅ Sharding
Both read & write scaling needed	✅ Both

💬 What to Say in Interviews
“I’d replicate my database to scale read-heavy traffic and ensure fault tolerance. For high write throughput, I’d shard by a stable key like user ID. This also helps isolate failures and balance load.”

“Sharding and replication aren't mutually exclusive — they’re complementary.”

⚠️ Challenges
Challenge	Mitigation Strategy
Replication lag (eventual consistency)	Use synchronous replication or read from primary
Shard hotspots (uneven load)	Use consistent hashing
Cross-shard joins	Denormalize or avoid cross-shard queries
Resharding complexity	Plan key design ahead, use routing layers

🏁 Final Takeaway
“Think of replication as data duplication for fault tolerance and performance.
Think of sharding as data division for horizontal scaling.”

Both are essential for real-world, production-grade systems.

