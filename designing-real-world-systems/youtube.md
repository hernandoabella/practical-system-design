Designing YouTube (Video Upload, Processing, Streaming)
"YouTube isn’t just about playing videos — it’s about uploading, transcoding, distributing, and streaming petabytes of data to billions of devices with low latency and high reliability."

🧾 Functional Requirements
✅ Core Features
Upload video (supporting large files)

Transcode video to multiple resolutions (e.g., 240p–4K)

Store and stream videos efficiently

Enable pause, play, seek (progressive or adaptive streaming)

Track views, likes, comments (not covered in this scope)

Show thumbnails and video metadata

🚫 Non-Functional
Global availability

Scalability (billions of views/day)

Fault tolerance and high durability

CDN-backed video delivery

🎯 System Goals
Goal	Objective
Availability	99.999% — video should always be accessible
Scalability	Handle millions of concurrent viewers
Durability	Prevent video data loss (multi-region replication)
Latency	Fast seek/start (ideally <1s startup delay)
Compatibility	Support all browsers/devices/resolutions

🧠 High-Level Architecture
pgsql
Copiar
Editar
+-------------------+       +------------------+
|    Upload Client  | ----> | Upload Service   |
+-------------------+       +--------+---------+
                                     |
                                     v
                            +-------------------+
                            |  Object Storage   |
                            +--------+----------+
                                     |
                            +--------v--------+
                            | Transcoding Job |
                            |   Queue (SQS)   |
                            +--------+--------+
                                     |
                            +--------v--------+
                            | Transcoder Pool  |
                            +--------+--------+
                                     |
                            +--------v--------+
                            | CDN + Chunker   |
                            +--------+--------+
                                     |
                            +--------v--------+
                            | Video Player    |
                            +-----------------+
📥 Upload Flow
User Uploads a Video

Handled via chunked upload (multipart)

Uploaded to temporary blob storage (e.g., S3)

Metadata Saved

File name, uploader ID, length, etc.

Saved to SQL or NoSQL metadata DB

🔄 Video Processing (Transcoding)
🎞️ Key Actions:
Triggered via job queue (e.g., AWS SQS, Kafka)

Convert video into multiple resolutions & bitrates (e.g., 144p to 4K)

Use ffmpeg (open-source) or proprietary encoder

Save outputs as chunked segments (HLS/DASH formats)

bash
Copiar
Editar
ffmpeg -i video.mp4 -preset fast -b:v 1000k -s 1280x720 output_720p.mp4
📦 Video Storage
Store each resolution in object storage (e.g., AWS S3, GCP Storage)

Use cold storage (e.g., Glacier) for rarely watched content

Store in chunks (e.g., 10-second segments)

plaintext
Copiar
Editar
/video/abc123/
    ├── 1080p/
    ├── 720p/
    ├── 480p/
    └── thumbnail.jpg
📡 Content Delivery via CDN
CDN (Content Delivery Network):
Delivers video chunks close to the viewer

Reduces latency & server load

Supports caching, edge locations, and geo-routing

Popular CDNs: Cloudflare, Akamai, CloudFront

🎬 Streaming Protocols
Protocol	Features	Use Case
HLS	Apple, HTTP-based, chunked	Most widely used
DASH	Adaptive bitrate streaming	Netflix, YouTube
RTMP	Older, used for live streaming	OBS → YouTube Live

Adaptive Bitrate: Player automatically chooses resolution based on network speed.

🧠 Timeline Example: Upload to Play
css
Copiar
Editar
[Upload Start] --> [File Saved] --> [Transcoding Job Queued] -->
[Transcoding Completed] --> [CDN Upload] --> [Available for Streaming]
Average end-to-end time: < 1 min (for <10min HD video with fast transcoding).

🛡️ Durability & Availability
Replicate videos across regions

Use checksum validation for file integrity

Fall back to alternate CDNs if one fails

📊 Key Metrics
Upload success rate

Transcoding latency

CDN cache hit rate

Playback start delay

Video buffer time

🧰 Tech Stack Suggestions
Layer	Tools / Tech
File Upload	NGINX, AWS S3, GCS, multipart upload
Transcoding	FFmpeg, AWS MediaConvert, Bitmovin
Storage	S3, Google Cloud Storage
Queue	SQS, Kafka, RabbitMQ
CDN	Cloudflare, CloudFront, Akamai
Monitoring	Prometheus, Grafana, New Relic

🎙️ What to Say in Interviews
“I’d use chunked uploads and object storage like S3. After upload, a job is queued to transcode the video into multiple resolutions using ffmpeg. Chunks are stored and served via a CDN like CloudFront using HLS. The frontend player supports adaptive streaming depending on the user’s bandwidth.”

✅ Summary Table
Feature	Design Choice
Upload Handling	Multipart + object storage
Transcoding	Queue + ffmpeg
Streaming	HLS / DASH + adaptive bitrate
Storage	Chunked segments in S3-like service
Delivery	Global CDN distribution
Metadata	SQL/NoSQL DB

