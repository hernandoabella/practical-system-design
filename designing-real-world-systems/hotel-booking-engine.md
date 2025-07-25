Designing a Hotel Booking Engine
Goal: Let users search, view, and book hotel rooms in real-time while ensuring availability, accuracy, and consistency.

✅ Functional Requirements
Browse hotels by date, location, filters (price, amenities, rating)

View hotel details and availability

Book rooms with confirmation and payment

Handle concurrent bookings and prevent overbooking

Support cancellations and refunds

Admin panel for hotels to manage rooms, prices, availability

🚫 Non-Functional Requirements
High availability and responsiveness

Strong consistency (especially for room inventory)

Low latency for search queries

Scalable to millions of users and properties

Secure payments and user data protection

🧱 Core Components
1. Frontend
Search page

Hotel listing with filters

Hotel detail and booking form

User account (reservations, cancelations)

2. API Gateway
Routes traffic to services

Handles auth, rate limiting, logging

3. Services
Service	Responsibilities
Search Service	Full-text, geo-based search
Inventory Service	Manage room availability
Booking Service	Process reservations with payment
Pricing Service	Return price based on date, user, promo
Notification Service	Confirmations via email/SMS
Hotel Admin Service	CRUD for hotels, rooms, rates, photos
User Service	Accounts, login, booking history
Payment Gateway	Stripe/PayPal integration

4. Databases & Storage
Data	Storage Type
User, hotel, booking	Relational DB (PostgreSQL)
Availability calendar	In-memory + transactional DB
Search index	Elasticsearch
Media (photos)	S3 / Blob storage
Caching	Redis

🏗️ High-Level Architecture
plaintext
Copiar
Editar
       [Frontend / Mobile]
               |
         +-------------+
         | API Gateway |
         +-------------+
               |
  +------------+------------+
  |            |            |
Search     Booking     Inventory
Service     Service      Service
  |            |            |
Elastic    PostgreSQL   Redis + SQL
Search       (TXNs)     (availability)
📚 Data Models (Simplified)
sql
Copiar
Editar
USERS(id, name, email, password_hash)

HOTELS(id, name, location, star_rating, description)

ROOMS(id, hotel_id, type, max_guests, price_per_night)

AVAILABILITY(room_id, date, is_available)

BOOKINGS(id, user_id, room_id, start_date, end_date, status, amount_paid)

PAYMENTS(id, booking_id, amount, status, method, txn_id)
🧠 Critical Design Considerations
🔁 Availability Consistency
Use Redis or in-memory layer for fast reads

Use pessimistic locking or transactions for availability writes

Consider optimistic concurrency control for high throughput

⚖️ Load Management
Cache frequently searched locations

Use CDN for hotel images

Rate-limit booking attempts per user/IP

💳 Payment Handling
Use external gateway (Stripe/PayPal) with webhooks

Ensure idempotency of payment processing

⚙️ Booking Flow (Happy Path)
plaintext
Copiar
Editar
User → Search Hotels → Select Room → Enter Details → Pay → Booking Confirmed
Behind the scenes:

Lock inventory

Process payment

Confirm booking

Send confirmation (email/SMS)

🧪 Failure Scenarios
Scenario	Handling Strategy
Payment succeeds, booking fails	Retry booking or refund user
Double booking attempt	Lock availability record (DB/Redis)
API timeout from hotel provider	Circuit breaker / fallback retry
Partial outages (e.g. search)	Graceful degradation, cached results

📈 Scaling Plan
Component	Scaling Method
Search Service	Partition by location, use replicas
Booking Service	Stateless, add more instances
Inventory Service	Shard by hotel/region
Media CDN	Global edge caching

🔐 Security
Input validation & sanitization

TLS everywhere

Rate limiting + API keys

PCI-compliant payment gateway usage

🧠 Interview Soundbite
“To ensure consistency in bookings, I’d decouple the booking flow from payment using an event queue. I’d also use an availability cache (e.g. Redis) backed by transactional writes to a SQL DB. For search, I’d index hotel metadata in Elasticsearch and geo-cluster it for low-latency queries.”

✅ Summary
Element	Technology / Strategy
Search	Elasticsearch + Geo filtering
Availability	Redis + Postgres (locking)
Booking	Atomic DB TXN + Queue + Webhook
Payment	Stripe + Idempotency Keys
Notifications	Worker queues (email/SMS)