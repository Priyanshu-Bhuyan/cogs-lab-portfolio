# TLS Certificate Analysis Lab

## Objective

The objective of this lab was to inspect TLS/SSL certificates, understand certificate fields, verify certificate validity, and observe how HTTPS uses certificates to establish secure communication.

---
# Environment

- Platform: AWS EC2
- Operating System: Ubuntu 22.04
- Tools Used:
  - OpenSSL
  - curl
  - Nginx
  - Linux Terminal

---

# Task 1: Analyze Google's TLS Certificate

## Command

```bash
openssl s_client -connect google.com:443 2>/dev/null | \
openssl x509 -noout -text | \
grep -E "Subject:|Issuer:|Not After"
```

## Results

### Issuer

```
C=US, O=Google Trust Services, CN=WR2
```

### Subject

```
CN=*.google.com
```

### Expiry Date

```
Aug 17 08:36:18 2026 GMT
```

## Explanation

### Subject

The Subject identifies the website the certificate belongs to.

```
CN=*.google.com
```

The wildcard (*) means the certificate is valid for:

- google.com
- www.google.com
- mail.google.com
- maps.google.com

and other Google subdomains.

---

### Issuer

The Issuer is the Certificate Authority (CA) that signed and validated the certificate.

```
Google Trust Services
```

A trusted CA confirms that the website is genuinely owned by the organization claiming ownership.

---

### Expiry Date

```
Aug 17 08:36:18 2026 GMT
```

Certificates are only valid for a limited period.

Benefits:

- Limits damage if a certificate is compromised.
- Forces regular security updates.
- Ensures cryptographic standards remain current.

---

# Task 2: Test HTTPS Connection

## Command

```bash
curl -v https://www.google.com
```

## Results

Observed:

- TLS Version: TLS 1.3
- ALPN Protocols Offered:
  - HTTP/2
  - HTTP/1.1

Example Output:

```
ALPN: curl offers h2,http/1.1
TLSv1.3 (OUT), TLS handshake, Client hello
TLSv1.3 (IN), TLS handshake, Server hello
```

## Explanation

### TLS 1.3

TLS 1.3 is the latest version of the TLS protocol.

Advantages:

- Faster handshakes
- Stronger encryption
- Better protection against attacks
- Improved privacy

---

### TLS Handshake

During the handshake:

1. Client sends ClientHello.
2. Server sends ServerHello.
3. Certificate is presented.
4. Certificate is verified.
5. Session keys are generated.
6. Secure encrypted communication begins.

---

# Task 3: Analyze Local Self-Signed Certificate

## Command

```bash
openssl s_client -connect localhost:443 2>/dev/null | \
openssl x509 -noout -text | \
grep -E "Subject:|Issuer:|Not After"
```

## Results

### Issuer

```
C=IN, ST=Odisha, L=Dhenkanal, O=COGS Lab, CN=3.26.178.153
```

### Subject

```
C=IN, ST=Odisha, L=Dhenkanal, O=COGS Lab, CN=3.26.178.153
```

### Expiry Date

```
Jun 19 15:35:10 2027 GMT
```

---

## Explanation

### Self-Signed Certificate

The Issuer and Subject are identical.

This indicates a self-signed certificate.

```
Issuer = Subject
```

Unlike Google's certificate, no public Certificate Authority validated this certificate.

---

### Why Self-Signed Certificates Are Used

Common uses:

- Development environments
- Internal testing
- Lab exercises
- Private networks

Advantages:

- Free
- Easy to create
- Useful for learning TLS

Disadvantages:

- Browsers show security warnings
- Not trusted publicly
- Vulnerable to impersonation if distributed improperly

---

# Comparison

| Property | Google Certificate | Local Certificate |
|-----------|------------------|------------------|
| Certificate Type | Public CA Signed | Self-Signed |
| Issuer | Google Trust Services | COGS Lab |
| Subject | *.google.com | 3.26.178.153 |
| Trusted by Browsers | Yes | No |
| Intended Use | Public Websites | Internal Testing |
| Security Warning | None | Browser Warning |

---

# Key Learnings

- Learned how TLS certificates establish trust.
- Understood the difference between Subject and Issuer fields.
- Verified certificate expiration dates.
- Observed TLS 1.3 handshake messages.
- Compared CA-signed certificates with self-signed certificates.
- Learned why browsers trust public certificates but warn about self-signed certificates.

---

# Conclusion

This lab demonstrated how HTTPS security relies on TLS certificates. Google's certificate was signed by a trusted Certificate Authority, making it trusted by browsers worldwide. The local certificate was self-signed and suitable only for testing environments. Through OpenSSL and curl, I gained practical experience inspecting certificates, validating trust chains, and understanding how secure HTTPS connections are established.
