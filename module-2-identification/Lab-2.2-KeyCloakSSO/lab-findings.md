# Lab 2.2 – Keycloak SSO, Attribute Mapping and Troubleshooting

## Objective

The objective of this lab was to deploy Keycloak as an Identity Provider (IdP), configure Single Sign-On (SSO), understand SAML attribute mapping, and troubleshoot authentication issues encountered during integration.

---

## Environment Setup

### Infrastructure

| Component             | Purpose                 |
| --------------------- | ----------------------- |
| VM1                   | Keycloak Server         |
| Docker                | Container Runtime       |
| Keycloak              | Identity Provider (IdP) |
| SAML Test Application | Service Provider (SP)   |
| AWS EC2               | Cloud Infrastructure    |

---

## Overview

Keycloak is an open-source Identity and Access Management (IAM) platform that provides:

* Single Sign-On (SSO)
* Identity Federation
* Multi-Factor Authentication (MFA)
* User Management
* Role-Based Access Control (RBAC)

In this lab, Keycloak was deployed using Docker and configured as a SAML Identity Provider.

---

## Keycloak Deployment

Keycloak was deployed in a Docker container.

Example verification:

```bash id="e8tpr3"
docker ps
```

Verified:

* Keycloak container running
* Port 8080 exposed
* Admin console accessible

---

## Realm Configuration

A dedicated realm was created:

```text id="yvymh0"
instasafe-lab
```

Purpose:

* Isolate authentication policies
* Manage users independently
* Configure SAML integrations

---

## User Creation

Test users were created inside the realm.

Example user attributes:

| Attribute | Value                                                       |
| --------- | ----------------------------------------------------------- |
| Username  | testuser                                                    |
| Email     | [testuser@instasafe.local](mailto:testuser@instasafe.local) |
| Enabled   | Yes                                                         |

---

## SAML Client Configuration

A SAML client was created to simulate a Service Provider.

Responsibilities:

* Redirect authentication requests to Keycloak
* Receive SAML assertions
* Establish trust relationship with the IdP

---

# Attribute Mapper Explanation

## What is an Attribute Mapper?

An Attribute Mapper is a Keycloak component that controls how user information is included inside a SAML assertion or token.

It acts as a translator between:

```text id="mjlwmr"
Keycloak User Attributes
          ↓
Attribute Mapper
          ↓
SAML Assertion
```

---

## Purpose of Attribute Mapping

Attribute Mappers are used to send user information to applications.

Examples:

| User Attribute | SAML Attribute   |
| -------------- | ---------------- |
| username       | NameID           |
| email          | email            |
| firstName      | givenName        |
| lastName       | surname          |
| roles          | group membership |

---

## Example Flow

User:

```text id="y5n0cx"
Username: testuser
Email: testuser@instasafe.local
```

After mapping:

```xml id="9bjp3t"
<saml:Attribute Name="email">
    <saml:AttributeValue>
        testuser@instasafe.local
    </saml:AttributeValue>
</saml:Attribute>
```

The Service Provider can then use this information for authorization decisions.

---

## Benefits of Attribute Mapping

### Identity Propagation

User identity is passed securely to applications.

### Authorization Support

Applications can make access decisions based on:

* Roles
* Groups
* Department
* Email

### Reduced User Management

Applications do not need local user databases.

---

# SSO Authentication Flow

The implemented flow was:

```text id="f9oq2r"
User
 ↓
Service Provider
 ↓
Keycloak (IdP)
 ↓
Authentication
 ↓
SAML Assertion
 ↓
Service Provider
 ↓
Access Granted
```

---

# Troubleshooting Performed

## Issue 1: HTTPS Required Error

### Observed

When accessing:

```text id="4k6ij5"
/admin
```

Keycloak displayed:

```text id="ztrh7j"
HTTPS Required
```

### Root Cause

The realm was configured to require SSL for authentication requests.

Keycloak was rejecting HTTP requests for security reasons.

### Resolution

SSL requirements were temporarily disabled for lab purposes.

Updated:

```text id="kqk70e"
Realm Settings
→ Login
→ SSL Required
→ None
```

Result:

```text id="7cl5wo"
Admin console became accessible over HTTP.
```

---

## Issue 2: Login Error (ssl_required)

### Observed

Container logs showed:

```text id="2kuzdi"
LOGIN_ERROR
error=ssl_required
```

### Investigation

Docker logs were reviewed:

```bash id="qzq7ua"
docker logs keycloak
```

Logs confirmed that authentication attempts were being rejected because SSL enforcement was active.

### Resolution

Disabled SSL requirement within realm settings.

Authentication succeeded afterward.

---

## Issue 3: SAML Test Application Not Redirecting Properly

### Observed

The Flask-based SAML test application failed to complete the login flow.

### Investigation

Reviewed:

* Callback URLs
* Realm configuration
* SAML endpoint configuration

Verified that the generated SAML request was reaching Keycloak.

### Resolution

Corrected endpoint configuration and SSL settings.

---

## Issue 4: Container Accessibility

### Observed

Initial access attempts failed.

### Investigation

Verified:

```bash id="01lzlh"
docker ps
```

Checked:

* Container status
* Port mapping
* Security Group rules

### Resolution

Confirmed:

```text id="gm3q3t"
8080/TCP
```

was allowed in AWS Security Groups.

---

# Security Considerations

## Identity Provider Security

Keycloak centralizes authentication and identity management.

## Secure Assertion Exchange

SAML assertions allow trusted identity sharing between systems.

## SSL Enforcement

HTTPS protects authentication requests and assertions from interception.

## Centralized Access Control

User access policies can be managed from a single location.

---

# Key Learnings

* Deployed Keycloak using Docker.
* Created realms and users.
* Configured a SAML Service Provider.
* Understood SAML authentication flow.
* Learned how Attribute Mappers work.
* Mapped user attributes into SAML assertions.
* Diagnosed SSL-related authentication issues.
* Reviewed container logs for troubleshooting.
* Implemented Single Sign-On concepts using an Identity Provider.

---

# Conclusion

This lab demonstrated the implementation of Single Sign-On using Keycloak and SAML. User authentication was centralized through Keycloak, while Attribute Mappers were used to transfer identity information to applications. Several real-world issues involving SSL enforcement, endpoint configuration, and SAML integration were investigated and resolved, providing practical experience with Identity and Access Management systems.
