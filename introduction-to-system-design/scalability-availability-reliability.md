# Scalability, Availability, and Reliability
Modern systems must grow, stay online, and recover from failure. Scalability, availability, and reliability drive most architectural decisions in system design.

## Scalability
Scalability is a system’s ability to handle increased load—more users, more data, more traffic—without a drop in performance.
<br/>
### Types of Scalability:
<img width="747" height="491" alt="System Design Graphics (7)" src="https://github.com/user-attachments/assets/0c1e8543-6cca-4c9f-855e-f118e5d852ca" />

### Example: 
- Instagram feed needs to scale horizontally to serve millions of users at peak times without slowing down.

## Availability
Availability refers to a system’s ability to remain accessible and operational at all times, even during failures or updates.

### Key Terms:
<img width="864" height="410" alt="System Design Graphics (12)" src="https://github.com/user-attachments/assets/d836ab37-0632-47b6-b59e-a97fe29c9566" />

### Example:
- Netflix uses multi-region deployments and redundancy so that if one data center fails, traffic shifts seamlessly to another.

## Reliability
Reliability is the system’s ability to perform consistently and correctly over time, not just up, but also working as expected.

### Characteristics of Reliable Systems:
- Resilience to failures (e.g., retries, circuit breakers)
- Data integrity and correctness
- Graceful degradation: When part of the system fails, the rest still works
- Monitoring and alerting: Detect problems before users do

### Reliability ≠ Availability:
A system might be available but unreliable (e.g., returns wrong data or crashes intermittently).

### Example:
- Banking systems must be extremely reliable—losing or corrupting transaction data is unacceptable.

### How They Interact
<img width="864" height="323" alt="System Design Graphics (8)" src="https://github.com/user-attachments/assets/4e6226a2-a86b-481d-a01c-0b6a67e88430" />

### Concept	Definition	Real Goal
<img width="864" height="257" alt="System Design Graphics (11)" src="https://github.com/user-attachments/assets/d93dd2be-7b4f-4803-9795-fc9e26507979" />
