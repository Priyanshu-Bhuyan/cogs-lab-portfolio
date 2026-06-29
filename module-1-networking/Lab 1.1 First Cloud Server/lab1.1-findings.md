# Lab 1.1 Findings – Your First Cloud Server

## My VM Details

- Provider: AWS EC2
- Region: ap-south-1 (Mumbai)
- Instance Type: t2.micro
- OS: Ubuntu 22.04

---

# Experiment 1: Ping to 8.8.8.8

## Command

```bash
ping -c 5 8.8.8.8
```

## Results

- Average RTT: 0.915 ms
- TTL Value Observed: 119
- Packet Loss: 0%

## Analysis

The ping test successfully reached Google's public DNS server (8.8.8.8) with no packet loss. The average round-trip time was very low, indicating good network connectivity. The TTL value of 119 suggests the packet traversed several network hops before reaching the destination.

---

# Experiment 2: DNS Resolution

## Commands

```bash
ping -c 3 google.com
nslookup google.com
nslookup google.com 1.1.1.1
dig google.com
dig google.com @8.8.8.8 +trace
```

## Results

### Ping google.com

- Resolved IP Address: 142.250.195.174
- Average RTT: 1.394 ms
- TTL: 118

### Default DNS Server

- DNS Server: 127.0.0.53
- Resolved IP: 142.250.195.174

### Cloudflare DNS

- DNS Server: 1.1.1.1
- Resolved IP: 142.250.195.174

### dig Output

- Resolved IP: 142.250.207.14
- Query Time: 2 ms
- Status: NOERROR

## Analysis

Both the local resolver and Cloudflare DNS successfully resolved google.com. Different DNS queries returned slightly different IP addresses because Google uses load balancing and geographically distributed servers.

---

# Experiment 3: Traceroute / DNS Trace

## Command

```bash
dig google.com @8.8.8.8 +trace
```

## Results

The DNS trace displayed the complete DNS resolution chain:

- Root DNS Servers
- .com TLD Servers
- Google Authoritative Name Servers
- Final DNS Record

## Analysis

The trace demonstrates how DNS resolution progresses from the root servers to top-level domain servers and finally to Google's authoritative DNS servers.

---

# Experiment 4: Port Scanning

## Command

```bash
nmap -sV localhost
```

## Results

| Port | Service | Version |
|--------|---------|---------|
| 22/tcp | SSH | OpenSSH 10.2p1 Ubuntu |
| 80/tcp | HTTP | nginx 1.31.2 |

## Analysis

The Nmap scan identified two active services:

1. SSH running on port 22.
2. Nginx web server running on port 80.

998 other ports were closed.

---

# Experiment 5: HTTPS Connection Test

## Command

```bash
curl -v https://www.google.com
```

## Results

- TLS Version: TLSv1.3
- Protocols Offered: HTTP/2 and HTTP/1.1
- SSL Trust Store: `/etc/ssl/certs/ca-certificates.crt`

## Analysis

The HTTPS connection was successfully established using TLS 1.3, which is the latest and most secure TLS version currently in common use.

---

# Experiment 6: Dockerized Nginx Test

## Commands

```bash
docker run -d --name test-nginx -p 8080:80 nginx
docker ps
curl http://localhost:8080
```

## Results

- Docker image downloaded successfully.
- Nginx container started successfully.
- Port Mapping: 8080 → 80
- Nginx welcome page served correctly.

## Analysis

Docker successfully deployed an Nginx web server container. Accessing localhost:8080 returned the default Nginx page, confirming that container networking and port forwarding were functioning properly.

---

# Key Learnings

- Provisioned and connected to an AWS EC2 Ubuntu server.
- Used SSH for remote server administration.
- Learned how ICMP ping works and how to interpret RTT and TTL values.
- Investigated DNS resolution using ping, nslookup, and dig.
- Performed DNS trace analysis.
- Used Nmap for service discovery and port scanning.
- Verified secure HTTPS communication using TLS 1.3.
- Deployed and tested a Dockerized Nginx web server.

---


# Conclusion

This lab provided hands-on experience with essential networking and cloud administration tools. I successfully provisioned an AWS EC2 instance, analyzed network connectivity, investigated DNS resolution, scanned local services, validated HTTPS security using TLS 1.3, and deployed a Dockerized Nginx web server. These activities helped build a strong foundation in Linux networking, troubleshooting, and cloud operations.
