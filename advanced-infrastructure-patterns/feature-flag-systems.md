Designing a Feature Flag System
“A feature flag is like a light switch for your code — release safely, iterate quickly, and control what users see in real-time.”

A Feature Flag System (also called a feature toggle) allows teams to enable or disable features without deploying new code. It’s essential for modern development practices like CI/CD, A/B testing, and progressive delivery.

🚦 What Is a Feature Flag?
A feature flag is a conditionally executed block of code, controlled by configuration:

python
Copiar
Editar
if feature_flags['new_checkout']:
    show_new_checkout()
else:
    show_old_checkout()
🎯 Why Use Feature Flags?
Benefit	Description
Safe Releases	Roll out features gradually, monitor & rollback
A/B Testing	Test multiple versions of a feature on real users
Canary Deployments	Enable features for a small % of traffic
Ops Control	Turn off features instantly during an incident
Experimentation	Test ideas without long dev cycles

🧱 Types of Feature Flags
Type	Use Case Example
Release flag	Gradually release new feature to users
Experiment flag	A/B test UI buttons or pricing plans
Ops flag	Turn off payment gateway in emergency
Permission flag	Enable premium features for paid users only
Kill switch	Disable a feature remotely if bugs arise

🧩 System Design Overview
text
Copiar
Editar
+------------+        +-------------------+        +-------------------+
|   Client   | <----> | Feature Flag SDK  | <----> |   Flag Service    |
+------------+        +-------------------+        +-------------------+
                                             |
                                          +--------+
                                          | Config |
                                          +--------+
🔍 Key Components
Feature Flag SDK

Used by client/backend to evaluate flags

Fetches and caches flags from service

Flag Service (Control Plane)

Centralized system to manage all flags

Admin panel to define rollout strategies

Evaluation Engine

Determines if a user sees the feature

Uses rules, segments, targeting

Storage/DB

Stores flag metadata, segments, audit logs

Example: PostgreSQL, Redis, DynamoDB

🧠 Flag Evaluation Logic
Evaluation based on:

User ID or cohort

Country, browser, or device

% rollout (e.g. 20% of users)

Paid/free tier

Random bucketing (for A/B tests)

python
Copiar
Editar
hash(user_id + flag_id) % 100 < 20  # 20% rollout
📊 Admin Interface Features
✅ Toggle on/off per flag

🎯 Target user segments

🔄 Rollout % sliders

📅 Scheduled releases

📈 Metrics & usage tracking

🧻 Rollback button

🔐 Security & Performance
Area	Best Practices
Latency	Use local caching (in-memory, Redis, CDN)
Consistency	Push updates via WebSocket or polling
Audit logs	Track all flag changes
AuthN/AuthZ	Role-based access for devs, PMs, testers

🛠 Tools & Libraries
Tool	Type	Notes
LaunchDarkly	SaaS	Enterprise-grade, powerful targeting
Unleash	Open source	Lightweight, customizable UI
Flagsmith	SaaS + OSS	Hosted or self-hosted
GrowthBook	OSS	Feature flags + experimentation
Split.io	Enterprise	Strong analytics and targeting

📦 API/SDK Sample Use
javascript
Copiar
Editar
if (featureFlags.get("showNewUI")) {
    renderNewUI();
} else {
    renderOldUI();
}
🧪 Real-World Use Cases
Scenario	How Feature Flags Help
Black Friday Sale	Enable sale banner for 100% of users at 12:01AM
Beta Program	Enable beta UI for internal users only
Production Bug	Instantly disable a problematic feature
Experiments on Pricing Page	A/B test two different price layouts

🧮 Scaling Considerations
Use Redis or CDN edge caching to reduce latency

Use pub/sub or push updates to notify clients of changes

Support versioned flags and rollback to previous states

Consider multi-region deployment for global flag access

💬 What to Say in Interviews
“I’d design a centralized flag service with SDKs on clients. The flags would support percentage rollout, segments, and rule-based targeting. Redis would cache evaluations, and config updates would use WebSockets for instant propagation.”

“This lets us release quickly, test features, and recover fast — aligning well with CI/CD and SRE best practices.”

📘 Summary Box
✅ Feature flags decouple deployment from release

🧠 Support experiments, canary rollouts, and quick rollbacks

🔐 Secure, auditable, and segment-aware

⚙️ Integrate via SDKs or API for minimal latency

🚀 Power continuous delivery, experimentation, and user targeting