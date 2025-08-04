# What is System Design?
System Design is the process of planning the architecture, components, data flow, and infrastructure of a system to meet specific goals. Think of it as the blueprint for building software systems that are scalable, reliable, and maintainable.

### Example Questions:
- How would you build Instagram for 100 million users?
- How would you design a messaging system that never loses messages?
- How would you store terabytes of images with low-latency retrieval?

## System Design vs Software Design
<img width="864" height="830" alt="Topic (4)" src="https://github.com/user-attachments/assets/ad5f5275-40f3-455a-8fe8-f87e5e179d18" />



  
## What Is a "System"?
```
┌───────────────────────────────────────┐
│            What Is a "System"?        │
├───────────────────────────────────────┤
│ A system is a collection of:          │
│                                       │
│  • Services                           │
│    - APIs                             │
│    - Microservices                    │
│    - Databases                        │
│                                       │
│  • Infrastructure                     │
│    - Servers                          │
│    - Load balancers                   │
│    - Caches (Redis, etc.)             │
│                                       │
│  • Data Flows                         │
│    - Client ↔ Server                  │
│    - Service ↔ Database               │
│    - Service ↔ Service                │
│                                       │
│  • Protocols                          │
│    - HTTP/HTTPS                       │
│    - TCP/UDP                          │
│    - WebSockets                       │
│                                       │
│  • User Experience                    │
│    - Latency                          │
│    - Error handling                   │
│    - Availability (SLA)               │
└───────────────────────────────────────┘
```
### A system is a collection of:
- Services (APIs, microservices, databases)
- Infrastructure (servers, load balancers, caches)
- Data Flows (client to server, service to DB, etc.)
- Protocols (HTTP, TCP, WebSockets)
- User Experience (latency, error handling)
 
## What Makes a “Good” System?
```
┌───────────────────────────────────────┐
│         What Makes a "Good" System?    │
├───────────────────────────────────────┤
│                                       │
│  ╭ Scalable:                          │
│  │ Can handle growth in users/data     │
│  │                                    │
│  ╭ Reliable:                          │
│  │ Works consistently during failures │
│  │                                    │
│  ╭ Maintainable:                      │
│  │ Easy to understand and improve      │
│  │                                    │
│  ╭ Performant:                        │
│  │ Fast responses, optimized resources │
│  │                                    │
│  ╭ Secure:                            │
│  │ Protects data and user access       │
│                                       │
└───────────────────────────────────────┘
```
- A well-designed system is:
- Scalable – Can handle growth in users/data.
- Reliable – Works consistently, even when parts fail.
- Maintainable – Easy to understand, debug, and improve.
- Performant – Fast responses, optimized resources.
- Secure – Protects data and user access.

> A PERFECT system doesn’t exist. Design is about trade-offs.

## Real-World Examples of System Design Questions
- How would you design a URL Shortener like bit.ly?
- How would you store and stream videos like Netflix?
- How would you build a rate limiter to protect your API?
- How would you store billions of photos?

## Why Learn System Design?
- **Think like an architect:** Learn to make smart trade-offs.
- **Build scalable products:** Move beyond "it works on my laptop."
- **Ace technical interviews:** FAANG and startups test system design heavily.
- **Lead teams and systems:** Grow into staff/principal engineer roles.
- **Speak business & tech:** Bridge the gap between product and code.
 
## Key Takeaways
- System Design is about architecting solutions that scale and perform.
- It’s less about code, more about trade-offs, architecture, and infrastructure.
- It’s essential for interviews, high-scale systems, and leadership roles.
- Every system has different needs—there’s no single “right” design, only appropriate trade-offs.
