Consistent Hashing in Distributed Systems
“Consistent hashing ensures your system doesn’t panic when a node joins or leaves.”

It’s a key technique in distributed systems to distribute data evenly across servers while minimizing reorganization when nodes are added or removed.

🧠 The Problem It Solves
In basic hashing (e.g. hash(key) % N), when you change the number of nodes (N), almost all keys are remapped — causing massive data reshuffling.

❌ Example:
python
Copiar
Editar
hash("apple") % 3 = 0  → Node A
# Add a node: N becomes 4
hash("apple") % 4 = 2  → Now goes to Node C
💥 This leads to:

Cache misses

Massive data movement

Load imbalance

✅ What Is Consistent Hashing?
It maps both keys and nodes into the same hash space (a circle or ring), so only a small subset of keys get remapped when a node is added or removed.

🔄 How It Works
Hash space is a ring (0–2³² or 0–360°).

Nodes are placed on the ring using a hash function.

Keys are also hashed and placed on the ring.

Each key is stored in the first node clockwise from its position.

🖼️ Text-Based Ring Diagram
less
Copiar
Editar
    360° ──────────────── 0°
      /                   \
Node A                Node D
     |                    |
   Key1              Key2
     \                   /
      Node B —— Node C
Keys fall to the next node clockwise. When a node leaves, only its section is affected.

🔢 Hash Ring Mechanics
Suppose hash space = 0 to 2³²

Node A hashes to position 10,000

Node B hashes to position 40,000

Key X hashes to 20,000 → Goes to Node B

✅ Node Added:
New Node C at 25,000

Only keys in (10,001–25,000] are remapped → minimal disruption

🎯 Virtual Nodes (vNodes)
To ensure better load balancing, each physical node is assigned multiple virtual nodes spread across the ring.

Benefits:
Smooths out uneven key distribution

Reduces hot spots

Makes node removal/addition smoother

🔍 Where It’s Used
Use Case	Description
Distributed caches	Like Memcached, Redis clusters
Load balancing	Hash-based routing to services
Storage systems	Cassandra, DynamoDB use consistent hashing for sharding
Content delivery	Cache/CDN layers route users to nearby edge nodes

🛠️ Tools & Libraries
Language	Library
Go	hashicorp/consistent
Java	ketama, Guava
Python	hash_ring
Node.js	consistent-hashing

⚖️ Trade-offs
Pros	Cons
Minimal data reshuffling	Slightly complex to implement
Great for distributed caching & storage	Requires careful tuning (hash function, vNodes)
Handles scaling events gracefully	May need consistent hash-aware clients

💬 What to Say in Interviews
“To prevent massive key redistribution on node changes, I’d use consistent hashing with virtual nodes. This would ensure stability, even during dynamic scaling.”

“It’s essential for systems like distributed caches or key-value stores where availability and low-latency access are critical.”