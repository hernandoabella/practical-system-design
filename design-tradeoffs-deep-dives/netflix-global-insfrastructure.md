Case Study: Netflix Global Infrastructure
“One of the world’s most demanding, distributed, and scalable real-time video platforms.”

Netflix isn't just a streaming service — it's a masterclass in resilient system design, global scalability, and content delivery engineering. This case study covers how Netflix streams high-definition content to over 230+ million users globally, while maintaining 99.99% uptime.

🧩 Key Requirements
🎯 Functional:
Global on-demand video streaming (SD, HD, 4K)

Personalized recommendation engine

Real-time user activity tracking (e.g., pause, resume)

Cross-device support (TV, mobile, tablet, desktop)

Smooth playback with adaptive bitrate

Multi-language subtitle/audio support

🔐 Non-Functional:
Near-zero downtime

Ultra-low latency playback (<1s start time)

CDN-level bandwidth optimization

Fault-tolerant and resilient architecture

Compliance with regional regulations (e.g., GDPR)

🏗️ High-Level Architecture Overview
text
Copiar
Editar
                            +---------------------------+
                            |     User Devices          |
                            | (Smart TVs, Phones, etc.) |
                            +------------+--------------+
                                         |
                                 HTTPS / Adaptive Bitrate
                                         |
                            +------------v------------+
                            |   Netflix Open Connect   |
                            |   CDN (Edge Caches)      |
                            +------------+------------+
                                         |
                       +-----------------+-----------------+
                       |                                   |
            +----------v---------+             +-----------v----------+
            | Netflix Backend    |             |   Recommendation ML   |
            | Services (API)     |             |   Models & Dataflow   |
            +----------+---------+             +-----------------------+
                       |                                       |
     +-----------------v-----------------+     +---------------v---------------+
     | Playback Service, Auth, Billing   |     | Spark, Presto, Kafka, MLFlow  |
     +----------------+------------------+     +---------------+---------------+
                      |                                      |
         +------------v------------+            +------------v------------+
         |   AWS Global Infrastructure           |   Netflix Data Lake    |
         | (EC2, S3, RDS, DynamoDB, etc.)        | (Amazon S3 + Hive + EMR)|
         +---------------------------------------+------------------------+
🌍 Open Connect – Netflix's Own CDN
Netflix built its own global CDN, called Open Connect, to reduce costs and improve performance.

1,000+ edge locations globally

Caches popular content at ISPs and IXPs (Internet Exchange Points)

Weekly pre-positioning: Uploads upcoming shows to edge caches in advance

Edge-aware load balancing ensures users connect to the closest server

Benefits:
Minimizes use of public cloud bandwidth

Allows precise control over QoS (quality of service)

Enables efficient prefetching and A/B testing at edge level

🧠 Personalization & Recommendation Engine
Netflix’s success lies in its ML-powered personalization:

Feature Store: Stores user preferences, watch history, genre affinities

Ranking Models: Use collaborative filtering, embeddings, deep learning

Daily Training Pipelines: Apache Spark, MLFlow, Presto

Real-Time Adaptation: Re-ranks videos based on live feedback

🎥 Playback & Adaptive Streaming
Videos are broken into segments (2-10 seconds) and encoded in:

Multiple resolutions: 240p → 4K

Multiple codecs: H.264, H.265, AV1

DASH / HLS protocols used for adaptive bitrate switching

The Netflix client selects the appropriate resolution based on:

User’s bandwidth

Device capabilities

Buffer health

🛡️ Fault Tolerance & Chaos Engineering
Netflix pioneered Chaos Engineering with tools like:

Tool	Purpose
Chaos Monkey	Randomly terminates servers to test resiliency
Latency Monkey	Injects delays to simulate network slowness
Simian Army	Suite of tools for failure injection

They also:

Use multi-region failover

Deploy stateless services with retries and exponential backoff

Operate on self-healing, immutable infrastructure

📊 Observability & Monitoring
Atlas: Netflix’s own metrics platform

Spinnaker: For multi-cloud deployments with rollback support

Titus: Custom container orchestration system built on top of Mesos

Zipkin: Distributed tracing for service latency monitoring

🧾 Compliance & Localization
Data must be hosted regionally for some countries

Supports multi-language metadata, subtitles, dubbing

Integrates DRM (Digital Rights Management) across devices

✅ Interview-Worthy Takeaways
If asked to design "Netflix-like architecture", emphasize:

Use of dedicated CDN (Open Connect) for edge delivery

Adaptive streaming with multiple encodings and bitrate ladders

Real-time personalization using ML pipelines and feature stores

Multi-region active-active deployment on AWS global infra

Chaos Engineering for validating system resilience

Observability via custom-built platforms like Atlas and Spinnaker

⚖️ Tradeoffs & Design Choices
Design Decision	Rationale
Build own CDN (Open Connect)	Reduce cost, control edge distribution
Pre-position content	Avoid peak-time bottlenecks
Use multiple bitrates/codecs	Ensure best experience across diverse networks/devices
Chaos testing in prod	Find unknown points of failure before real incidents
Custom infra tools	Tailored control over deployment & scaling

📦 Bonus: Content Delivery Lifecycle (Text Flow)
text
Copiar
Editar
Upload → Transcode → Segment → Store in S3 → Push to Edge Cache → Playback on Device