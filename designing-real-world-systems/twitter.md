Designing Twitter (Fanout & Timelines)
“Designing Twitter is a classic system design interview challenge. It combines real-time data, high write and read throughput, and massive fan-out operations.”

🧾 Functional Requirements
✅ Core Features:
Users can post tweets (text/media)

Users can follow/unfollow others

Users can see a home timeline (tweets from people they follow)

Users can see a profile timeline (user's own tweets)

Real-time updates for new tweets

Support likes, retweets, and replies (out of scope for this version)

🚫 Non-Functional:
Low latency for timeline loading

High throughput (millions of tweets/day)

Scalability to millions of users

High availability and durability

🎯 System Goals
Category	Goal
Scale	100M+ DAUs, 500M+ tweets/day
Performance	Timelines load under 200ms
Durability	Tweets never lost
Availability	99.99% uptime
Real-time UX	Updates within a second

🧱 High-Level Architecture
sql
Copiar
Editar
          +------------------+
          |  User Service    |
          +------------------+
                   |
+------+     +-------------+     +--------------+
| Auth |<--->| Tweet Store |<--->| Timeline DB  |
+------+     +-------------+     +--------------+
                   |                    |
                   v                    v
          +------------------+   +----------------+
          | Fanout Service   |   | Pull Timeline  |
          +------------------+   +----------------+
                   |
              +-----------+
              | Kafka Bus |
              +-----------+
🧠 Key Components
1. User Service
Manages user accounts and follow relationships

Stores follow graph (who follows whom)

sql
Copiar
Editar
followers(user_id, follower_id)
2. Tweet Store
Stores all tweets (text/media) in append-only DB

Tweets are immutable (easy to cache and shard)

sql
Copiar
Editar
tweets(tweet_id, user_id, content, timestamp)
3. Timeline Store
Stores precomputed timelines per user

Each timeline is a list of tweet IDs

sql
Copiar
Editar
timelines(user_id, [tweet_id1, tweet_id2, ...])
🌀 Fanout Strategy
A. Fanout on Write
When User A tweets, system pushes tweet ID to timelines of all A’s followers

Pros:

Fast read (timeline is ready)

Great for read-heavy workloads

Cons:

Expensive if A has millions of followers

Risk of write amplification

B. Fanout on Read
When User B loads timeline, system pulls latest tweets from people they follow

Pros:

Cheap write

Flexible for users with many followers

Cons:

Expensive and slow read

Heavy DB queries

Hybrid Model (Recommended):
Use fanout on write for users with < X followers (say 10,000)

Use fanout on read for celebrities (X > 10,000 followers)

Cache hot timelines in Redis

🧰 Tweet Posting Flow
vbnet
Copiar
Editar
1. User A posts a tweet → Tweet stored
2. Fanout Service fetches A’s followers
3. Pushes tweet ID to each follower's timeline
4. Timeline is stored in Timeline DB
5. Kafka logs each fanout event for auditing
📥 Timeline Read Flow
markdown
Copiar
Editar
1. User B opens app → requests home timeline
2. Timeline Service fetches B's timeline (list of tweet IDs)
3. Tweets retrieved in batch from Tweet Store
4. Sorted and returned with timestamps
🗃️ Timeline DB Options
Use Case	DB Type
Tweet Store	Cassandra, DynamoDB, Bigtable
Timeline Storage	Redis, ScyllaDB, Elasticsearch
Follower Graph	Neo4j, SQL (index-heavy)

📦 Caching
Redis used for hot timelines (celebs, active users)

Tweets cached using tweet ID as key

Use LRU + TTL to keep fresh

📈 Scaling Considerations
Challenge	Solution
Write Amplification	Use hybrid fanout strategy
Celebrity tweets	Fanout-on-read + CDN caching
Timeline freshness	Kafka stream + background workers
Global availability	Geo-sharding timelines
Hot timelines (Trump)	Pre-warm Redis, use CDN fallback

🔐 Security & Privacy
Auth via JWT or OAuth2

Only show tweets from followed users (except public profiles)

Apply rate limits on post/refresh

📊 Metrics to Monitor
Tweet latency (post → visible)

Fanout time (per 1K followers)

Timeline load latency

Cache hit/miss ratio

Timeline freshness (last tweet age)

🧰 Sample Tech Stack
Component	Tools
Queue	Kafka, RabbitMQ
Datastore	Cassandra, ScyllaDB
Cache	Redis
Infra	Kubernetes, Terraform
Monitoring	Prometheus, Grafana, Jaeger

🎙️ What to Say in Interviews
“I’d design Twitter with a hybrid fanout model. For most users, I’d push tweets to followers’ timelines at write time. For celebrity users, I’d pull tweets at read time to avoid massive write overhead. I’d store timelines in a NoSQL DB like ScyllaDB and cache hot data in Redis.”

✅ Summary Table
Feature	Design Choice
Fanout strategy	Hybrid (push for regular, pull for celebs)
Tweet storage	Append-only NoSQL
Timeline DB	Sharded Redis + persistent NoSQL
Follower graph	Indexed SQL or Graph DB
Caching	Redis + CDN fallback
Queue system	Kafka for async fanout