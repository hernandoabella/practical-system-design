Designing a Real-Time Chat System
(e.g., WhatsApp, Messenger, Slack, Telegram)

A chat system enables instant, bi-directional communication between users. It must ensure reliable message delivery, real-time updates, and scalability to millions of users.

🧾 Functional Requirements
✅ Core Features
One-to-one messaging

Group chats

Message delivery & read receipts

Online/offline indicators

Message history

Real-time updates (WebSockets or long polling)

Multimedia messages (images, videos, files)

Typing indicators

🚫 Non-Functional Requirements
High availability and scalability

Low latency (< 200ms delivery time)

Message durability (no message loss)

End-to-end encryption (optional)

Fault tolerance and retry logic

🎯 System Goals
Property	Goal
Latency	< 200 ms
Scale	10M+ concurrent users
Availability	99.999%
Storage	30 days or more chat history
Real-Time	Always up-to-date conversations

🧠 High-Level Architecture
plaintext
Copiar
Editar
User A ↔ WebSocket Gateway ↔ Chat Service ↔ Message Queue ↔ Message Store
                                 ↓
                             Notification Service
                                 ↓
                             WebSocket Gateway ↔ User B
🧩 Key Components
1. WebSocket Gateway
Maintains persistent connections

Identifies users via auth token/session

Routes messages to the right user/channel

Tech: Node.js, Go, or NGINX with WS support
Scale: Each WS server can handle 20k–100k connections

2. Chat Service (Core)
Handles message routing and logic:

Validate users

Apply delivery rules

Route to correct chat/group

Pushes messages to a message queue for durability

3. Message Queue (Kafka / RabbitMQ / SQS)
Ensures asynchronous, durable message processing

Buffers high-velocity traffic

Supports retries and fan-out to multiple services

4. Message Store (Database)
Stores all chat history and metadata

Data Type	DB Choice
Messages	NoSQL (Cassandra, DynamoDB, MongoDB)
Users	SQL (Postgres)
Groups	SQL or Redis

json
Copiar
Editar
{
  "message_id": "m123",
  "sender_id": "u123",
  "receiver_id": "u456",
  "timestamp": "2025-07-19T12:30Z",
  "content": "Hello!",
  "status": "delivered"
}
5. Delivery Acknowledgment & Status Tracking
Sent → Delivered → Seen

Updated via WebSocket or polling

TTL cleanup of transient state

6. Push Notification Service
For offline users

Integrates with FCM (Firebase) / APNs

Sends fallback alert if user missed real-time message

🔁 Message Flow
plaintext
Copiar
Editar
1. User A sends message via WebSocket
2. Message goes to Chat Service
3. Chat Service stores in DB & pushes to Kafka
4. Kafka triggers delivery to recipient
5. If User B is online → WS push
   Else → Notification Service triggers push notification
6. Delivery & seen events sent back to sender
📦 Data Models
sql
Copiar
Editar
-- Message Table
messages(id, sender_id, receiver_id, content, timestamp, status)

-- User Table
users(id, name, status, last_seen)

-- Group Table
groups(id, name, participants, created_at)
⚙️ Scaling Techniques
Challenge	Solution
Connection overload	Load balance WebSocket servers
Message durability	Use Kafka with disk-backed queues
Delivery speed	Use Redis pub/sub for fast fanout
Horizontal scaling	Stateless microservices + token routing
Group chat explosion	Fanout via batch messaging queues

🧠 Advanced Features
Feature	How to Build
Typing Indicator	Emit typing_start/stop via WebSocket
Read Receipts	Emit read_ack event via WS
Message Reactions	Store in message metadata
Multimedia Sharing	Upload to S3 → send file link via WS
End-to-End Encryption	Use public/private keys with AES/PGP

🔐 Security Considerations
Auth tokens for WS connection

Encrypt content in transit (TLS) and optionally at rest

Spam protection (rate limits per user)

Abuse reporting (message logging, moderation service)

🧪 Monitoring & Metrics
Metric	Importance
Message delivery latency	Track performance
WebSocket disconnects	Server health
Queue backlog	Potential bottlenecks
Push notification delays	Mobile experience quality
Message loss/failures	Reliability insights

🧠 Interview Tips
"I’d use WebSocket gateways to maintain persistent connections, Kafka as a durable message broker, and Redis for delivery fanout. Chat messages are stored in NoSQL for fast reads/writes, and push notifications are sent via Firebase/APNs for offline users."

✅ Summary Table
Component	Technology Stack
Real-time channel	WebSockets (WS protocol)
Message broker	Kafka or RabbitMQ
Storage	MongoDB / Cassandra / PostgreSQL
Notification	Firebase (FCM), Apple (APNs)
Media	Amazon S3 / GCP Storage
Auth	JWT / Session Tokens