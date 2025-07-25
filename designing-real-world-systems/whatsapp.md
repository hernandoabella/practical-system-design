Designing WhatsApp (Messaging & Delivery Receipts)
“WhatsApp is a real-time, low-latency messaging platform used by billions — and it guarantees message delivery with double ticks and end-to-end encryption.”

🧾 Functional Requirements
✅ Core Features:
One-to-one & group messaging

Message delivery & read receipts (✓, ✓✓, ✓✓ blue)

Real-time sync across devices

Offline messaging support

End-to-end encryption

Media sharing (optional)

🚫 Non-Functional:
Low latency (<200ms) for message delivery

Highly available and durable

Works with intermittent connectivity (mobile-first)

Scalable to 2B+ users globally

🎯 System Goals
Category	Goal
Performance	Real-time delivery (under 200ms)
Durability	No message loss even if offline
Security	End-to-end encryption
Availability	99.99% uptime
Scale	100M+ concurrent users

🧱 High-Level Architecture
pgsql
Copiar
Editar
Client ↔ Load Balancer ↔ Messaging Gateway ↔ Message Queue ↔ Storage & Delivery Worker ↔ DB
                                              ↘↘↘
                                          Notification Service
Persistent connections (WebSockets)

Messages flow through a broker (Kafka/RabbitMQ/SQS)

Stored and relayed to online clients or queued for later

💬 Messaging Flow (1:1 Chat)
markdown
Copiar
Editar
1. Alice sends "Hi" to Bob
2. Server stores message
3. If Bob is online:
     - Message delivered → ✓✓
     - Bob opens chat → ✓✓ Blue
   Else:
     - Message queued
4. Bob reconnects → gets message from queue
📥 Delivery Receipt System (✓, ✓✓, ✓✓ Blue)
Tick Type	Meaning
✓	Sent to server
✓✓	Delivered to recipient’s device
✓✓ Blue	Read by recipient

Receipts are events: stored as metadata alongside messages

Each event triggers update to sender’s client

🧰 Core Components
1. Connection Layer (WebSocket Server)
Maintains persistent connection with clients

Authenticates users (JWT, token)

Receives/sends message packets

2. Message Broker (Kafka/RabbitMQ/SQS)
Decouples senders from delivery workers

Durable, high-throughput pipeline

Retry & dead-letter queues for failures

3. Delivery Workers
Consume from queue

Check recipient status (online/offline)

Send message OR enqueue for delayed delivery

Update message status

4. Message Store
Stores all messages with TTL (e.g., 30 days)

Encryption handled at client level

sql
Copiar
Editar
messages (
  message_id UUID,
  sender_id,
  recipient_id,
  content (encrypted),
  status ENUM(queued, delivered, read),
  timestamp
)
5. Presence Service
Tracks online/offline user status

TTL-based heartbeat (e.g., ping every 30s)

Stored in Redis or in-memory DB

🧱 Group Messaging Logic
Fan-out strategy: message copied to each member's inbox

Avoid duplicate send to same device

Delivery receipts per recipient

Can use fan-out-on-write (smaller groups) or fan-out-on-read (large groups)

🔐 Security: End-to-End Encryption (E2EE)
Messages encrypted on client

Server never sees plaintext

Uses Signal Protocol

Each message signed with a per-device session key

Server stores encrypted payload

📤 Offline Delivery
If recipient is offline:

Message stored in queue or DB

On reconnect, client fetches backlog

Message marked as “delivered” only after ACK

📊 Analytics & Logging
Track delivery latency, failure rates

Store event logs for observability

Metrics fed into Prometheus/Grafana

🧠 Scaling Considerations
Challenge	Solution
100M+ concurrent users	Partition WebSocket servers
Guaranteed delivery	Use queues with retries, persistence
Message ordering	Sequence IDs, per-chat delivery order
Online presence tracking	Redis-backed pub/sub heartbeat system
High availability	Use active-passive failover, replication

🧰 Tech Stack Example
Component	Tools/Technologies
Real-time conn	WebSockets / gRPC
Queue	Kafka / RabbitMQ
DB	PostgreSQL / Cassandra / Dynamo
Caching	Redis / Memcached
Monitoring	Prometheus + Grafana
Media Store	S3 / Cloudflare CDN

💬 What to Say in Interviews
“I’d use WebSockets for real-time delivery, backed by a message broker like Kafka. Messages are stored in encrypted format and pushed to the recipient or queued for later. Read/delivery receipts are separate metadata events, and Redis would power presence detection.”

✅ Summary Table
Feature	Implementation
Real-time messaging	WebSockets + broker queue
Delivery tracking	Metadata + ACK system
Read receipts	Event-based, client-triggered
Offline support	Queued delivery, TTL storage
Group chat	Fan-out with per-user delivery logs
Security	Signal Protocol E2EE
Scaling	Horizontally shard each component