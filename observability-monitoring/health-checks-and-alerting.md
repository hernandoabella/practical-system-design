Health Checks & Alerting
"You can’t fix what you don’t monitor, and you can’t monitor what you don’t check."
Health checks and alerting are critical components of any production-grade system to ensure reliability, uptime, and fast issue response.

✅ Why They Matter
Purpose	Description
Proactive Monitoring	Detect issues before they become outages
Fast Incident Response	Alert SREs/devs instantly when something breaks
Auto-remediation	Restart unhealthy services (e.g., in Kubernetes)
SLI/SLO Compliance	Keep system health within promised targets

🧪 Types of Health Checks
Type	Description	Example
Liveness	Checks if service is alive (i.e., not stuck or crashed)	/healthz → 200 OK
Readiness	Checks if service is ready to receive traffic	/readyz → DB connected
Startup	Checks if service finished booting	Kubernetes probes

These are especially useful in container orchestration platforms like Kubernetes, which can kill/restart pods based on probe results.

🏗️ Implementation Example (Express.js)
js
Copiar
Editar
app.get('/healthz', (req, res) => {
  res.status(200).send('OK');
});

app.get('/readyz', async (req, res) => {
  const dbConnected = await checkDatabaseConnection();
  dbConnected ? res.status(200).send('Ready') : res.status(500).send('DB not ready');
});
🧠 Health Check Best Practices
Keep endpoints fast and lightweight

Don’t expose sensitive data

Separate liveness from readiness

Use caching if external checks are expensive

Avoid false positives by adding grace periods

📢 Alerting: How It Works
Component	Description
Monitoring System	Detects anomalies using metrics/logs/traces
Alert Manager	Sends alerts via Slack, SMS, PagerDuty, etc.
Receivers	Devs, SREs, on-call teams

Popular stacks: Prometheus + Alertmanager, Datadog, Grafana OnCall, New Relic, etc.

⚙️ Alerting Example (Prometheus)
Rule: Alert if HTTP errors > 5% over 5 minutes

yaml
Copiar
Editar
groups:
- name: http-alerts
  rules:
  - alert: HighErrorRate
    expr: rate(http_requests_total{code="500"}[5m]) 
          / rate(http_requests_total[5m]) > 0.05
    for: 2m
    labels:
      severity: "critical"
    annotations:
      summary: "High 500 error rate detected"
🔔 Alerting Best Practices
Use “for” clauses to avoid flapping

Group related alerts to reduce noise

Set severity levels: info, warning, critical

Link alerts to dashboards or runbooks

Rotate on-call schedules with escalation paths

📊 Dashboards to Support Alerting
Tool	Use Case
Grafana	Visualize metrics, traces
Kibana	Analyze logs
Datadog	Full-stack observability
New Relic	APM + alerting

Good dashboards help verify alerts, track trends, and investigate root causes.

🔐 Security Note
Don’t expose health endpoints publicly

If needed, protect them with auth or IP whitelisting

Mask internal details from external systems

💬 Interview Soundbite
"For production services, I’d implement /healthz for liveness and /readyz for readiness. Paired with Prometheus alerts and Grafana dashboards, we can detect issues early, alert the team, and maintain high uptime."

✅ Summary
Health checks tell if a system is alive and well

Alerting tells someone to fix it when it's not

Together, they form the backbone of site reliability engineering (SRE)