# Lab 1.3 – WireGuard ZTNA Component Mapping

## Objective

The objective of this lab was to understand the core components of a Zero Trust Network Access (ZTNA) architecture by implementing a secure WireGuard tunnel between two AWS EC2 instances and mapping each component to its corresponding ZTNA role.

---

## Environment Setup

### Infrastructure

| Component           | Purpose                            |
| ------------------- | ---------------------------------- |
| VM1                 | WireGuard Gateway / ZTNA Connector |
| VM2                 | Protected Resource Server          |
| AWS Security Groups | Network Access Control             |
| WireGuard           | Secure Encrypted Tunnel            |
| Ubuntu Server       | Host Operating System              |

### Network Configuration

| VM  | Tunnel IP   |
| --- | ----------- |
| VM1 | 10.0.0.1/24 |
| VM2 | 10.0.0.2/24 |

WireGuard was configured to establish an encrypted peer-to-peer tunnel between both instances.

---

## Implementation Steps

### 1. WireGuard Installation

WireGuard was installed on both EC2 instances using Ubuntu package repositories.

### 2. Key Generation

Public and private key pairs were generated on both VMs.

Example:

```bash
wg genkey | tee privatekey | wg pubkey > publickey
```

These keys were exchanged between peers to establish trust.

### 3. WireGuard Configuration

VM1 was configured as the gateway node:

```ini
[Interface]
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
AllowedIPs = 10.0.0.2/32
```

VM2 was configured as the client node:

```ini
[Interface]
Address = 10.0.0.2/24

[Peer]
Endpoint = <VM1_PUBLIC_IP>:51820
AllowedIPs = 10.0.0.0/24
PersistentKeepalive = 25
```

### 4. Security Group Configuration

The following inbound rules were configured:

| Protocol | Port  | Purpose               |
| -------- | ----- | --------------------- |
| SSH      | 22    | Remote Administration |
| UDP      | 51820 | WireGuard Tunnel      |

---

## ZTNA Component Mapping

### ZTNA Gateway

**Mapped Component:** VM1

Responsibilities:

* Accept incoming WireGuard connections
* Authenticate peer devices using public keys
* Control access to protected resources

### Protected Resource

**Mapped Component:** VM2

Responsibilities:

* Host services accessible only through trusted connections
* Remain isolated from unauthorized traffic

### Identity Verification

**Mapped Component:** WireGuard Public Key Authentication

Responsibilities:

* Verify peer identity
* Establish cryptographic trust
* Prevent unauthorized access

### Secure Tunnel

**Mapped Component:** WireGuard VPN Tunnel

Responsibilities:

* Encrypt all traffic
* Ensure confidentiality and integrity
* Protect data in transit

### Access Control

**Mapped Component:** AWS Security Groups

Responsibilities:

* Restrict inbound and outbound traffic
* Allow only approved protocols and ports

---

## Troubleshooting Performed

### Connectivity Issues

Observed:

* 100% packet loss between peers
* WireGuard handshake not established

Actions Taken:

* Verified peer public keys
* Reviewed AllowedIPs configuration
* Confirmed UDP 51820 rule in Security Groups
* Checked VM public IP addresses
* Validated WireGuard service status

### DNS Resolution Issues

Observed:

```bash
Temporary failure in name resolution
```

Actions Taken:

* Checked DNS configuration
* Restarted systemd-resolved
* Verified internet connectivity using 8.8.8.8
* Identified WireGuard route influence on DNS behavior

### Instance Initialization Delay

Observed:

* VM2 remained in initialization state

Actions Taken:

* Waited for AWS health checks to complete
* Verified instance status checks
* Revalidated network connectivity after initialization

---

## Security Benefits

### Encryption

WireGuard encrypts all traffic using modern cryptographic algorithms.

### Identity-Based Access

Access is granted only to devices possessing trusted cryptographic keys.

### Reduced Attack Surface

Only required ports are exposed through Security Groups.

### Zero Trust Principles

* Verify every connection
* Never trust by default
* Enforce least privilege access
* Continuously validate peer identity

---

## Key Learnings

* Understood the architecture of Zero Trust Network Access.
* Configured a secure WireGuard tunnel between cloud instances.
* Learned public/private key-based authentication.
* Implemented encrypted communication channels.
* Mapped infrastructure components to ZTNA concepts.
* Troubleshot routing, DNS, and connectivity issues in AWS environments.
* Applied Zero Trust principles in a practical cloud deployment.

---

## Conclusion

This lab demonstrated how WireGuard can be used as a lightweight Zero Trust access mechanism. By combining encrypted tunnels, identity-based authentication, and controlled network access, a secure communication path was established between cloud resources while adhering to Zero Trust security principles.
