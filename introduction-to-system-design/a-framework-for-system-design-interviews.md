# A Framework for System Design Interviews
> You don’t need to memorize answers. You need to show how you think.

System design interviews are open-ended and ambiguous. The good news? With the right framework, you can handle any question confidently.

## What Interviewers Are Looking For
- **Structured thinking:** Break down complexity into clear parts.
- **Tradeoff awareness:** Understand the implications of design decisions.
- **Communication:** Explain what you're building and why.
- **Adaptability:** Adjust to changing or vague requirements.
- **Technical depth:** Go deep when needed (DB, cache, APIs, etc.).

## The 4-Step System Design Interview Framework
### 1. Clarify Requirements
> Don’t start designing. Start asking.

### Ask:
- What’s the main goal of the system?
- Who are the users? What’s the usage pattern?
- Core features? (Scope control)
- Non-functional requirements? (Latency, uptime, consistency)
- Constraints or required tech?

### Example:
- **Prompt:** Design a URL shortener
- **Clarifications:** Should links expire? Are analytics needed? Is custom aliasing allowed?

### 2. High-Level Design
Paint the big picture before details.
### Include:
- Clients (web, mobile)
- API layer or Gateway
- Core services/microservices
- Databases & caching
- Load balancers, queues, CDN
- External integrations (payment, notifications)

**Tools to mention:** REST vs gRPC, SQL vs NoSQL, Redis, Kafka, S3, CDN, API Gateway

### 3. Deep Dive into Key Components
Pick 2–3 critical parts and go deep.
### Possible areas:
- Database schema & partitioning
- Caching strategy & invalidation rules
- Rate limiting & abuse prevention
- Messaging queues & async processing

### API design & versioning
- **Security:** JWT, OAuth2, encryption
- **Data model:** Relational, KV store, Graph DB

**Pro tip:** State why you chose a design, and what changes at scale.

### 4. Bottlenecks, Scaling & Tradeoffs
Every system breaks. Where does yours?
### Discuss:
- First bottlenecks under load
- Failure points & failover strategies
- Vertical vs horizontal scaling plans
- MVP → millions of users growth path

### Common tradeoffs:
- Consistency vs Availability
- Speed vs Cost
- Complexity vs Maintainability

<img width="875" height="95" alt="System Design Graphics (19)" src="https://github.com/user-attachments/assets/e85a8496-252e-4769-87d1-6dc37654815a" />

### Example Prompt Walkthrough
- **Prompt:** Design a Messaging App (like WhatsApp)
- **Clarify:** Group messages? Read receipts? Multimedia?
- **High-Level:** Client → API → Chat service → DB/Cache + Queue
- **Deep Dive:** Data schema, real-time delivery, message ordering
- **Bottlenecks:** Message fanout, offline sync, scaling to millions

### Final Thought
You don’t need to design the perfect system.
### You need to:
- Think out loud
- Justify your decisions
- Show awareness of constraints
