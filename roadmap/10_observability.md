# 📊 Observability — Prometheus, Grafana, Loki, ELK & Tracing

> **Goal:** Build the ability to deeply understand what's happening inside production systems through metrics, logs, and traces.
>
> **Target:** 0–1 year → can design and operate a production observability stack

---

## 📚 Resources

### Primary
- **Prometheus docs:** https://prometheus.io/docs/introduction/overview/
- **Grafana docs:** https://grafana.com/docs/
- **OpenTelemetry:** https://opentelemetry.io/docs/
- **Google SRE Book (free) — Chapter: Monitoring Distributed Systems:** https://sre.google/sre-book/monitoring-distributed-systems/

### Books
- *Prometheus: Up & Running* — Brian Brazil (O'Reilly)
- *Observability Engineering* — Charity Majors, Liz Fong-Jones, George Miranda
- *Site Reliability Engineering* — Google (free online, essential reading)

### Courses / Videos
- **TechWorld with Nana — Prometheus + Grafana:** https://youtu.be/QoDqxm7ybLc
- **KodeKloud Prometheus:** https://kodekloud.com/courses/prometheus-certified-associate/
- **Grafana YouTube channel:** https://www.youtube.com/@GrafanaLabs

### Practice
- **Grafana Play (online sandbox):** https://play.grafana.org/
- **Prometheus demo:** https://demo.prometheus.io/
- **KodeKloud observability labs:** https://kodekloud.com/

---

## Stage 0 — The Four Golden Signals & Observability Theory

### Learn
- The 3 pillars of observability: Metrics, Logs, Traces
- The Four Golden Signals: Latency, Traffic, Errors, Saturation
- RED Method (Rate, Errors, Duration) — for services
- USE Method (Utilization, Saturation, Errors) — for resources
- SLI, SLO, SLA, Error Budgets
- Observability vs monitoring: what's the difference?

### Resources
- https://sre.google/sre-book/monitoring-distributed-systems/
- https://grafana.com/blog/2018/08/02/the-red-method-how-to-instrument-your-services/
- https://www.brendangregg.com/usemethod.html

### 🟢 Easy Exercises
1. For a web API, identify the four golden signals and write down what metric you would use for each
2. Write your own SLO for a hypothetical API: "99.9% of requests complete in under 300ms" — calculate the allowed error budget per month in minutes
3. Explain the difference between SLI and SLO in your own words with a concrete example

### 🟡 Medium Exercises
1. Calculate: if your SLO is 99.9% availability per month, how many minutes of downtime are you allowed? At 99.95%? At 99.99%?
2. Design the full observability strategy for a 3-tier app: what metrics, logs, and traces would you collect at each tier?

---

## Stage 1 — Prometheus Fundamentals

### Learn
- Prometheus architecture: scraping, data model, storage
- Metric types: Counter, Gauge, Histogram, Summary
- PromQL: selectors, operators, functions (`rate()`, `increase()`, `sum()`, `by()`)
- Labels: the power of multi-dimensional data
- Exporters: node_exporter, blackbox_exporter, mysqld_exporter
- Alertmanager: routes, receivers, grouping, inhibition

### Resources
- **PromQL cheat sheet:** https://promlabs.com/promql-cheat-sheet/
- **PromQL for beginners:** https://valyala.medium.com/promql-tutorial-for-beginners-9ab455142085
- **Awesome Prometheus:** https://github.com/roaldnefs/awesome-prometheus

### Guided Examples
```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
      - targets: ['node-exporter:9100']
```

```promql
# Rate of HTTP requests per second over last 5 minutes
rate(http_requests_total[5m])

# Error rate percentage
rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m]) * 100

# 95th percentile latency
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))

# CPU usage percentage per node
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

### 🟢 Easy Exercises
1. Run Prometheus locally with Docker, expose `node_exporter`, and verify metrics are being scraped at `http://localhost:9090/targets`
2. Write a PromQL query that shows total HTTP request count for the last hour
3. Write a PromQL query that shows the current memory usage percentage of a node
4. Create a simple Alertmanager alert: trigger when CPU > 80% for 5 minutes, send to a webhook

### 🟡 Medium Exercises
1. Write a recording rule that pre-computes `job:http_request_rate5m:mean` to speed up dashboard queries
2. Configure Alertmanager with: route to Slack for warnings, PagerDuty for critical, and silence business-hours alerts
3. Write PromQL queries for the four golden signals of an nginx service

### 🔴 Hard Exercises
1. Write a custom Prometheus exporter in Python using `prometheus_client` that exposes metrics from a MySQL database (connections, slow queries, replication lag)
2. Implement recording rules and alerting rules for a complete SLO: 99.9% success rate with error budget burn rate alerts

### 🏗 DevOps Project
Deploy a full Prometheus stack on Kubernetes using the Prometheus Operator: ServiceMonitors for all services, recording rules for the four golden signals, Alertmanager with Slack + email routing, and retention policy.

---

## Stage 2 — Grafana Dashboards

### Learn
- Grafana UI: data sources, panels, variables, annotations
- Panel types: time series, stat, gauge, bar chart, table, heatmap, logs
- Dashboard variables for dynamic filtering
- Alerting in Grafana
- Dashboard-as-code with Grafonnet or Terraform provider
- Grafana provisioning (dashboards and data sources via files)

### Resources
- https://grafana.com/docs/grafana/latest/dashboards/
- **Grafana dashboard best practices:** https://grafana.com/blog/2022/06/06/grafana-dashboards-best-practices/
- **Grafana Labs YouTube tutorials:** https://www.youtube.com/@GrafanaLabs

### 🟢 Easy Exercises
1. Connect Grafana to your Prometheus instance, create a simple time series panel showing node CPU usage
2. Create a variable for `instance` that filters all panels in a dashboard by node
3. Import a community dashboard (e.g., Node Exporter dashboard ID: 1860) from Grafana.com

### 🟡 Medium Exercises
1. Build a complete Node Exporter dashboard from scratch (without importing): CPU, memory, disk, network panels with proper thresholds and colors
2. Create an SLO dashboard: current error rate, 30-day error budget remaining, burn rate alert status

### 🔴 Hard Exercises
1. Provision a Grafana dashboard via a JSON file mounted as a ConfigMap in Kubernetes — the dashboard appears automatically when Grafana starts
2. Write a Terraform config that creates Grafana data sources, folders, and dashboards via the Grafana Terraform provider

---

## Stage 3 — Log Management

### Learn
- **Grafana Loki:** labels-based log aggregation (like Prometheus for logs)
- **Promtail:** log shipping agent for Loki
- **LogQL:** Loki's query language (similar to PromQL)
- **ELK Stack:** Elasticsearch, Logstash, Kibana
- **EFK Stack:** Elasticsearch, Fluentd, Kibana
- **Structured logging:** JSON logs vs unstructured
- Log levels and when to use each

### Resources
- **Loki docs:** https://grafana.com/docs/loki/latest/
- **LogQL tutorial:** https://grafana.com/docs/loki/latest/query/
- **Elastic docs:** https://www.elastic.co/guide/index.html
- **Fluentd docs:** https://www.fluentd.org/

### 🟢 Easy Exercises
1. Run Loki + Promtail + Grafana with Docker Compose and ship your nginx logs to Loki
2. Write a LogQL query that shows all ERROR logs from nginx in the last hour
3. Write a LogQL query that counts log lines by log level over time (as a time series)

### 🟡 Medium Exercises
1. Set up log-based alerting in Grafana: alert when the rate of ERROR logs exceeds 10/minute
2. Set up Fluentd on a Kubernetes cluster to ship pod logs to Elasticsearch, then visualize in Kibana
3. Compare Loki vs ELK: write a 1-page comparison of cost, complexity, and use cases

### 🔴 Hard Exercises
1. Parse unstructured nginx logs with a Promtail pipeline to extract: IP, status, latency as labels and fields
2. Set up log correlation: link a Grafana trace to the specific log lines that occurred during that trace

---

## Stage 4 — Distributed Tracing

### Learn
- What is distributed tracing and why it matters for microservices
- OpenTelemetry: the standard for instrumentation
- Trace, Span, Parent/Child span relationships
- Sampling strategies: head-based vs tail-based
- Jaeger and Tempo (tracing backends)
- Integrating traces with Grafana

### Resources
- **OpenTelemetry docs:** https://opentelemetry.io/docs/
- **Jaeger docs:** https://www.jaegertracing.io/docs/
- **Grafana Tempo:** https://grafana.com/docs/tempo/latest/

### 🟢 Easy Exercises
1. Instrument a simple Python Flask app with OpenTelemetry to emit spans for each HTTP request
2. Run Jaeger locally with Docker and visualize your Flask app's traces
3. Identify the slowest span in a sample trace and explain what it tells you

### 🟡 Medium Exercises
1. Add custom spans to your app: instrument the database query and external API call as separate child spans
2. Connect Grafana to Jaeger or Tempo and create a panel showing trace error rates over time

---

## Stage 5 — Alerting Best Practices & On-Call

### Learn
- Alerting philosophy: alert on symptoms, not causes
- Alert fatigue: the danger of too many alerts
- Severity levels: critical, warning, info
- Runbooks: what to do when an alert fires
- PagerDuty / Opsgenie: on-call rotation, escalation
- Post-mortems: blameless culture, 5-why analysis

### Resources
- **Google SRE Book — Alerting on SLOs:** https://sre.google/workbook/alerting-on-slos/
- **Rob Ewaschuk's "Philosophy on Alerting":** https://docs.google.com/document/d/199PqyG3UsyXlwieHaqbGiWVa8eMWi8zzAn0YfcApr8Q/

### 🟢 Easy Exercises
1. Review 10 Prometheus alerts from the `kubernetes-mixin` project — classify each as "symptom-based" or "cause-based"
2. Write a runbook for a "High CPU Alert": what are the first 5 steps to investigate?
3. Write a blameless post-mortem for a real or simulated incident using the standard 5-section template

### 🟡 Medium Exercises
1. Design the complete alerting strategy for a Kubernetes platform: what alerts are critical vs warning? What has a runbook?
2. Implement multi-window, multi-burn-rate SLO alerts using Prometheus recording rules

### 🏗 Capstone Project
Deploy a full production observability stack on Kubernetes:
- Prometheus Operator with ServiceMonitors for all services
- Grafana with provisioned dashboards (node, app, SLO)
- Loki + Promtail for centralized logging
- Alertmanager with Slack + email routing
- One SLO with error budget burn rate alerts and a runbook

---

## 🔗 Additional Observability Resources

### Tools
- **Prometheus Operator:** https://github.com/prometheus-operator/prometheus-operator
- **kube-prometheus-stack (all-in-one Helm chart):** https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack
- **victoria-metrics (Prometheus alternative):** https://victoriametrics.com/
- **Thanos (Prometheus long-term storage):** https://thanos.io/

### Certification
- **Prometheus Certified Associate (PCA):** https://training.linuxfoundation.org/certification/prometheus-certified-associate/

---

*Progress: 0/6 stages complete · Est. time to complete: 6–8 weeks at 3 hrs/day*
