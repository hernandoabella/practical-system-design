What is System Design?

System Design is the process of defining the architecture, components, modules, and data flow of a system to meet specific requirements — whether it's a simple app or a globally distributed platform.

Think of it like being the architect, not the builder.

It includes:

Choosing the right building blocks (databases, queues, caches)

Designing scalable communication between services

Making tradeoffs based on performance, cost, and complexity

🛠 Real-World Examples:

How Instagram handles millions of stories every minute

How Uber matches drivers and riders in real-time

🤝 Interview vs Real-World System Design

Aspect

Interviews

Real-World

Time Constraint

45–60 minutes

Weeks or months of iterations

Scope

Narrow use-case

Entire product experience

Tools

Whiteboard or Excalidraw

Diagrams, architecture docs, Terraform

Focus

Clarity, tradeoffs, communication

Reliability, maintainability, team alignment

🧠 Tip: Interviews reward structured thinking and clear assumptions — not perfect systems.

🏗️ Core System Qualities

Every system needs to balance 3 core principles:

Scalability – Can it grow with more users or data?

Availability – Is it up and responsive when needed?

Reliability – Does it behave as expected consistently?

You don’t always need 100% of all three. The art is in choosing what to sacrifice — and when.

🧾 Types of Systems (Know What You’re Designing)

Type

Use Case Examples

OLTP (Online Transaction Processing)

Payments, Banking, CRUD apps

OLAP (Online Analytical Processing)

Dashboards, Analytics, Big Data

Batch Systems

Monthly reports, Data pipelines

Real-Time Systems

Messaging, Games, IoT devices

⚡️ Real systems often combine multiple types — e.g., YouTube has batch jobs + real-time video delivery.

🧪 Mini Exercise

"Design a system that lets users upload images and view them in a feed."

What kind of system is this?

What qualities matter most? (availability? cost? scale?)

Sketch a high-level architecture in 2 minutes
