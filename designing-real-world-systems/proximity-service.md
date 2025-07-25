Designing a Proximity Service / Nearby Friends
(e.g., “Find My Friends”, Tinder, Snapchat Snap Map, Uber nearby drivers)

A proximity service lets users discover other users/devices near their physical location. It must be accurate, low-latency, privacy-conscious, and scalable.

🧾 Functional Requirements
✅ Core Features
Share real-time or periodic location with the backend

Find users/devices within a certain radius (e.g., 5km)

Filter by type (e.g., only friends, only drivers)

Push notifications or updates when nearby users change

🚫 Non-Functional Requirements
Real-time performance (updates every few seconds)

Scalable to millions of users

Secure and privacy-compliant (e.g., GDPR)

Support for mobile and web clients

🎯 System Goals
Property	Goal
Latency	≤ 100–300ms
Scale	50M+ users updating frequently
Accuracy	Within 10–100 meters
Privacy	Must protect user location
Availability	99.9%

🧠 High-Level Architecture
plaintext
Copiar
Editar
[Mobile Client] → [Location API Gateway]
       ↓                 ↓
   [Auth Service]     [Geo-Spatial Index] ← [PostGIS / H3 / S2]
                          ↓
                [Nearby Lookup Service]
                          ↓
                 [User Service / Filters]
                          ↓
                     [Result to Client]
🧩 Key Components
1. Location Upload API
Mobile clients send GPS coordinates periodically

Include: user ID, timestamp, coordinates

Use HTTP or gRPC

Example:

json
Copiar
Editar
{
  "user_id": "u123",
  "lat": 40.7128,
  "lon": -74.0060,
  "timestamp": 1723459839
}
2. Geospatial Indexing
Option A: PostGIS (PostgreSQL Extension)
Use earth_distance or ST_DWithin() for distance filtering

Pros: Easy to integrate

Cons: Not great at high scale

Option B: Uber H3 or Google S2
Encode lat/lng into hierarchical hex grids

Nearby search = look in same and adjacent hexagons

Fast, scalable, distributed-friendly

📦 Store user locations by H3 cell → maintain hash map like:

json
Copiar
Editar
"H3:8928308280fffff": [user1, user2, user3]
3. Nearby Lookup Service
Accepts: lat/lon, radius, and filters

Queries geospatial index for nearby users

Applies business logic (e.g., friends only)

Returns ranked or random nearby results

4. Real-Time Pub/Sub (Optional)
Use Redis pub/sub or Kafka to notify clients:

When a nearby friend appears

When someone leaves the radius

5. Privacy Controls
Only share location if user opts in

Support ghost mode / incognito

Token-based access for friends

TTL for location updates (auto-expiry)

⚙️ Scaling Techniques
Problem	Solution
Frequent updates	Use delta uploads / adaptive timing
Write overload	Use message queues or Kafka
Read hotspots	Cache nearby user results
Index scaling	Shard location index by region
Mobile optimization	Use batching / compression

🧠 Query Example (Using H3)
plaintext
Copiar
Editar
1. Encode user location into H3 cell (res 9)
2. Query same + adjacent H3 cells
3. Retrieve users in those cells from Redis
4. Filter out users beyond actual radius (via Haversine formula)
5. Return final list to user
🧪 Data Models
sql
Copiar
Editar
-- User location table
user_locations(user_id, lat, lon, timestamp, h3_cell)

-- Friendship mapping
friendships(user_id_1, user_id_2, status)
🔐 Security & Privacy
HTTPS always for location data

Validate auth tokens on upload & query

Rate limiting on location requests

Encrypt location data at rest

Opt-in only sharing with friend control

📊 Monitoring Metrics
Metric	Importance
Location update rate	Monitor ingestion load
Nearby query latency	Ensure real-time responses
Hotspot traffic	Popular areas for prefetch/caching
Ghost mode usage	Privacy feature health
Accuracy complaints	Debug GPS issues

🧠 Interview Tips
“I’d store user locations using Uber’s H3 indexing to encode latitude/longitude into geo-cells, allowing constant-time lookup for nearby users. To prevent overload, I’d use Redis for hot area caching and Kafka to buffer frequent GPS updates from clients. For privacy, only friends can query your location, and users can opt-in or disable sharing.”

✅ Summary Table
Component	Choice / Stack
Location Ingestion	REST/gRPC + Kafka
Indexing Engine	H3 / S2 / PostGIS
Lookup Cache	Redis
Database (fallback)	PostgreSQL or DynamoDB
Notification System	Kafka, FCM / APNs
Privacy Enforcement	Token-based ACL + TTL cleanup