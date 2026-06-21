# Lab 3.2 – Infrastructure Observability with Prometheus, Grafana and SigNoz Comparison

## Objective

The objective of this lab was to deploy Prometheus and Grafana for infrastructure monitoring, collect metrics from monitored systems, visualize operational data through dashboards, and compare the Prometheus-Grafana monitoring stack with SigNoz observability platform.

---

## Environment Setup

### Infrastructure

| Component     | Purpose                |
| ------------- | ---------------------- |
| VM1           | Monitoring Server      |
| Prometheus    | Metrics Collection     |
| Grafana       | Visualization Platform |
| Node Exporter | Host Metrics Exporter  |
| Docker        | Container Runtime      |
| AWS EC2       | Cloud Infrastructure   |

---

## Overview

Modern infrastructure requires continuous monitoring to maintain availability, performance, and reliability.

The monitoring stack deployed consisted of:

```text
Node Exporter
      ↓
 Prometheus
      ↓
  Grafana
```

This architecture enables metric collection, storage, querying, and visualization.

---

# Monitoring Architecture

```text
                 ┌─────────────┐
                 │ VM / Server │
                 └──────┬──────┘
                        │
                        ▼
                ┌──────────────┐
                │ Node Exporter│
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ Prometheus   │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ Grafana      │
                └──────────────┘
```

---

# Component Mapping

## Node Exporter

### Purpose

Node Exporter collects operating system metrics.

Examples:

* CPU Usage
* Memory Usage
* Disk Usage
* Network Statistics
* System Load

### Role

Acts as the metric source.

---

## Prometheus

### Purpose

Prometheus collects and stores metrics.

Responsibilities:

* Metric scraping
* Time-series storage
* Query processing
* Alert integration

### Example Query

```promql
up
```

Result:

```text
1
```

Meaning:

```text
Target Reachable
```

---

## Grafana

### Purpose

Grafana visualizes collected metrics.

Responsibilities:

* Dashboard creation
* Real-time visualization
* Performance analysis
* Historical trend analysis

---

# Metrics Collected

## CPU Monitoring

Query:

```promql
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

Purpose:

* Detect CPU bottlenecks
* Monitor resource consumption

---

## Memory Monitoring

Query:

```promql
(1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes) * 100
```

Purpose:

* Monitor memory utilization
* Detect memory pressure

---

## Network Monitoring

Query:

```promql
rate(node_network_receive_bytes_total[5m])
```

Purpose:

* Monitor incoming traffic
* Detect unusual activity

---

# Dashboard Creation

Grafana dashboards were created to visualize:

* CPU Utilization
* Memory Consumption
* Network Traffic
* Service Health

The dashboards provided real-time visibility into infrastructure performance.

---

# Operational Benefits

## Real-Time Monitoring

Provides immediate visibility into system health.

## Historical Analysis

Enables performance trend analysis.

## Capacity Planning

Supports future infrastructure planning.

## Faster Troubleshooting

Reduces Mean Time To Resolution (MTTR).

---

# Prometheus + Grafana vs SigNoz

## Overview

Both solutions provide observability capabilities but differ in architecture and scope.

---

## Prometheus + Grafana

### Architecture

```text
Exporter
   ↓
Prometheus
   ↓
Grafana
```

### Strengths

* Highly flexible
* Large ecosystem
* Industry standard
* Excellent metric collection
* Powerful querying using PromQL

### Limitations

* Multiple components required
* Separate alerting configuration
* Tracing requires additional tools
* More operational complexity

---

## SigNoz

### Architecture

```text
Applications
      ↓
 OpenTelemetry
      ↓
    SigNoz
```

### Strengths

* Unified observability platform
* Metrics
* Logs
* Distributed tracing
* Built-in dashboards
* Simplified deployment

### Limitations

* Smaller ecosystem
* Less mature than Prometheus ecosystem
* Higher resource requirements

---

## Feature Comparison

| Feature               | Prometheus + Grafana | SigNoz   |
| --------------------- | -------------------- | -------- |
| Metrics               | Yes                  | Yes      |
| Dashboards            | Yes                  | Yes      |
| Logs                  | External Integration | Built-In |
| Distributed Tracing   | External Integration | Built-In |
| OpenTelemetry Support | Partial              | Native   |
| Ecosystem Size        | Very Large           | Moderate |
| Complexity            | Higher               | Lower    |
| Customization         | Extensive            | Moderate |

---

# Why Prometheus + Grafana Was Chosen

For this lab, Prometheus and Grafana were selected because:

1. Industry-standard monitoring stack.
2. Widely used in cloud and DevOps environments.
3. Excellent integration with exporters.
4. Strong community support.
5. Ideal for learning observability fundamentals.

---

# Verification Activities

## Container Verification

Verified running containers:

```bash
docker ps
```

Confirmed:

* Prometheus
* Grafana
* Node Exporter

---

## Metrics Verification

Executed:

```promql
up
```

Result:

```text
1
```

Confirmed successful metric collection.

---

## Dashboard Verification

Confirmed:

* CPU graph displaying data
* Memory graph displaying data
* Network graph displaying data

---

# Key Learnings

* Deployed Prometheus monitoring stack.
* Configured Node Exporter metrics collection.
* Connected Grafana to Prometheus.
* Created infrastructure dashboards.
* Executed PromQL queries.
* Monitored CPU, memory, and network metrics.
* Understood observability architecture.
* Compared Prometheus-Grafana with SigNoz.
* Learned strengths and trade-offs of different monitoring solutions.

---

# Conclusion

This lab demonstrated the deployment of a modern monitoring stack using Prometheus and Grafana. Metrics were collected through Node Exporter and visualized using Grafana dashboards. The exercise provided practical experience in observability, metric analysis, and dashboard creation. A comparison with SigNoz highlighted how different observability platforms address monitoring, logging, and tracing requirements in modern cloud environments.
