Designing a Distributed Email Service
Goal: Send, receive, store, and search emails reliably across millions of users with scalability, security, and speed.

✅ Functional Requirements
Send and receive emails with attachments

Support SMTP for outgoing mail

Support IMAP/POP3 for client access

Store and index messages for fast retrieval

Support folders, labels, and filters

Enable search (subject, sender, content)

Spam filtering and delivery status

Track read/unread status and threading

🚫 Non-Functional Requirements
High availability (24/7 access)

Strong consistency for inbox state

Low-latency message delivery

Scalable to millions of users and billions of messages

Secure storage and transmission (end-to-end encryption)

🧱 Core Components
Component	Responsibilities
Mail Transfer Agent (MTA)	Handle sending via SMTP
Mail Delivery Agent (MDA)	Handle storing email to user inbox
Mail User Agent (MUA)	Interface for users (web, IMAP, app)
Spam Filter / Scanner	Block malicious content
Storage Engine	Store emails and attachments
Search Indexer	Support full-text search
Notification Service	Push real-time updates (new email)

🏗️ High-Level Architecture
plaintext
Copiar
Editar
User
 │
 ▼
[MUA - Web, App, IMAP]
 │
 ▼
[API Gateway]
 │
 ├─> [Authentication Service]
 ├─> [Send Email Service] ──> [MTA - SMTP] ──> [Other Mail Server]
 ├─> [Inbox Service]
 │      └─> [MDA] ──> [Storage: Email DB + Object Store]
 ├─> [Spam & Virus Filter]
 ├─> [Search Service] ──> [Index Engine]
 └─> [Notification Service] ──> [WebSocket/Push]
💾 Data Model (Simplified)
sql
Copiar
Editar
USERS(id, email, password_hash, quota, ...)

EMAILS(id, sender_id, recipient_id, subject, body, timestamp, thread_id, folder, is_read)

ATTACHMENTS(id, email_id, filename, mime_type, url)

LABELS(id, user_id, label_name)

EMAIL_LABELS(email_id, label_id)
Attachments are stored in Blob storage (S3/GCS) with metadata in the DB.

🧠 Key Design Considerations
🔐 Security
TLS for SMTP, IMAP, POP3

DKIM, SPF, DMARC for outbound legitimacy

Encryption of stored content (AES-256)

Rate limiting to prevent abuse (e.g., email spam)

🔁 Delivery & Retry Logic
Store email if recipient server is unreachable

Retry with exponential backoff

Bounce handling and SMTP status codes

📬 Incoming Email Flow
plaintext
Copiar
Editar
Internet → MTA → Spam Filter → MDA → Email Storage → Inbox Index
📤 Outgoing Email Flow
plaintext
Copiar
Editar
MUA → API → Send Service → MTA (SMTP) → Recipient MTA
📈 Scaling Considerations
Component	Strategy
Storage	Shard by user ID or domain
Index/Search	Use Elasticsearch
Email Queues	Use Kafka for async processing
Attachment Store	Use S3 / Object Storage
Notification	Use pub/sub + WebSocket fallback

🔍 Search Functionality
Index subject, body, sender, labels, etc.

Use Elasticsearch or Apache Lucene

Search Queries:

"from:alice subject:meeting after:2024-01-01"

"label:work has:attachment"

🔔 Notifications
Use WebSocket or Push APIs for:

New message alert

Read receipt updates

Email deletion or label changes

🔄 Sync & Access Protocols
Protocol	Purpose	Notes
SMTP	Send emails	Port 587 with TLS
IMAP	Read/access email	Syncs state (read/unread, folders)
POP3	Download emails	Often used for local storage

🧠 Interview Soundbite
“I’d separate the MTA, MDA, and MUA into modular services. Use SMTP for send, IMAP for read, and store metadata in PostgreSQL with attachments in object storage. Emails are indexed in Elasticsearch for fast search. A spam filter service would scan all incoming messages, and all user events (like read/unread) would be pushed via a notification system.”

✅ Summary Table
Component	Tech Stack
Send/Receive	Postfix, Exim, SMTP, IMAP
Storage	PostgreSQL, S3
Indexing/Search	Elasticsearch
Spam Filtering	SpamAssassin, ClamAV
Notification	Redis Pub/Sub, WebSockets
Security	TLS, SPF/DKIM/DMARC