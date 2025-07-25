Designing a Real-Time Gaming Leaderboard System
(e.g., Fortnite, Call of Duty, Kahoot, Clash Royale, etc.)

A leaderboard system allows players to see rankings based on real-time game performance. It must be fast, consistent, and scalable for millions of concurrent users.

🧾 Functional Requirements
✅ Core Features
Add/update player scores (per game, mode, region, etc.)

Retrieve leaderboard (top N players, or surrounding user)

Support multiple leaderboards (e.g., global, friends, region)

Reset leaderboards (daily, weekly, seasonally)

🚫 Non-Functional Requirements
Low latency (real-time updates and reads)

High throughput (millions of score updates per minute)

Scalable and distributed

Fault-tolerant and consistent

🎯 System Goals
Property	Goal
Latency	< 100 ms for read/write
Consistency	Strong or eventual (configurable)
Availability	99.9% uptime
Scale	100M+ players
Isolation	Per-region or per-game leaderboards

🧠 High-Level Architecture
plaintext
Copiar
Editar
+------------+       +------------------+       +------------------+
| Game Client|  -->  | Leaderboard API  |  -->  | Redis / Sorted DB |
+------------+       +------------------+       +------------------+
        ↑                     ↓                          ↓
        |                +-------------+         +------------------+
        |                | Write Queue | ----->  | Persistent Store |
        |                +-------------+         | (Postgres, Dynamo)|
        |                                           +----------------+
        ↓
  [Notification Service] (Optional - e.g., notify player enters Top 10)
🧩 Core Components
1. 🔢 Data Model – Score Records
json
Copiar
Editar
{
  "user_id": "player_123",
  "game_mode": "solo",
  "score": 2400,
  "timestamp": 1729891200,
  "region": "NA"
}
Each game/mode/region gets its own sorted set or partition.

2. 🧮 Fast Scoring – Redis Sorted Sets (ZSET)
Store players in Redis ZSETs keyed by leaderboard type:

bash
Copiar
Editar
ZADD leaderboard:solo:NA 2400 player_123
ZADD leaderboard:solo:EU 3000 player_999
Get top 10 players:

bash
Copiar
Editar
ZREVRANGE leaderboard:solo:NA 0 9 WITHSCORES
Get rank of a specific player:

bash
Copiar
Editar
ZREVRANK leaderboard:solo:NA player_123
3. 📦 Persistent Storage (for durability)
Redis is in-memory → periodically sync to persistent DB:

PostgreSQL

DynamoDB

Cassandra

Use for:

Leaderboard backups

Historical stats

Analytics queries

4. 📥 Queueing Layer
Use Kafka, RabbitMQ, or SQS to buffer incoming updates.

Consumers read from queue and update:

Redis for fast ranking

DB for long-term storage

5. 👥 Grouped Leaderboards
Filter by:

Friends (requires a friends service)

Region / server

Clan / guild

Each group is a logical leaderboard with its own keyspace

⚙️ Scaling Techniques
Challenge	Solution
Hot ZSETs	Use region/game/user partitioning
Redis memory limits	Evict stale leaderboards / compress
Score spikes (e.g., tournament)	Use async queues to buffer updates
Massive reads	Use CDN-cached JSON or edge workers

🔐 Security Considerations
Validate updates via signed session tokens

Rate-limit updates (no cheating)

Prevent clients from directly editing scores

Audit logs for admin changes or rollbacks

📊 Monitoring
Metric	Importance
Update rate per second	Detect unusual spikes
Redis memory usage	Prevent eviction
Score skew	Find anomalies or cheaters
Latency of score write	Ensure real-time guarantees

🧠 Interview Tips
“I’d store leaderboard entries in Redis using sorted sets for O(log N) updates and reads. For persistence, I’d write to PostgreSQL asynchronously via Kafka. To scale, I’d shard leaderboards by region and game mode. Friend-based leaderboards would be dynamic sets filtered from the main store using precomputed social graphs.”

✅ Summary Table
Feature	Strategy / Stack
Real-time updates	Redis ZSET
Persistence	PostgreSQL / DynamoDB
Score ingestion	Kafka or RabbitMQ
Grouped boards	Logical partitions per group
Security	Token validation, rate limits
Data export	Batched to analytics pipeline

🔄 Optional Enhancements
Historical leaderboards (per week/season)

Player profile stats aggregation

WebSocket push for live ranking updates

Graph-based friend leaderboard queries