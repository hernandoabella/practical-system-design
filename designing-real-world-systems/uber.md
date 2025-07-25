Designing Uber (GPS, Matching, Pricing)
“Uber isn't just a taxi app — it’s a real-time, location-based, distributed system handling billions of events per day across riders and drivers.”

🧾 Functional Requirements
✅ Core Features:
Users can request rides

Drivers receive ride requests & accept them

Real-time location tracking of riders and drivers

Trip pricing, fare estimates, surge pricing

Payment integration

Ride status updates (en route, started, completed)

🚫 Non-Functional:
Real-time latency (sub-500ms) for matching

Global availability

Highly fault-tolerant

Horizontally scalable

Geo-distributed architecture

🎯 System Goals
Attribute	Goal
Scalability	Millions of concurrent users
Availability	99.999% uptime
Accuracy	Real-time geolocation & ETA
Performance	Sub-second request-to-match latency
Security	Secure location, payment & identity

🧠 High-Level Architecture
lua
Copiar
Editar
+--------+        +-----------------+       +-----------------+
| Rider  |<-----> | API Gateway     |<----> | Ride Service     |
+--------+        +-----------------+       +--------+--------+
                                                 |     |
+--------+        +-----------------+       +-----+    +----------------+
| Driver |<-----> | WebSocket Server|<----> | GPS |    | Pricing Engine |
+--------+        +-----------------+       +-----+    +----------------+
                                |
                                v
                      +------------------+
                      | Matching Service |
                      +------------------+
                                |
                     +-----------------------+
                     | Notification Service  |
                     +-----------------------+
📍 Key Components
1. API Gateway
Routes REST/GraphQL calls to appropriate services

Authentication (JWT)

Rate limiting

2. WebSocket / Streaming Server
Persistent connection for location updates

Real-time communication with drivers and riders

Event streaming via Kafka or gRPC streams

3. GPS Location Service
Updates driver and rider location every few seconds

Stores recent geolocation coordinates in Redis with TTL

Spatial indexing via [H3 (Uber’s own lib)] or [GeoHash]

sql
Copiar
Editar
driver_locations(driver_id, lat, lng, updated_at)
4. Matching Engine
Finds the closest available drivers

Uses geo-spatial queries (e.g. within 3km radius)

Can use K-D Tree, R-Trees, or Uber’s H3 Hex Grid

Filters based on rating, vehicle type, etc.

text
Copiar
Editar
Algorithm:
1. Rider sends pickup location
2. Matching engine queries all active drivers in area
3. Ranks based on proximity & availability
4. Sends ride request to driver
5. Waits for response or retries another match
5. Ride Service
Manages lifecycle of a ride: requested → accepted → started → completed

Persists ride history in a relational DB (Postgres/MySQL)

sql
Copiar
Editar
rides(ride_id, rider_id, driver_id, start_time, end_time, fare, status)
6. Pricing Engine
Calculates:

Base fare + time + distance

Dynamic pricing (surge factor)

Predicts ETA and estimated cost before trip

text
Copiar
Editar
Fare = Base + (Distance x Rate) + (Time x Rate) x Surge Factor
May use ML model for demand prediction

7. Notification Service
Push notifications (FCM, APNs)

Updates for ride accepted, driver arriving, ride started, etc.

8. Payment Service
Handles fare processing

Supports credit cards, PayPal, digital wallets

Supports cancellation fees and driver payouts

PCI compliant

🗺️ Location Matching via H3 (Hex Grid)
Uber’s open-source H3 is a hexagonal spatial indexing system.

World is divided into hexagons

Each driver is mapped to a hex

Search for drivers in neighboring hexes when matching

text
Copiar
Editar
Rider in Hex: 8f28308280f18c1
→ Query drivers in surrounding hexagons
⚖️ Trade-Offs & Design Decisions
Problem	Solution
Latency in driver matching	Use in-memory location store + fast geo queries
Stale location updates	TTL on Redis keys + frequent location pings
High rider density	Shard geo-spatial index across regions
Surge pricing fairness	Use heatmap + demand models
Global system latency	Deploy services in multi-region zones

🔐 Security & Privacy
Encrypt location data in-transit (TLS)

User privacy for location and phone numbers

Payment system PCI-DSS compliant

📈 Monitoring & Metrics
Location update frequency

Match success rate

ETA accuracy

Driver churn rate

Ride cancellation rate

Use Prometheus + Grafana + OpenTelemetry

📦 Tech Stack Suggestions
Component	Tech Choices
Location tracking	WebSockets, Kafka, Redis
Matching engine	H3, GeoHash, Elasticsearch
DB	PostgreSQL, MySQL, Cassandra
Messaging	RabbitMQ, Kafka
Notification	Firebase, OneSignal
Infra	Kubernetes, Terraform

🧠 What to Say in Interviews
“I’d track driver and rider locations via WebSockets and store them in Redis. The matching engine would use H3 for spatial indexing. A separate pricing engine would apply surge logic based on demand heatmaps. All messages and events would flow through Kafka for durability and scaling.”

✅ Summary Table
Feature	Key Tech / Design Approach
Real-time location	WebSockets + Redis + H3
Driver matching	Matching engine with proximity filters
Pricing	Rule-based + ML surge model
Payments	Secure payment gateway + payouts
Ride status	Lifecycle management in relational DB
Notifications	Push service + Kafka events
Global scale	Region-aware deployment + CDN

