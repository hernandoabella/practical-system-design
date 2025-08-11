# Requirement Gathering: Asking the Right Questions
Your first job is not building, it is understanding.
Prompts are often vague. Ask questions to uncover what’s missing before you start designing.

## Why Requirement Gathering Matters
Without asking the right questions:
- You risk solving the wrong problem
- You might overbuild or miss key features
- You won’t be able to defend tradeoffs later

In reality, senior engineers often help define the spec—not just implement it.

## What to Ask (Structured Framework)
<img width="630" height="976" alt="System Design Graphics (20)" src="https://github.com/user-attachments/assets/acb4844d-5376-4561-b109-97ec1923b218" />

## Sample Conversation Starters
- “Let’s clarify the core features first so we don’t overbuild.”
- “Are we optimizing for latency, availability, or cost?”
- “Do we expect millions of users at launch or gradual growth?”
- “Should we support analytics, notifications, or file uploads?”
- “Do users need to log in, or is usage anonymous?”

## Mini Case Study – File Sharing Service
### Before designing:
- Upload, view, delete, share?
- Authentication required?
- Versioning needed?
- Max file size?
- **Storage choice:** S3, GCS?
- Expiry or public/private settings?
- UI or API-only?

## Summary Table


💡 Five minutes of smart requirement gathering can save you twenty-five minutes of wrong design.
