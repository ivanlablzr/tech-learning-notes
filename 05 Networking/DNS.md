---
type: note
tags: networking, osi, protocols, dns, layer 7 - application
---


**DNS (Domain Name System)** is the internet's "phone book."

Humans prefer names:

```text
www.example.com
api.company.com
```

Computers communicate using IP addresses:

```text
93.184.216.34
2606:2800:220:1:248:1893:25c8:1946
```

DNS translates domain names into IP addresses so that applications know where to send network traffic.

---

## Where DNS Fits in the Network Stack

A simplified view:

```text
Application Layer
├─ HTTP
├─ HTTPS
├─ SMTP (email)
├─ FTP
├─ SSH
└─ DNS

Transport Layer
├─ TCP
└─ UDP

Internet Layer
└─ IP
```

DNS itself is an application-layer protocol.

Traditionally:

- DNS uses UDP port 53 for most queries.
    
- DNS uses TCP port 53 for large responses and some special operations.
    
- Modern encrypted DNS can use HTTPS or TLS.
    

---

## How DNS and [[HTTP]] Work Together

Suppose you enter:

```text
https://www.example.com/products
```

The browser cannot immediately send an HTTP request because it doesn't know the server's IP address.

The sequence is roughly:

```text
Browser
   │
   ├─ DNS lookup: "What is www.example.com?"
   │
   ├─ DNS response: "93.184.216.34"
   │
   ├─ Connect to 93.184.216.34
   │
   ├─ TLS handshake (HTTPS)
   │
   └─ HTTP request
```

Without DNS, the browser would need the IP address.

---

## Step-by-Step Example

### 1. User enters a URL

```text
https://www.example.com
```

### 2. Browser checks caches

It looks in:

- Browser DNS cache
    
- Operating system DNS cache
    
- Local DNS resolver cache
    

If found, no network lookup is needed.

---

### 3. DNS query occurs

The computer asks a DNS resolver:

```text
What is the IP address for www.example.com?
```

The resolver replies:

```text
93.184.216.34
```

---

### 4. Connection is established

For HTTPS:

```text
Client ── TCP/QUIC ──► Server
Client ◄─ TLS setup ─► Server
```

---

### 5. HTTP request is sent

```http
GET / HTTP/1.1
Host: www.example.com
```

The server responds with the webpage.

---

## DNS Is Used by Many Protocols

HTTP is only one consumer of DNS.

### Email (SMTP)

Sending mail to:

```text
alice@example.com
```

Requires DNS lookups to find mail servers.

DNS returns MX (Mail Exchange) records:

```text
example.com
  MX -> mail.example.com
```

Then the sending mail server connects to the mail server.

---

### SSH

When you run:

```bash
ssh server.example.com
```

DNS resolves:

```text
server.example.com → 203.0.113.10
```

Then SSH connects to that IP.

---

### FTP

FTP clients use DNS before establishing connections.

---

### Databases

Applications often connect to:

```text
db.company.com
```

instead of hardcoded IP addresses.

DNS provides the current database server address.

---

## Important DNS Record Types

### A Record

Maps a name to an IPv4 address.

```text
example.com
  A -> 93.184.216.34
```

---

### AAAA Record

Maps a name to an IPv6 address.

```text
example.com
  AAAA -> 2606:2800:220:1:248:1893:25c8:1946
```

---

### CNAME Record

Alias to another name.

```text
www.example.com
  CNAME -> example.com
```

---

### MX Record

Mail server information.

```text
example.com
  MX -> mail.example.com
```

---

### TXT Record

Arbitrary text data.

Commonly used for:

- Domain verification
    
- Email security (SPF, DKIM, DMARC)
    

---

### NS Record

Nameservers responsible for the domain.

```text
example.com
  NS -> ns1.provider.net
```

---

## Recursive and Authoritative DNS

There are several participants.

### Recursive Resolver

Usually provided by:

- Your ISP
    
- Public DNS services
    
- Enterprise networks
    

Examples include public resolvers operated by companies such as Google and Cloudflare.

Its job is to find the answer for you.

---

### Authoritative Nameserver

The source of truth for a domain.

For example:

```text
example.com
```

has authoritative DNS servers that know its records.

---

## A Simplified DNS Resolution Process

Suppose nobody has the answer cached.

```text
Client
  │
  ▼
Resolver
  │
  ├─ Ask root server
  │
  ├─ Ask .com server
  │
  ├─ Ask example.com nameserver
  │
  ▼
Answer returned
```

This happens very quickly, usually in milliseconds.

---

## DNS and HTTPS

DNS and HTTPS solve different problems.

### DNS answers:

```text
Where is the server?
```

### HTTPS answers:

```text
Am I talking securely to the right server?
```

A DNS response may say:

```text
www.example.com → 93.184.216.34
```

TLS certificates then help verify that the server at that address actually controls `www.example.com`.

---

## Modern DNS Enhancements

### DNS over HTTPS (DoH)

DNS messages are carried inside HTTPS requests.

```text
DNS
  ↓
HTTPS
  ↓
TCP or QUIC
```

Benefits:

- Encryption
    
- Reduced eavesdropping
    

---

### DNS over TLS (DoT)

DNS queries are encrypted using TLS directly.

```text
DNS
  ↓
TLS
  ↓
TCP
```

---

## Putting It All Together

When you visit a website:

```text
1. User enters URL
2. DNS resolves domain → IP
3. TCP or QUIC connection established
4. TLS secures the connection (HTTPS)
5. HTTP request sent
6. HTTP response returned
7. Browser renders content
```

So DNS is not part of HTTP itself. Instead, DNS is a separate protocol that typically runs **before** HTTP (and before many other protocols) to discover where a named service is located. HTTP, SMTP, SSH, database clients, and countless other applications all rely on DNS as the directory service that maps names to reachable network endpoints.