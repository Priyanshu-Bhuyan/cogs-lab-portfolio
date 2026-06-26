# Lab 4.1 – GitHub as a Support Desk

## Objective

The objective of this lab was to understand how GitHub Issues can be utilized as a lightweight support desk for tracking customer incidents, managing ticket lifecycles, documenting investigations, and coordinating support operations.

---

# Environment

- GitHub Repository: cogs-lab-portfolio
- GitHub Issues
- Labels
- Milestones
- GitHub Project Board

---

# Tasks Performed

## 1. Repository Preparation

Used the existing GitHub portfolio repository and created a dedicated Module 4 section for support operations documentation.

Directory Structure:

module4/
- lab4.1.md
- lab4.2.md
- handovers/
- pirs/
- screenshots/

---

## 2. Label Creation

Created labels to categorize support tickets based on priority, product, and workflow status.

Priority Labels

- P1-Critical
- P2-High
- P3-Medium
- P4-Low

Workflow Labels

- status:in-progress
- status:waiting-on-customer
- status:resolved

Product Labels

- product:mfa
- product:ztna
- product:sso

Purpose:

Labels help engineers quickly identify ticket urgency, ownership, and current status.

---

## 3. Milestone

Created the milestone:

Sprint 1 – Support Operations

Purpose:

- Group tickets belonging to the same sprint
- Track completion progress
- Organize support activities

---

## 4. Project Board

Created a Kanban-style board with the following workflow:

Backlog

↓

In Progress

↓

Waiting on Customer

↓

Resolved

↓

Closed

Purpose:

Visual representation of ticket progress throughout its lifecycle.

---

# Support Tickets

## Ticket 1

Title

[P2] MFA SMS OTP not delivered – Finance department, 25 users affected

Priority

P2 – High

Product

MFA

Status Flow

In Progress

↓

Waiting on Customer

↓

Resolved

↓

Closed

Investigation

- Verified registered phone numbers
- Confirmed Email OTP functionality
- Checked SMS gateway logs
- Identified rejected SMS responses
- Escalated to L2 Support

Root Cause

Carrier-side DND policy blocked the primary SMS sender ID.

Resolution

- Activated backup sender ID
- Verified successful OTP delivery
- Customer confirmed issue resolution
- Closed the incident

---

## Ticket 2

Title

[P3] User locked out after password change – AD sync delay suspected

Priority

P3 – Medium

Product

SSO

Investigation

- Password reset verified
- LDAP synchronization reviewed
- Manual synchronization initiated

Expected Outcome

Restore user authentication after directory synchronization.

---

## Ticket 3

Title

[P4] Request for documentation – Configure a secondary ZTNA gateway

Priority

P4 – Low

Product

ZTNA

Request

Customer requested deployment guidance for configuring an additional gateway to improve availability.

Action

Prepared documentation and deployment recommendations.

---

# Ticket Lifecycle

Each support request followed a structured workflow:

Ticket Created

↓

Initial Investigation

↓

Internal Notes

↓

Escalation (if required)

↓

Waiting on Customer

↓

Resolution

↓

Customer Confirmation

↓

Ticket Closed

This lifecycle ensures every issue is documented, investigated, and resolved consistently.

---

# Key Concepts Learned

## GitHub Issues

Used to track incidents and customer requests.

## Labels

Used to classify tickets by:

- Priority
- Product
- Current Status

## Milestones

Organize tickets into support sprints.

## Project Boards

Provide visual tracking of ticket progress.

## Internal Comments

Allow engineers to document investigation notes without losing troubleshooting history.

## Escalation

Used when an issue requires assistance from another support level or team.

---

# Best Practices

- Assign priorities consistently.
- Document every troubleshooting step.
- Use meaningful labels.
- Escalate with sufficient evidence.
- Update customers regularly.
- Close tickets only after customer confirmation.

---

# Learning Outcome

This lab demonstrated how GitHub can effectively serve as a lightweight IT support platform by combining issue tracking, project management, documentation, and collaboration. It also reinforced incident management practices such as prioritization, escalation, customer communication, and structured ticket resolution.
