# Lab 2.1 – OpenLDAP Identity Management

## Objective

The objective of this lab was to deploy and configure an OpenLDAP server for centralized identity management. The lab focused on creating users and groups, performing LDAP queries, and validating authentication mechanisms used in enterprise identity infrastructures.

---

## Environment Setup

### Infrastructure

| Component           | Purpose                          |
| ------------------- | -------------------------------- |
| VM1 (Ubuntu Server) | OpenLDAP Server                  |
| OpenLDAP            | Directory Service                |
| LDAP Utilities      | Directory Management and Queries |
| AWS EC2             | Cloud Infrastructure             |

---

## Overview of OpenLDAP

OpenLDAP is an open-source implementation of the Lightweight Directory Access Protocol (LDAP). It provides a centralized directory service for storing and managing user identities, groups, and authentication information.

Common use cases include:

* Centralized authentication
* User and group management
* Enterprise identity stores
* Single Sign-On integrations
* Access control systems

---

## Implementation Steps

### 1. Installation of OpenLDAP

OpenLDAP server and utilities were installed on the Ubuntu server.

Example packages:

```bash
slapd
ldap-utils
```

The LDAP administrator account and base distinguished name (DN) were configured during installation.

---

### 2. Directory Structure Creation

A basic directory hierarchy was created.

Example:

```text
dc=instasafe,dc=local
│
├── ou=People
└── ou=Groups
```

This structure separates user accounts from group objects.

---

### 3. User Creation

LDAP user entries were created within the People organizational unit.

Example user attributes:

| Attribute    | Description               |
| ------------ | ------------------------- |
| uid          | Unique username           |
| cn           | Common Name               |
| sn           | Surname                   |
| mail         | Email address             |
| userPassword | Authentication credential |

---

### 4. Group Creation

LDAP groups were created to organize users.

Example:

```text
Developers
Administrators
Support
```

Users were assigned to appropriate groups based on access requirements.

---

### 5. LDAP Search Operations

Directory queries were performed using LDAP search commands.

Example:

```bash
ldapsearch
```

Queries were used to:

* Retrieve user information
* Verify directory structure
* Validate object creation
* Inspect group membership

---

### 6. Authentication Testing

Authentication tests were performed using LDAP bind operations.

Successful authentication confirmed:

* User existence
* Correct credentials
* Proper directory configuration

Failed authentication attempts were also tested to validate access control behavior.

---

## Identity Management Component Mapping

### Identity Store

**Mapped Component:** OpenLDAP Directory

Responsibilities:

* Store user identities
* Store group information
* Maintain authentication records

### Authentication Provider

**Mapped Component:** LDAP Bind Operations

Responsibilities:

* Validate user credentials
* Verify account existence
* Control authentication requests

### User Repository

**Mapped Component:** People Organizational Unit

Responsibilities:

* Maintain user records
* Store profile attributes
* Manage identity lifecycle

### Group Management

**Mapped Component:** Groups Organizational Unit

Responsibilities:

* Organize users
* Support role-based access
* Simplify authorization management

---

## Verification Activities

### User Verification

LDAP searches successfully returned created user entries.

Verified:

* Username
* Distinguished Name
* Email
* Group Membership

### Group Verification

LDAP searches confirmed:

* Group creation
* Member assignments
* Directory hierarchy integrity

### Authentication Verification

Successful login tests confirmed:

* Proper credential validation
* Functional LDAP directory services

Failed login attempts confirmed:

* Unauthorized access was denied
* Invalid credentials were rejected

---

## Security Considerations

### Centralized Identity Management

All user identities are stored in a single directory service.

### Reduced Administrative Overhead

User accounts can be managed from one location.

### Role-Based Access Support

Groups simplify authorization and access control.

### Enterprise Integration

LDAP directories can integrate with:

* Keycloak
* Active Directory
* VPN solutions
* SSO platforms
* Identity Providers

---

## Troubleshooting Performed

### LDAP Search Failures

Observed:

* Search queries returned no results

Actions Taken:

* Verified Base DN configuration
* Confirmed LDAP service status
* Checked directory entries

### Authentication Issues

Observed:

* Invalid credential errors

Actions Taken:

* Verified user password
* Confirmed distinguished name format
* Re-tested LDAP bind operations

### Service Validation

Checked:

```bash
systemctl status slapd
```

Verified:

* LDAP service running
* Directory accessible
* Queries responding successfully

---

## Key Learnings

* Understood LDAP directory architecture.
* Configured an OpenLDAP server.
* Created users and groups.
* Performed LDAP searches and queries.
* Tested authentication workflows.
* Learned centralized identity management concepts.
* Understood the relationship between authentication and directory services.
* Prepared the foundation for SSO and Identity Provider integrations.

---

## Conclusion

This lab demonstrated the deployment and management of an OpenLDAP directory service. User accounts, groups, and authentication workflows were successfully configured and tested. The implementation highlighted the importance of centralized identity management and its role as a foundational component of enterprise authentication systems.
