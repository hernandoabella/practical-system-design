Designing Instagram (Posts, Feed, Stories)
“Instagram isn’t just photo sharing — it’s a real-time, feed-driven, scalable social platform used by billions. Let's design its core.”

🧾 Functional Requirements
✅ Core Features:
Users can create accounts, follow others

Post photos/videos with captions

See feed of posts from followed users

Upload and view Stories (auto-expiring content)

Like, comment, and bookmark posts

🚫 Non-Functional:
Low latency feed

High availability (99.99%)

Read-heavy system (Feed, Stories)

Ability to scale to 1B+ users

Content delivery optimized for mobile

🧠 High-Level Architecture
pgsql
Copiar
Editar
             +----------+
    +------> |  CDN     | <-----------+
    |        +----------+            |
    |                               \|/
+--------+   +------------+   +----------------+     +---------------+
| Client |<->| API Server |<->| Feed Generator |<--> | Post Service  |
+--------+   +------------+   +----------------+     +---------------+
                              |                  \   /   +-----------+
                              +--> User Service  -->+----> Story DB  |
                              |                  /        +-----------+
                              +--> Notification Service
🧱 Core Components Breakdown
1. User Service
Handles registration, login (JWT or OAuth)

Stores user profile, follower/following relationships

2. Post Service
Handles uploads (metadata, media links)

Stores post info in a DB (PostgreSQL / Cassandra)

Images/videos sent to object storage (S3, GCS)

sql
Copiar
Editar
posts(post_id, user_id, caption, media_url, timestamp)
3. Feed Generator
Two strategies:
Strategy	Description	Used By
Push	Precompute feed on write	Facebook
Pull	Fetch posts from followees on read	Twitter

👉 Instagram uses hybrid: active users = push, inactive = pull

Store feed in a cache (Redis) or DB (Cassandra, Elasticsearch)

TTL for stories = 24h

4. Story Service
Similar to post service but with expiration logic

Background job deletes expired stories

View counters & interaction tracking

🖼️ Media Storage
Media uploaded to CDN-backed object storage

Images resized in background using worker queue

Videos processed (thumbnails, compression)

lua
Copiar
Editar
+----------+
| S3/GCS   |
+----------+
    |
    +---> Cloudflare / CDN
⚡ Scaling the Feed System
Challenge	Solution
High write load	Use Kafka + worker pools for feed push
Real-time feed	Use long polling / WebSocket / SSE
Massive reads	Precompute feed, cache with Redis
Hot users	Shard writes, use async fanout

🧮 Follower Graph Modeling
Each user has a followers table:

sql
Copiar
Editar
followers(user_id, follower_id)
Use graph DB (like Neo4j) or scalable relational model

Pre-calculate mutual followers, friend suggestions

🧪 Sample Flow: Uploading a Post
User uploads a photo

API saves metadata to Post DB

Media stored in S3 → CDN

Feed Generator fans out to followers (store post ref)

Notification service sends alerts to tagged users

🔔 Notification System
Real-time alerts for:

New likes/comments

New followers

Mentioned in a post/story

Use Kafka + async workers to push notifications

🔐 Authentication & Privacy
Use JWT for session management

Per-post privacy (public/private)

Blocked users can’t view/interact

Story viewers tracked and limited

📊 Analytics & Tracking
View counts, likes, impressions stored in analytics DB

Use Kafka for ingestion → Data Lake → Dashboards

🛠️ Tech Stack Suggestions
Component	Technology
Backend	Node.js, Go, Python
DB	PostgreSQL, Cassandra
Feed Cache	Redis, Elasticache
Media Storage	S3 + Cloudflare CDN
Messaging	Kafka, RabbitMQ
Real-Time	WebSockets/SSE
Infra	Kubernetes, Terraform

🧠 Interview Tips
“For a feed-based system like Instagram, I'd use a hybrid push-pull feed model. Real-time interactions require cache and async messaging. Media is stored in S3 and served via CDN to reduce latency. Each component must scale independently.”

📘 Summary Table
Feature	Design Approach
Posts	Stored in DB, media in S3/CDN
Stories	TTL-based expiry, stored in Story DB
Feed	Hybrid model: push for active, pull for rest
Real-time	WebSockets / Long Polling
Scale	Kafka, Redis, sharded DBs
Security	JWT, privacy settings, blocking logic
Analytics	Kafka → Data Warehouse