# Elasticsearch Deployment and Prometheus Monitoring

## 1. How Prometheus Works

Prometheus is an open-source monitoring system used for collecting metrics from different sources. It is designed for reliability and scalability.

### **Architecture & Components:**

- **Prometheus Server**: Prometheus works by periodically "scraping" metric data from configured endpoints (targets) on applications, servers, and other infrastructure components via HTTP requests, storing this data in a time-series database, and allowing users to query and analyze the metrics using its dedicated query language (PromQL) to monitor system health and trigger alerts when specific conditions are met; essentially, it pulls data from targets at regular intervals instead of pushing data to it, making it a pull-based monitoring system. 
- **Exporters**: Collect metrics from applications.
- **Pushgateway**: Supports short-lived jobs to push metrics.
- **Alertmanager**: Handles alerts and routes them to notification channels.
- **PromQL**: A query language for data retrieval and analysis.

### **Workflow:**

1. Prometheus scrapes metrics from endpoints exposed by applications.
2. The scraped metrics are stored in a time-series database.
3. Alertmanager processes and routes alerts based on predefined rules.
4. Grafana can be used to visualize metrics in dashboards.

## 2. Create Custom Prometheus Alerts & Rules

### **Defining Alerting Rules**

To create custom alerts, define rules in Prometheus configuration files.

### **Example Alert Rule (YAML Configuration)**

Below is a Prometheus alerting rule for detecting high CPU usage:

```yaml
groups:
  - name: example.rules
    rules:
      - alert: HighCPUUsage
        expr: 100 * (1 - avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m]))) > 80
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "High CPU Usage Detected"
          description: "CPU usage is above 80% for more than 2 minutes."
```

This rule triggers an alert if CPU usage remains above 80% for 2 minutes.

## 3. PromQL Query for Grafana

To visualize an application metric (counter) trend in Grafana, use this PromQL query:

```sql
rate(my_application_metric_total[5m])
```
