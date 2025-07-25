Designing Google Drive / Dropbox (Cloud Storage System)
Cloud storage services like Google Drive and Dropbox allow users to upload, sync, and share files securely across devices. Designing such a system requires support for file storage, syncing, sharing, versioning, metadata, and permissions—all while ensuring durability, availability, and scalability.

🧾 Functional Requirements
✅ Core Features
Upload, download, delete files

Folder creation and hierarchy

File syncing across devices

File versioning (optional)

Public/private file sharing

User authentication and permissions

🚫 Non-Functional Goals
High availability (files always accessible)

Strong durability (no data loss)

Scalable to petabytes of data

Low-latency access globally

🎯 System Goals
Property	Target
Durability	99.999999999% (11 9’s — replicate across regions)
Availability	99.99%+
Latency	< 300ms for file access
Scalability	Support millions of users and billions of files

🧠 High-Level Architecture
text
Copiar
Editar
          +-------------+
          |  Web / App  |
          +-------------+
                |
                v
      +--------------------+
      |  API Gateway       |
      +--------------------+
                |
     +----------+----------+
     |   Auth & User DB    |
     +----------+----------+
                |
     +----------v----------+
     |   File Metadata DB   |
     +----------+----------+
                |
         +------+------+
         |  File Storage |
         | (S3, GCS, etc)|
         +------+------+
                |
         +------+------+
         |  Versioning  |
         +-------------+
📁 Core Components
1. API Gateway
Entry point for uploads, downloads, sharing

Verifies user sessions

Rate limits and request validation

2. Authentication
OAuth2 / JWT tokens

Users and permissions database

sql
Copiar
Editar
users(user_id, email, password_hash)
permissions(user_id, file_id, access_type)
3. File Metadata DB
Stores file names, size, type, hierarchy

Points to the actual file chunks in storage

SQL or NoSQL (e.g., PostgreSQL, DynamoDB)

sql
Copiar
Editar
files(file_id, user_id, name, parent_folder_id, size, checksum, created_at)
4. Chunked File Storage
Store files in chunks (e.g., 4 MB)

Use object storage like AWS S3, GCS, Azure Blob

Optional: Use deduplication to save space (store checksum hash)

Enable multi-region replication for durability

5. Versioning
Store multiple versions of the same file

Could be full copies or delta-based (diff only)

sql
Copiar
Editar
file_versions(version_id, file_id, version_number, created_at)
🔄 Syncing Across Devices
Sync Process:
Client polls or receives events (WebSockets or Push Notifications)

Sync token / delta API used to detect changes

Conflict resolution (user override or last-write-wins)

🛡️ Sharing & Permissions
Sharing Type	Details
Public Link	Read-only access to a file
Invite Only	Only selected users have access
Editable	Users can upload/replace
Folder Access	Propagate permissions to contents

🧰 File Upload Flow
User sends file upload request

API Gateway verifies auth token

File split into chunks on client

Each chunk uploaded to blob storage

Metadata and file references saved in DB

📦 Download Flow
Client requests file

Server checks permissions

Fetch metadata → get chunk references

Stream chunks in correct order

🧠 Optimizations
Area	Optimization
Storage Cost	Deduplication (hash match)
Access Latency	Caching popular files (CDN, Redis)
Versioning	Store deltas, not full copies
Bandwidth	Gzip, Brotli compression
Large Files	Resume multipart uploads

🧪 Sample File Upload API
http
Copiar
Editar
POST /upload
Headers:
  Authorization: Bearer <token>
Body:
  {
    "filename": "design.pdf",
    "chunk": "<binary>",
    "index": 1,
    "total_chunks": 5
  }
📊 Monitoring & Metrics
File upload success/failure rates

Chunk retry/failure counts

Storage space used per user

API response time

Sync event latency

🧰 Tech Stack Suggestions
Layer	Technologies
API Gateway	Express.js / FastAPI / Kong
Auth	Firebase Auth / OAuth2
Metadata DB	PostgreSQL / DynamoDB
File Storage	AWS S3 / GCS / Azure Blob
Queue (sync)	Kafka / Redis Streams
Sync Notifications	WebSockets / Firebase
Monitoring	Prometheus, Grafana, ELK Stack

🎙️ What to Say in Interviews
"I’d store files as chunks in object storage and use a metadata DB to track hierarchy and permissions. Uploads are handled via multipart API, and changes are synced using push notifications or sync tokens. Versioning and deduplication reduce cost, while access control ensures secure sharing."

✅ Summary Table
Component	Choice
File Storage	Object Storage (S3, GCS), chunked
Metadata Storage	SQL or NoSQL
Versioning	Optional, delta-based preferred
Sync	Push notifications + polling fallback
Sharing	ACL-based + public/private link support
Durability	Multi-region replication (11 9s durability)