# 4-Step Framework for Solving Any System Design Interview Question
> Structure beats spontaneity—every time.

System design interviews are intentionally ambiguous. This framework gives you a repeatable, proven approach to handle any question with confidence and clarity—even if you’ve never seen the problem before.

## Why Use a Framework?
Without structure, candidates often:
- Jump into components too early
- Forget important requirements
- Get lost in technical details
- Fail to highlight tradeoffs

A good framework helps you stay focused, prioritize, and communicate effectively.

## The 4 Steps
### Step 1: Clarify Requirements
A wrong design begins with wrong assumptions.

**Your goal:** Understand what you're being asked to design. Interviews often start vague on purpose—your job is to ask clarifying questions.

### What to clarify:
- **Functional features:** What should it do?
- **Non-functional goals:** Latency, availability, consistency?
- **Constraints:** Specific tech, budget, scale?
- **Assumptions:** Mobile/web clients? Auth required? SLA?

### Example Questions:
- Should users be able to delete or edit posts?
- Do we need support for file uploads?
- Are analytics or real-time metrics required?

### Step 2: Outline the High-Level Architecture
Draw the box-and-lines version of your system.

**Your goal:** Identify the key components and how they interact.

### Focus on:
- Client layer (web/mobile)
- APIs or gateway layer
- Core services or microservices
- Caching layers
- Database/storage systems
- External dependencies (e.g., payment, auth)

**Optional:** CDN, load balancers, monitoring

Keep it simple, but complete. This is your system’s skeleton.

### Step 3: Deep Dive Into Core Components
Zoom in on the most interesting or challenging parts.

**Your goal:** Show your depth of knowledge.

Pick 2–3 key areas depending on the system. Could include:
- Database schema and scaling (sharding, replication)
- Caching strategy (what, where, how long)
- Rate limiting & abuse protection
- Queuing and async processing
- File storage & CDN design
- Security and auth flow
- Realtime components (WebSockets, long-polling)

Always explain why you're doing it this way and the tradeoffs involved.

### Step 4: Discuss Bottlenecks, Tradeoffs & Scale
Stress test your design.
**Your goal:** Show that you’ve thought about:
- What breaks under load?
- How to scale to 10x more users?
- What happens on failure?
- Which parts can be improved over time?

### Talk through:
- Vertical vs horizontal scaling
- Caching invalidation strategies
- Tradeoffs between consistency, latency, availability
- Monitoring, logging, and alerting plans

**Be honest:** No system is perfect. Tradeoffs are real. Show you know them.

### Prompt: “Design Instagram Stories”
- **Clarify:** Expiration? Notifications? Privacy controls?
- **Architecture:** Client → API → Story service → DB/CDN → Notification queue
- **Deep dives:** Story storage (S3 + CDN), story expiration with TTL/cache, user feed generation
- **Bottlenecks:** Content fanout, CDN cache misses, offline viewing

### Final Advice
- Speak while thinking. Interviews aren't silent exams.
- Use whiteboarding or visuals. Don’t keep ideas in your head.
- Start simple, then iterate. MVP first, scale second.
- Practice 10–15 problems with this framework until it’s second nature.

Think like an architect. Communicate like a teacher.
