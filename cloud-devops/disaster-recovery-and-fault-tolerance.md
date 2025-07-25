Disaster Recovery & Fault Tolerance
“Hope for the best, design for the worst.”

In distributed systems, failures are inevitable—whether from hardware crashes, network partitions, bugs, or human error. The goal isn’t to prevent all failures—it’s to survive them gracefully.

🚨 Definitions First
Term	Meaning
Disaster Recovery	A planned strategy to restore service after a major outage
Fault Tolerance	The system’s ability to keep working despite component failures

🧱 Fault Tolerance: Build for Resilience
🔹 Key Principles
Concept	Explanation
Redundancy	Duplicate services/resources to remove single points of failure
Graceful Degradation	Partially serve users (e.g., read-only mode) rather than crash
Failover Systems	Automatic switching to backup systems (e.g., replica DBs)
Timeouts & Retries	Don’t hang forever—fail fast and retry with backoff
Circuit Breakers	Stop calling broken services to avoid cascading failures
Load Balancing	Distribute load evenly to avoid overloading any component

⚙️ Components That Need to Be Fault Tolerant
Databases (primary/replica)

Caches (Redis with cluster mode)

Queues (durable, persistent)

Load balancers (with health checks)

Microservices (stateless, auto-recovering)

File storage (replication, backups)

🌪️ Disaster Recovery: Prepare for the Unexpected
🔐 Plan Components
Element	Example/Note
RPO (Recovery Point Objective)	Max data loss (e.g., last 15 min of data is tolerable)
RTO (Recovery Time Objective)	Max time to restore service (e.g., must be back within 1hr)
Backups	Regular snapshots of DB, files, config (stored offsite/cloud)
Standby Environments	Cold (offline), warm (partially active), hot (always-on)
Failover Testing	Regularly simulate outages to test recovery readiness
Runbooks	Step-by-step guides for outages, maintained and tested

📊 DR Readiness Levels
text
Copiar
Editar
Level 0 – No plan, hope it never breaks 😨
Level 1 – Backups only
Level 2 – Backups + manual restore
Level 3 – Backups + automated restore + monitoring
Level 4 – Active-active failover, DR drills
🏗️ Architecting for Fault Tolerance (Diagram - Text Form)
text
Copiar
Editar
                +-------------+
   Users  --->  | Load Balancer| ---> [Primary Service]
                +-------------+         |
                                       [Fallback Service]
                                          |
                                      [Database Cluster]
                                    /      |      \
                            [Replica1] [Replica2] [Read-Only]
🧪 Real-World Strategies
Platform	Strategy
AWS	Multi-AZ DBs, Route53 health checks, S3 versioning
Netflix	Chaos Monkey to simulate random failures
Google	Global load balancing + edge caching
Kubernetes	Pod auto-restart, liveness probes, node affinity

🧠 Interview Insight
“In mission-critical systems, I typically recommend an active-active architecture across regions to reduce RTO and RPO to near-zero. Add health checks, autoscaling, and regular failover tests as part of your CI/CD.”

✅ Summary Table
Dimension	Fault Tolerance	Disaster Recovery
Focus	Preventing downtime	Recovering from downtime
Timeline	Real-time	Post-failure (minutes to hours)
Cost	Higher (redundant infra)	Moderate (depends on setup)
Best For	Always-on systems (e.g. payments, search)	Data-critical systems (e.g. storage)
Must-Have	Load balancing, retries, failover	Backups, RPO/RTO planning, DR drills

🧰 Tools to Know
Backups: AWS Backup, Velero, RDS snapshots

Monitoring: Prometheus, Grafana, Datadog

Chaos Testing: Gremlin, Chaos Monkey

DR Automation: Runbooks, Terraform state restore, cloud snapshots

Multi-region deployment: Cloudflare, AWS Global Accelerator, GCP Load Balancing