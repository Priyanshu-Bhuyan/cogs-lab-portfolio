# Lab 0.1 – Networking Fundamentals

## Lab Information

* **Lab ID:** 0.1
* **Course:** Networking Fundamentals
* **Week:** 1
* **Role:** Developer
* **Duration:** 4 Hours

---

# Objective

The objective of this lab was to create a two-VM virtual network topology, establish connectivity between systems, capture and analyze a TCP three-way handshake, document routing information, and understand the differences between traditional VPN architectures and Zero Trust Network Access (ZTNA).

---

# Environment Setup

## Virtual Machines

### VM-A (Client)

* Operating System: Ubuntu Server 22.04 LTS
* IP Address: 192.168.10.1/24
* Interface: enp0s3
* Purpose: Client system initiating network connections

### VM-B (Server)

* Operating System: Ubuntu Server 22.04 LTS
* IP Address: 192.168.10.2/24
* Interface: enp0s3
* Purpose: Server hosting HTTP service

---

# Network Topology

```text
                 VirtualBox NAT
                       |
                 10.0.3.2
                       |
                 enp0s8 (NAT)
                       |
              VM-A (Client)
              192.168.10.1
                 enp0s3
                       |
          Internal Network: labnet
                       |
                 enp0s3
              VM-B (Server)
              192.168.10.2
```

---

# Connectivity Verification

Both virtual machines were connected using the VirtualBox Internal Network named `labnet`.

Connectivity was verified from VM-A using:

```bash
ping 192.168.10.2
```

Successful ICMP replies confirmed proper communication between the systems.

---

# TCP Three-Way Handshake Analysis

A packet capture was performed using Tshark while VM-A initiated an HTTP request to VM-B.

## HTTP Server

VM-B:

```bash
python3 -m http.server 8080
```

## Packet Capture

VM-A:

```bash
sudo tshark -i enp0s3 -w /tmp/handshake-capture.pcap
```

## Client Request

VM-A:

```bash
curl http://192.168.10.2:8080
```

---

## Handshake Packets

### Packet 1 – SYN

```text
192.168.10.1 → 192.168.10.2
48158 → 8080
[SYN]
```

Purpose:
Client initiates a TCP connection request to the server.

---

### Packet 2 – SYN-ACK

```text
192.168.10.2 → 192.168.10.1
8080 → 48158
[SYN, ACK]
```

Purpose:
Server acknowledges the request and agrees to establish the connection.

---

### Packet 3 – ACK

```text
192.168.10.1 → 192.168.10.2
48158 → 8080
[ACK]
```

Purpose:
Client confirms receipt of the server acknowledgment and completes the TCP connection establishment process.

---

# HTTP Communication Verification

After the TCP handshake was completed, the following application-layer communication was observed:

### HTTP Request

```text
GET / HTTP/1.1
```

### HTTP Response

```text
HTTP/1.0 200 OK
```

This confirms successful data transfer between client and server.

---

# Routing Table Analysis

Command executed:

```bash
ip route show
```

Output Summary:

```text
default via 10.0.3.2 dev enp0s8
10.0.3.0/24 dev enp0s8
192.168.10.0/24 dev enp0s3
```

## Interpretation

### Default Gateway

```text
10.0.3.2
```

Provides Internet connectivity through the VirtualBox NAT network.

### NAT Network

```text
10.0.3.0/24
```

Automatically assigned VirtualBox network used for Internet access.

### Internal Lab Network

```text
192.168.10.0/24
```

Directly connected network used for communication between VM-A and VM-B.

---

# Traditional VPN vs ZTNA

## Traditional VPN

Architecture:

```text
User → VPN Tunnel → Corporate Network → Application
```

Characteristics:

* Provides network-level access.
* Users gain visibility into internal networks.
* Trust is established after authentication.
* Larger attack surface.

---

## Zero Trust Network Access (ZTNA)

Architecture:

```text
User → Identity-Aware Proxy → Specific Application
```

Characteristics:

* Provides application-level access.
* Internal network remains hidden.
* Continuous verification of user identity.
* Smaller attack surface and reduced lateral movement risk.

---

## Key Differences

| Traditional VPN                     | ZTNA                             |
| ----------------------------------- | -------------------------------- |
| Network-level access                | Application-level access         |
| Broad access after login            | Least-privilege access           |
| Internal network exposed            | Applications hidden behind proxy |
| Higher attack surface               | Reduced attack surface           |
| Implicit trust after authentication | Continuous trust verification    |

---

# Challenges Encountered

1. Ubuntu 24.04 compatibility issues within VirtualBox.
2. Network connectivity loss when only Internal Network adapters were configured.
3. DNS resolution failures while installing Tshark.
4. Packet capture file permission issues.
5. VirtualBox clipboard limitations when using Ubuntu Server.

All issues were successfully resolved through network adapter reconfiguration and system troubleshooting.

---

# Conclusion

This lab provided practical experience in configuring virtual networks, assigning static IP addresses, validating connectivity, analyzing TCP connection establishment, understanding routing tables, and comparing traditional VPN architectures with Zero Trust Network Access models. The successful packet capture and protocol analysis reinforced core networking concepts essential for secure application development and troubleshooting.
