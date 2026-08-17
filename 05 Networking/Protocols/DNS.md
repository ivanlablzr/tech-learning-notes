---
type: note
tags: networking, osi, protocols, dns, layer 7 - application
---

> [[05 Networking]] → canonical deep note for this protocol. The cross-protocol **'load a webpage' end-to-end flow** is documented once in [[Master Index — Technology Vault]]; this note stays focused on the protocol itself.
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
