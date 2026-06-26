# Lab 4.2 – PACE Handover & Post-Incident Report (PIR)

## Objective

The objective of this lab was to learn how enterprise support teams perform structured shift handovers using the PACE framework and document incidents using a Post-Incident Report (PIR).

These practices ensure operational continuity, effective communication between support engineers, and continuous service improvement.

---

# Environment

Repository

cogs-lab-portfolio

Module

module4

Artifacts

- handovers/
- pirs/

---

# Part 1 – PACE Handover

PACE is a structured method used during support shift transitions.

## P – Priority

Identify all high-priority incidents requiring continued attention.

Example:

- MFA SMS OTP Failure
- Priority: P2
- Status: Waiting on Customer

Purpose:

Ensures critical issues remain visible during shift changes.

---

## A – Actions Taken

Document completed troubleshooting activities.

Example

- Verified customer information
- Reviewed SMS gateway logs
- Escalated issue to L2
- Activated backup sender ID

Purpose

Prevent duplicate work during the next shift.

---

## C – Context

Explain the current situation.

Example

Carrier-side DND policy blocked the primary sender ID. Customer testing is pending after configuration changes.

Purpose

Provides engineers with the complete background before continuing work.

---

## E – Expectations

Describe what the next shift should accomplish.

Example

- Wait for customer confirmation
- Verify successful OTP delivery
- Close incident if testing succeeds
- Continue monitoring

Purpose

Clearly defines ownership and next actions.

---

# Benefits of PACE

- Consistent communication
- Reduced knowledge loss
- Faster issue resolution
- Improved shift continuity

---

# Part 2 – Post-Incident Report (PIR)

A PIR documents the complete lifecycle of an incident after resolution.

---

## Incident Summary

Service

MFA SMS Authentication

Priority

P2 – High

Status

Resolved

---

## Timeline

09:15 – Incident reported

09:30 – Investigation started

09:45 – Gateway logs reviewed

10:00 – Escalated to L2

10:30 – Backup sender ID activated

11:00 – Customer confirmed restoration

11:15 – Incident closed

---

## Root Cause

The primary SMS sender ID was rejected due to carrier DND policy, preventing OTP delivery.

---

## Impact Assessment

- 25 users affected
- Finance department unable to authenticate
- Email OTP remained operational
- No data loss

---

## Resolution

- Activated backup sender ID
- Validated SMS gateway configuration
- Verified OTP delivery
- Confirmed service restoration with customer

---

## Preventive Actions

- Continuous sender ID monitoring
- Automatic backup sender activation
- Regular carrier compliance verification
- Scheduled health checks for SMS gateways

---

## Open Items

- Monitor SMS delivery for 24 hours
- Update operational documentation
- Review carrier whitelist configuration

---

# Importance of PIR

A Post-Incident Report helps organizations:

- Understand root causes
- Prevent recurring incidents
- Improve operational processes
- Capture lessons learned
- Strengthen service reliability

---

# Learning Outcome

This lab demonstrated how structured documentation supports enterprise operations. The PACE handover ensured seamless communication between support shifts, while the Post-Incident Report captured the incident timeline, root cause, resolution, and preventive actions. Together, these practices improve operational efficiency, accountability, and continuous improvement within IT support teams.
