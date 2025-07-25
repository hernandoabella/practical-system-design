Designing a News Feed System
(e.g., Facebook, Twitter, LinkedIn, Instagram feed)

A news feed system dynamically delivers personalized, ordered content to users, pulling from a large dataset of user-generated content, relationships, and activities. It must be fast, scalable, and personalized — even with billions of posts and users.

🧾 Functional Requirements
✅ Core Features
Show personalized feed (based on follows, likes, trends)

Infinite scroll or pagination

New content visibility (near real-time)

Reactions (like, comment, share)

Content types: text, images, videos, links

🚫 Non-Functional Requirements
Low latency (< 300ms per feed load)

Scalability (support millions of users/posts)

High availability

Strong consistency for recent posts and likes

Content moderation capability

🎯 System Goals
Property	Target
Latency	< 200–300ms for feed rendering
Scalability	1B+ users and 10B+ posts
Feed freshness	Near real-time
Personalization	Based on user activity, interests, social graph
Write-heavy	Must handle massive post ingestion

🧠 High-Level Architecture
plaintext
Copiar
Editar
                    +-------------+
                    |  Frontend   |
                    +-------------+
                          |
                    +-------------+
                    | API Gateway |
                    +-------------+
                          |
             +------------+-------------+
             |                          |
     +-------v------+          +--------v--------+
     | Feed Service |          |   Post Service  |
     +-------+------+          +--------+--------+
             |                          |
     +-------v--------+       +--------v--------+
     | Timeline Cache |       |    Post DB      |
     |  (Redis/MemDB) |       | (NoSQL / SQL)   |
     +----------------+       +-----------------+
🧱 Key Components
1. Post Service
Handles new post creation and storage

Content stored in blob storage (if images/videos)

Metadata (post ID, user ID, timestamp, likes) in DB

json
Copiar
Editar
{
  "post_id": "p123",
  "user_id": "u456",
  "type": "text",
  "text": "Just learned system design!",
  "likes": 104,
  "created_at": "2025-07-19T09:12:00"
}
2. Feed Generation Models
Model	Description
Pull Model	Query posts from followed users in real time
Push Model	Precompute and store each user’s feed (timeline)
Hybrid	Push for heavy users, Pull for light users

3. Timeline Service
Fetches a list of post IDs from followed users

Sorts by:

Recency

Engagement (likes/comments)

Personalized ranking score

Cached in Redis, updated via fanout on new posts

text
Copiar
Editar
User Timeline (user: u456)
[
  post:p989, post:p1052, post:p1430, ...
]
🌀 Fanout-on-Write vs Fanout-on-Read
Strategy	Fanout-on-Write	Fanout-on-Read
Description	Push post IDs to followers’ feed	Read from author’s posts at runtime
Pros	Fast feed load	No unnecessary storage
Cons	Expensive writes	Slow feed reads
When to Use	For active users	For light/occasional users

🔁 Feed Ranking
Factors:
Recency of post

Engagement (likes/comments)

Relationship strength

Machine Learning score (optional)

User preferences/interests

Example:
python
Copiar
Editar
score = 0.4 * likes + 0.3 * recency + 0.3 * affinity
💾 Data Storage
Data Type	DB Choice
Posts	SQL/NoSQL (Cassandra, MongoDB)
Media (images)	Object storage (S3, GCS)
User timelines	Redis or Memcached
Relationships	Graph DB or SQL table

📦 Sample API Endpoints
http
Copiar
Editar
GET /feed?user_id=123
POST /post
POST /like?post_id=p123
GET /post/:id
⚙️ Optimizations
Area	Optimization
Feed speed	Cache timeline in Redis
Trending content	Precompute trending posts
Data skew	Use sharding and consistent hashing
Media latency	Use CDN for videos/images
Personalization	Lightweight ML-based ranking service

📊 Metrics to Track
Feed load latency

Post-to-feed delivery delay

User engagement rate (likes/comments per user)

Fanout failure rate

Redis hit/miss ratio

🛡️ Security & Abuse Handling
Content moderation system

Report/block functionality

Rate limiting feed endpoints

Bot/spam detection

🎙️ What to Say in Interviews
“I'd use a hybrid fanout model: push timelines to heavy users and pull for light ones. Posts are stored in NoSQL with media in S3. Feed rankings are cached in Redis using recency, engagement, and social proximity. A ranking service improves personalization using engagement history.”

✅ Summary Table
Component	Technology Choice
Post Storage	MongoDB / Cassandra + S3
Timeline Caching	Redis
Feed Ranking	Heuristics or ML-based scoring
Fanout Strategy	Hybrid (write for heavy users)
Feed API	REST + Pagination + Filters