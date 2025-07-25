Designing an S3-like Object Storage System
A distributed object storage system allows you to store and retrieve massive volumes of unstructured data (files, videos, logs, backups) using a simple API (usually REST-based).

✅ Functional Requirements
Upload, retrieve, delete objects via API

Store metadata (filename, size, type, timestamps)

Support for buckets (namespaces) and keys (paths)

Multipart uploads for large files

Versioning and soft deletion

Public and private access control (ACLs, presigned URLs)

🚫 Non-Functional Requirements
High durability (e.g., 99.999999999% or “11 9s”)

Scalable to exabytes of data and billions of objects

Low-latency for frequent access (hot data)

Strong consistency for recent writes (eventual for some reads)

Secure access and audit logging

📦 Core Concepts
Concept	Description
Bucket	A namespace to store objects (like folders)
Object	The actual file/data being stored
Key	Unique identifier within a bucket (filename or path)
Metadata	Descriptive data for each object

🧱 Core Components
Component	Role
API Gateway	Receives upload, download, delete requests
Object Store	Stores raw binary data (chunks)
Metadata Store	Tracks keys, versions, size, timestamps
Chunk Manager	Handles splitting/merging of large objects
Replication Manager	Ensures durability across zones/nodes
Access Control	Verifies ACLs, tokens, presigned URLs
Garbage Collector	Cleans up deleted/expired versions

🏗️ High-Level Architecture
text
Copiar
Editar
          ┌───────────────────────┐
          │     Client / SDK      │
          └─────────┬─────────────┘
                    │
            ┌───────▼────────┐
            │   API Gateway  │  <── REST API (S3-compatible)
            └───────┬────────┘
                    │
      ┌─────────────┼────────────────────┐
      │             │                    │
┌─────▼────┐ ┌──────▼─────┐     ┌────────▼────────┐
│Metadata  │ │Chunk Manager│     │ Access Control │
│ Service  │ └──────┬─────┘     └────────┬────────┘
└─────┬────┘        │                    │
      │     ┌───────▼────────┐     ┌─────▼─────┐
      │     │   Object Store │     │ Auth Store│
      │     └───────┬────────┘     └───────────┘
┌─────▼────┐  ┌──────▼──────┐
│ Replicator│  │ Garbage Collector │
└───────────┘  └──────────────────┘
🧮 Metadata Schema (Simplified)
sql
Copiar
Editar
BUCKETS(id, name, owner_id, created_at)

OBJECTS(id, bucket_id, key, version_id, size, content_type, etag, is_deleted, created_at)

CHUNKS(object_id, chunk_index, storage_path, checksum, created_at)
⚙️ Storage Layer
Object Data: stored in distributed blob storage (e.g., disks, Ceph, or local files)

Chunks: large files are split (e.g., 10MB each) and stored independently

Redundancy: data is replicated across multiple storage nodes or zones (3x replication or erasure coding)

Versioning: multiple object versions retained in metadata, flagged as is_deleted or active

🔐 Access Control
Feature	Description
ACLs	Bucket/Object-level access rules
Presigned URLs	Temporary signed links with expiration
IAM integration	Enforce policies based on user role or token
Audit logs	Track who accessed or modified what and when

🧠 Key Design Considerations
🧪 Durability
Replication factor (3x) or erasure coding

Background integrity checks with checksum

Geo-redundancy across data centers

📈 Scalability
Shard metadata store (e.g., based on bucket ID or hash of key)

Horizontal scaling of object storage nodes

Use consistent hashing to assign objects to nodes

⚡ Performance
Cache frequently accessed objects (CDN or in-memory cache)

Parallelize multipart uploads/downloads

Use of index services for object listing/search

🔁 Multipart Upload
text
Copiar
Editar
1. Initiate upload (creates upload ID)
2. Upload chunks in parallel (chunk 1, 2, 3…)
3. Complete upload (server merges and finalizes)
🌐 Example REST API (S3-Compatible)
http
Copiar
Editar
PUT /my-bucket/photos/beach.jpg     --> Upload
GET /my-bucket/photos/beach.jpg     --> Download
DELETE /my-bucket/photos/beach.jpg  --> Soft delete
GET /my-bucket/photos/              --> List objects
⚖️ Trade-Offs
Concern	Solution
Consistency	Strong on write, eventual for list/read
Speed vs Storage	Use compression, tune chunk size
Cost	Tiered storage (hot, cold, archival)
Large file sizes	Multipart uploads and streaming

💬 Interview Soundbite
“I’d break the system into an API gateway, metadata store, and chunk-based object storage. For durability, I’d replicate chunks across zones. Access control would rely on ACLs and presigned tokens. To scale metadata, I’d use sharding by bucket ID, and store large objects using multipart upload with a chunk manager.”

📘 Tech Stack Recommendations
Layer	Tools/Tech
API Gateway	NGINX + Go/Python/Node
Metadata DB	PostgreSQL / CockroachDB
Object Store	MinIO / Ceph / Local Disks
Chunk Store	S3-compatible disk backend
Replication	Kafka / Custom replication loop
Auth	OAuth2 / IAM integration