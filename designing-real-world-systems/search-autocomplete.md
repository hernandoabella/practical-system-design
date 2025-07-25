Designing a Search Autocomplete System
(e.g., Google Search, Amazon, YouTube search box)

The goal of a Search Autocomplete System is to provide fast, relevant suggestions as a user types. The system must be low latency, context-aware, and scalable to millions of users and queries.

🧾 Functional Requirements
✅ Core Features
Suggest search queries as the user types

Prioritize based on popularity, recency, or user behavior

Support typo tolerance and prefix matching

Language support (optional)

Suggestions within a given context (e.g., category, location)

🚫 Non-Functional Requirements
Latency < 100ms per keystroke

High availability (99.99%)

Horizontal scalability

Real-time indexing or periodic updates

🎯 System Goals
Property	Goal
Latency	< 100ms per keystroke
Scale	Millions of users and suggestions
Storage	Up to 100M+ search terms
Availability	99.99%
Freshness	Daily index updates or near-real-time

🧠 High-Level Architecture
plaintext
Copiar
Editar
      User
       |
    Frontend
       |
   Autocomplete API
       |
     Cache (Redis)
       |
   Search Trie / Prefix Index
       |
    Query Logs & Analytics
🧩 Key Components
1. Autocomplete API
Receives keystrokes (e.g., /search?q=sho)

Debounced (e.g., 100ms delay) to reduce API load

Sends back a ranked list of search suggestions

2. Search Index
Option A: In-Memory Trie
Fast prefix lookup (O(k) time, where k = prefix length)

Stores frequency counters for popularity ranking

Easily sharable across shards

Option B: Search Engine (e.g., Elasticsearch)
Supports fuzzy matching, typo tolerance, boosting

Scalable, but slightly slower than in-memory trie

3. Popularity Ranking
Each suggestion has a score based on:

Search frequency (logs)

CTR (click-through rate)

Freshness or recency

Personalization (optional)

json
Copiar
Editar
[
  { "term": "shoes", "score": 98 },
  { "term": "shopify", "score": 73 },
  { "term": "shorts", "score": 59 }
]
4. Caching Layer
Use Redis or Memcached to cache frequent prefixes

LRU cache + TTL eviction policy

Avoid DB/index hits on every keystroke

5. Real-Time Logging and Learning
Every query typed is logged

Logs processed into batch jobs or streaming jobs (e.g., with Apache Flink, Spark)

Feed updated term frequency back to index

📦 Data Models
sql
Copiar
Editar
-- Suggestions Table (for Elasticsearch or SQL fallback)
suggestions(term TEXT, frequency INT, last_updated TIMESTAMP)

-- Search Logs (for training)
query_logs(user_id, query_text, timestamp)
🧠 Query Flow
plaintext
Copiar
Editar
1. User types "sh"
2. Frontend waits 100ms (debounce)
3. Sends GET /autocomplete?q=sh
4. Redis checked for cached results
5. If not cached → query Trie or Elasticsearch
6. Suggestions ranked and returned
7. Query logged for feedback loop
🔍 Example Response
json
Copiar
Editar
{
  "suggestions": [
    "shoes",
    "shopify",
    "shorts",
    "shampoo"
  ]
}
⚙️ Scaling Techniques
Problem	Solution
High query volume	Redis + Sharded Trie
Update frequency	Stream logs into update pipeline
Typo tolerance	Elasticsearch fuzzy queries
Personalization overhead	Bucket users into cohorts

🔐 Security Considerations
Sanitize input to prevent injection attacks

Rate limit to prevent scraping

Ensure GDPR compliance for personal queries

📊 Monitoring & Metrics
Metric	Why It Matters
Query latency	Ensure user experience
Cache hit ratio	Optimize backend load
Top queries	Improve index and marketing
Error rate	System health
Feedback CTR	Are users clicking suggestions?

🧠 Interview Tips
"I'd design an autocomplete service using a Trie structure in memory for fast prefix matching, backed by a batch update process that uses search logs. Redis would cache hot prefixes, and Elasticsearch could be used for fuzzy suggestions or typo tolerance."

✅ Summary Table
Component	Choice / Strategy
Cache	Redis + TTL
Search Index	Trie or Elasticsearch
Storage	PostgreSQL or document store
Learning Loop	Logs → batch job → update Trie/Index
Ranking Factors	Frequency, recency, CTR