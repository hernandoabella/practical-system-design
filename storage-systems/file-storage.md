File Storage Systems (S3, GCS, etc.)
“Not all data fits in a database. Sometimes, you just need a bucket to drop your files into — safely, durably, and at scale.”

Modern systems often need to store user uploads (images, videos, documents, backups). That’s where cloud object storage like Amazon S3, Google Cloud Storage (GCS), and Azure Blob Storage come in.

📦 What Is Object Storage?
Object storage is a way to store binary files (objects) in a flat namespace using unique keys.

Files = Objects

Folders = Prefixes

IDs = Keys

Each file is stored as:

css
Copiar
Editar
{ key: "user123/profile.jpg", data: binary, metadata: { content-type, timestamp } }
🧰 Object Storage vs File vs Block
Feature	Object Storage (S3)	File System (NFS)	Block Storage (EBS)
Access	HTTP (via REST API)	OS-level	Attached to VM
Scale	Infinite	Medium	Per disk
Use Case	Backups, media, logs	Shared drives	Databases, OS
Format	Key-value (binary)	Hierarchical	Raw blocks
Performance	Medium	Fast	Fastest

☁️ Popular Services
Provider	Product	Highlights
AWS	Amazon S3	Most widely used; lifecycle, policies
Google Cloud	GCS	Similar to S3, great for big data
Azure	Blob Storage	Integrated with Microsoft ecosystem
MinIO	Open-source	Self-hosted, S3-compatible
Backblaze	B2	Cost-effective storage alternative

🧱 How It Works
text
Copiar
Editar
[User Uploads File]
        ↓
+----------------------+
|    Application       |
+----------------------+
        ↓
+----------------------+
| Object Storage (S3)  |
| - bucket: "user-data"|
| - key: "user123/resume.pdf" |
+----------------------+
📜 Example: Upload to S3 (Pseudocode)
python
Copiar
Editar
s3.put_object(
  Bucket='user-content',
  Key='avatars/user123.jpg',
  Body=file_data,
  ContentType='image/jpeg'
)
🔐 Key Features
Feature	Description
Buckets	Containers for files (like folders)
Keys	Unique file identifiers
Presigned URLs	Temporary access links
Versioning	Retain history of file changes
Lifecycle Policies	Auto-delete or move to archive
Storage Classes	Hot, cold, archive (S3 Standard, Glacier)
Encryption	Server-side or client-side (SSE, KMS)
Access Control	IAM policies, public/private ACLs

📁 When to Use File Storage
✅ Use object storage when you need to:

Store user uploads (images, videos, resumes, etc.)

Host static websites or assets

Archive logs, reports, backups

Scale media platforms without hitting DB limits

❌ Don’t use object storage if:

You need low-latency transactional access

You're building a relational data model

You need POSIX-compliant file systems

💬 What to Say in Interviews
“For large binary files or unstructured content, I’d decouple file storage from the DB and use S3 or GCS. It’s scalable, durable (99.999999999%), and integrates well with CDNs for fast delivery.”

“To prevent exposing S3 buckets directly, I’d serve files via a signed URL or through a secure proxy layer.”

⚠️ Real-World Considerations
Concern	Mitigation
Hot files / high access	Use CDN (CloudFront)
Large file uploads	Use multipart upload
Security	Use IAM policies, no public buckets
Cost for frequent access	Use proper storage class (Standard vs Infrequent Access)

✅ Summary
Property	Value
Durability	99.999999999% (11 9’s)
Scalability	Virtually unlimited
Access	HTTPS (API-based)
Cost	Pay-per-GB, tiered
Best Use Case	Media, backups, static content

🏁 Final Takeaway
“Modern apps separate binary data from relational data. Use S3-like storage to store files — reliably, securely, and at massive scale.”