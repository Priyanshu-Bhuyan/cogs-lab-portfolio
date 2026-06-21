# Lab 3.1 – Uptime Kuma Monitoring, Monitor Type Mapping and Gateway Monitoring Rationale

## Objective

The objective of this lab was to deploy Uptime Kuma, configure multiple monitor types, understand how each monitoring method maps to infrastructure components, and justify the monitoring strategy used for gateway service availability.

---

## Environment Setup

### Infrastructure

| Component   | Purpose                    |
| ----------- | -------------------------- |
| VM1         | Monitoring Server          |
| VM2         | Gateway/Application Server |
| Uptime Kuma | Monitoring Platform        |
| Nginx       | Web Gateway Service        |
| AWS EC2     | Cloud Infrastructure       |

---

## Overview

Uptime Kuma is an open-source monitoring solution used to continuously verify the availability and health of services.

The platform provides:

* HTTP Monitoring
* TCP Port Monitoring
* ICMP Ping Monitoring
* Alerting
* Service Status Dashboards

In this lab, Uptime Kuma was deployed on VM1 while VM2 hosted the monitored Nginx gateway service.

---

# Monitoring Architecture

```text
                    ┌─────────────┐
                    │ Uptime Kuma │
                    │    VM1      │
                    └──────┬──────┘
                           │
         ┌─────────────────┼─────────────────┐
         │                 │                 │
         ▼                 ▼                 ▼
    HTTP Check       TCP Check         Ping Check
         │                 │                 │
         ▼                 ▼                 ▼
      Nginx             SSH Port         VM Network
      Service            (22)           Reachability
                      on VM2
```

---

# Monitor Type Mapping

## 1. HTTP Monitor

### Configured Monitor

```text
Lab Nginx Server
```

### Target

```text
http://<VM2_PUBLIC_IP>
```

### Purpose

The HTTP monitor verifies:

* Web server availability
* HTTP response generation
* Application accessibility
* User-facing service health

### What It Detects

Examples:

* Nginx service stopped
* Web application failure
* HTTP errors
* Gateway outages

### Mapping

| Monitor Type | Infrastructure Component |
| ------------ | ------------------------ |
| HTTP         | Nginx Gateway Service    |

---

## 2. TCP Monitor

### Configured Monitor

```text
Lab SSH Port
```

### Target

```text
Port 22
```

### Purpose

The TCP monitor verifies:

* Port accessibility
* Service listening state
* Network connectivity

### What It Detects

Examples:

* SSH service failure
* Firewall blocking
* Port closure
* Host accessibility issues

### Mapping

| Monitor Type | Infrastructure Component |
| ------------ | ------------------------ |
| TCP          | SSH Service              |

---

## 3. Ping Monitor

### Configured Monitor

```text
Lab Ping Monitor
```

### Purpose

Verifies basic network reachability.

### What It Detects

Examples:

* Host offline
* Network outage
* Routing failure

### Limitation

AWS commonly blocks ICMP traffic.

As a result:

```text
Ping Monitor
↓
May Show DOWN
↓
Even When Service Is Healthy
```

This behavior was observed during the lab.

### Mapping

| Monitor Type | Infrastructure Component |
| ------------ | ------------------------ |
| Ping         | Network Reachability     |

---

# Gateway Monitoring Rationale

## Why Monitor the Gateway?

The Nginx gateway is the first point of entry for users accessing services.

If the gateway becomes unavailable:

```text
Gateway Failure
↓
Users Cannot Access Applications
↓
Service Outage
```

Even if:

* VM is running
* SSH is working
* Network is reachable

Users still experience downtime.

---

## Why HTTP Monitoring Was Chosen

HTTP monitoring validates the complete user experience.

Example:

```text
User
 ↓
HTTP Request
 ↓
Nginx Gateway
 ↓
HTTP Response
```

If any part of this chain fails:

```text
Monitor Status = DOWN
```

This provides more meaningful monitoring than simply checking whether the server is powered on.

---

## Monitoring Strategy

The monitoring approach follows a layered design.

### Layer 1 – Network Monitoring

Monitor:

```text
Ping
```

Purpose:

* Verify basic host reachability

---

### Layer 2 – Service Monitoring

Monitor:

```text
TCP Port 22
```

Purpose:

* Verify operating system accessibility

---

### Layer 3 – Application Monitoring

Monitor:

```text
HTTP
```

Purpose:

* Verify actual service availability

---

# Failure Simulation

## Scenario

The Nginx service was intentionally stopped.

Command:

```bash
sudo systemctl stop nginx
```

---

## Observed Results

### HTTP Monitor

Status:

```text
DOWN
```

Reason:

Nginx stopped responding to requests.

---

### TCP Monitor

Status:

```text
UP
```

Reason:

SSH service remained operational.

---

### VM Availability

Status:

```text
UP
```

Reason:

The server itself remained online.

---

## Analysis

This demonstrated an important monitoring principle:

```text
Server Alive
≠
Application Healthy
```

A host can remain operational while the critical service is unavailable.

---

# Recovery Validation

The service was restored using:

```bash
sudo systemctl start nginx
```

Results:

* HTTP Monitor returned to UP
* Gateway accessible again
* User access restored

Monitoring confirmed successful recovery.

---

# Operational Benefits

## Early Detection

Outages are identified quickly.

## Faster Troubleshooting

Different monitor types help isolate failures.

Examples:

| Status    | Interpretation     |
| --------- | ------------------ |
| Ping Down | Host/Network Issue |
| TCP Down  | Service/Port Issue |
| HTTP Down | Application Issue  |

## Reduced Downtime

Alerts enable faster response and recovery.

---

# Key Learnings

* Deployed Uptime Kuma for infrastructure monitoring.
* Configured HTTP, TCP, and Ping monitors.
* Understood monitor type mapping.
* Learned why gateway services require application-level monitoring.
* Simulated service failures and recovery.
* Observed differences between infrastructure health and application health.
* Applied layered monitoring principles.
* Gained practical experience with availability monitoring.

---

# Conclusion

This lab demonstrated the implementation of service monitoring using Uptime Kuma. Multiple monitor types were configured to provide visibility into network, service, and application health. Through outage simulation and recovery testing, the importance of gateway monitoring and layered observability was validated. The exercise highlighted how effective monitoring reduces downtime and improves operational reliability.
