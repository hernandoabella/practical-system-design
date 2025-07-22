Requirement Gathering: Asking the Right Questions
“Great design starts with great understanding.”

System design interviews (and real projects) often start with incomplete or vague prompts. Your job is to extract clarity. Before you design anything, you must first figure out what you're actually building.

🎯 Why Requirement Gathering Matters
Without asking the right questions:

You risk designing for the wrong goals

You might overbuild or miss critical features

You won’t be able to justify tradeoffs later

📌 In real life, engineers rarely start with a spec—they help define it.

🔍 What to Ask in an Interview
Below is a structured breakdown of questions to guide your discussion.

diff
Copiar
Editar
┌──────────────────────────┐
│ Functional Requirements  │
└──────────────────────────┘
- What is the primary goal of the system?
- What are the core features?
  → e.g., For a chat app: send/receive messages, delete, read receipts?
- Are we building a Minimum Viable Product (MVP) or a full product?

┌────────────────────────────────────┐
│ Non-Functional Requirements (NFRs) │
└────────────────────────────────────┘
- Performance: Latency expectations? (e.g., < 200ms)
- Scale: How many users, QPS, data volume?
- Reliability: Uptime SLA? 99.9%? Failover?
- Consistency: Is eventual consistency okay?
- Security: Auth, encryption, rate limiting?

┌─────────────┐
│ Constraints │
└─────────────┘
- Tech stack limitations? Must use Redis or AWS?
- Legal/compliance (GDPR, HIPAA)?
- Timeline or team size constraints?

┌───────────────┐
│ External APIs │
└───────────────┘
- Are we integrating with payment, email, third-party login?
- Rate limits or failure modes of these APIs?

┌────────────┐
│ User Types │
└────────────┘
- Who uses the system? (admin vs regular user)
- Do permissions differ by role?
- Any anonymous users?

┌──────────────────────┐
│ Data Characteristics │
└──────────────────────┘
- Structured or unstructured?
- Data growth rate?
- Query patterns: reads, writes, aggregations?

┌──────────────────┐
│ Future Additions │
└──────────────────┘
- What’s not needed now, but might be soon?
- Should the design support modularity/extensibility?
💬 Sample Conversation Starters
You can use these during the interview to sound thoughtful and collaborative.

“Let’s clarify the core features first so we don’t overbuild.”

“Are we optimizing for latency, availability, or cost?”

“Do we expect millions of users at launch or gradual growth?”

“Should we support analytics, notifications, or file uploads?”

“Do users need to log in, or is it anonymous usage?”

🧪 Example: Designing a File Sharing Service
Before you draw any diagrams, ask:

Upload file? View files? Delete or share?

Should users authenticate?

Is versioning required?

Max file size?

Storage backend preferences (S3? GCS?)

Expiry or public/private settings?

UI vs API-only?

🧠 Once these are answered, the design becomes focused and realistic.

✅ Summary Table
Category	Sample Questions
Functional Requirements	What should the system do? What are the MVP features?
Non-Functional Requirements	What are the latency, scale, and availability expectations?
Constraints	Are we restricted to certain tools, languages, or budgets?
External Dependencies	Will we use third-party APIs or services?
User Types	Who are the users? Do roles matter?
Data Characteristics	How much data? What kind of queries?
Future Additions	Do we expect rapid feature growth or need for modular design?

🏁 Final Thought
“If you don’t know what to build, even the best architecture is useless.”

A solid 5 minutes of requirement gathering can save you from 25 minutes of incorrect design.