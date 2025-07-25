# What Is System Design?
```
        ┌───────────────────────┐
        │  What is System Design?  │
        └──────────┬──────────────┘
                   │
          ┌───────┴───────┐
          ▼               ▼
    ┌───────────┐   ┌───────────┐
    │ Components │   │Characteristics│
    └─────┬─────┘   └─────┬─────┘
          │               │
      ┌───┴───┐       ┌───┴───┐
      ▼       ▼       ▼       ▼
┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐
│Services │ │Infra    │ │Flows    │ │UX       │
├─────────┤ ├─────────┤ ├─────────┤ ├─────────┤
│• APIs   │ │• Servers │ │• Client→ │ │• Latency │
│• DBs    │ │• LB      │ │• Service→│ │• Errors  │
└─────────┘ │• Cache   │ │→ DB     │ └─────────┘
            └─────────┘ └─────────┘
``` 
System design is the process of defining the architecture, components, modules, interfaces, and data of a system to meet specific requirements. It's essentially a blueprint for building and implementing a system, ensuring it fulfills functional, technical, and business needs. This involves outlining a structured plan for how different parts of the system will interact and work together.

### In simple terms:
"How would you build [X] to handle [Y users or load]?"

### For example:
- How would you build Instagram for 100 million users?
- How would you design a messaging system that never loses messages?
- How would you store terabytes of images with low-latency retrieval?
 
## System Design vs Software Design
```
┌───────────────────────┬───────────────────────┐
│    System Design      │    Software Design    │
├───────────────────────┼───────────────────────┤
│ Focuses on:           │ Focuses on:           │
│ • Entire system       │ • Individual          │
│   architecture        │   components/modules  │
│ • Scalability         │ • Code structure      │
│ • Reliability         │ • Algorithms          │
│ • Infrastructure      │ • Design patterns     │
│ • Data flows          │ • Class diagrams      │
│                       │                       │
│ Deals with:           │ Deals with:           │
│ • Distributed systems │ • Implementation      │
│ • Microservices       │   details             │
│ • Load balancing      │ • SOLID principles    │
│ • Databases           │ • Clean code          │
│ • Caching             │                       │
└───────────────────────┴───────────────────────┘
```
  
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
