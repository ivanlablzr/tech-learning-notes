---
type: note
tags: networking, osi, protocols, tls, ssl, layer 5 - session
---


**TLS (Transport Layer Security)** is the protocol that provides:

- **Confidentiality** (encryption)
    
- **Integrity** (tamper detection)
    
- **Authentication** (verifying who you're talking to)
    

When you see:

```text
https://example.com
```

the "S" means HTTP is running over TLS.

---

## SSL vs TLS

Historically:

|Protocol|Status|
|---|---|
|SSL 2.0|Obsolete|
|SSL 3.0|Obsolete|
|TLS 1.0|Obsolete|
|TLS 1.1|Obsolete|
|TLS 1.2|Still widely used|
|TLS 1.3|Current standard|

People still say "SSL certificate" or "SSL connection," but modern systems almost always use **TLS**, not SSL.

---

## Where TLS Fits

Without TLS:

```text
HTTP
  ↓
TCP
  ↓
IP
```

With HTTPS:

```text
HTTP
  ↓
TLS
  ↓
TCP
  ↓
IP
```

TLS sits between the application protocol and the transport layer, on the session layer.

It can protect many protocols, not just HTTP:

|Protocol|Secured Version|
|---|---|
|HTTP|HTTPS|
|SMTP|SMTPS / STARTTLS|
|IMAP|IMAPS|
|POP3|POP3S|
|LDAP|LDAPS|

---

## Why TLS Exists

Imagine logging into a website over plain HTTP.

```http
POST /login HTTP/1.1

username=alice
password=secret123
```

Anyone who can observe the traffic could potentially read it.

TLS encrypts the connection so observers see only ciphertext:

```text
8F A1 7C 92 D3 ...
```

rather than the actual credentials.

---

## The Three Main Goals

### 1. Confidentiality

Only the client and server can read the data.

```text
Client ── encrypted data ── Server
```

An observer can see packets but not their contents.

---

### 2. Integrity

TLS detects modification.

If someone changes:

```text
Transfer $100
```

to

```text
Transfer $10000
```

the integrity checks fail and the connection is terminated.

---

### 3. Authentication

TLS verifies that:

```text
www.bank.com
```

is actually operated by the entity controlling that domain.

This helps prevent impersonation.

---

# Certificates

Authentication relies on **digital certificates**.

A certificate contains information such as:

```text
Subject: www.example.com
Public Key: ...
Issuer: Certificate Authority
Expiration: ...
```

The server sends this certificate during the TLS handshake.

---

## Certificate Authorities (CAs)

Browsers trust a set of organizations called **Certificate Authorities**.

Examples include:

- DigiCert
    
- GlobalSign
    
- Let's Encrypt
    

A CA verifies control of a domain and signs a certificate.

Your browser trusts the CA, so it can trust certificates signed by that CA.

---

## Public-Key Cryptography

TLS uses two keys:

```text
Public Key
Private Key
```

The public key can be shared freely.

The private key must remain secret.

A simplified model:

```text
Encrypt with Public Key
          ↓
Decrypt with Private Key
```

Only the holder of the private key can decrypt.

---

# The TLS Handshake

The handshake occurs before any application data is exchanged.

A simplified TLS 1.3 flow:

```text
Client                      Server
   |                           |
   |------ ClientHello ------->|
   |                           |
   |<----- ServerHello --------|
   |<----- Certificate --------|
   |                           |
   |---- Key Exchange -------->|
   |                           |
   |==== Encrypted Traffic ====|
```

---

## Step 1: ClientHello

The client says:

```text
I support:
- TLS 1.3
- TLS 1.2

I support these cipher suites.
Here's a random value.
```

---

## Step 2: ServerHello

The server replies:

```text
We'll use TLS 1.3.
Here's my random value.
```

---

## Step 3: Certificate

The server sends its certificate.

The client verifies:

- Signature chain
    
- Expiration date
    
- Domain name
    
- Trust chain
    

---

## Step 4: Key Exchange

The client and server establish a shared secret.

Modern TLS usually uses:

Elliptic Curve Diffie–Hellman

This allows both sides to derive the same secret without transmitting it directly.

---

## Step 5: Session Keys

Both sides independently generate:

```text
Session Key
```

used for symmetric encryption.

From this point forward:

```text
HTTP Data
    ↓
Encrypted
    ↓
Sent
```

---

# Why Use Symmetric Encryption Afterward?

Public-key cryptography is relatively expensive.

Once a secure secret has been established:

```text
AES
ChaCha20
```

are used because they are much faster.

A common pattern is:

```text
Public-key crypto → establish secret
Symmetric crypto → encrypt data
```

---

# Certificate Validation

When you visit:

```text
https://example.com
```

your browser checks:

### Domain Match

Certificate:

```text
example.com
```

Requested:

```text
example.com
```

Must match.

---

### Expiration

Certificates have validity periods.

```text
Valid From
Valid Until
```

Expired certificates trigger warnings.

---

### Signature Chain

The browser validates a chain:

```text
Website Certificate
       ↓
Intermediate CA
       ↓
Root CA
```

The root CA is already trusted by the operating system or browser.

---

# What Happens if Validation Fails?

Browsers show warnings such as:

```text
Your connection is not private
```

Common causes:

- Expired certificate
    
- Wrong hostname
    
- Self-signed certificate
    
- Invalid signature chain
    

---

# TLS and [[HTTP]]

HTTP itself is unchanged.

Normal HTTP request:

```http
GET /products HTTP/1.1
Host: example.com
```

With HTTPS:

```text
HTTP request
      ↓
TLS encrypts it
      ↓
TCP transmits ciphertext
```

The server decrypts and processes the HTTP message.

---

# TLS 1.3 Improvements

TLS 1.3 simplified the protocol and removed many insecure features.

Benefits include:

- Faster handshakes
    
- Stronger default cryptography
    
- Reduced attack surface
    
- Better performance
    

It is now the preferred TLS version for modern systems.

---

# Forward Secrecy

A major feature of modern TLS is **forward secrecy**.

Even if an attacker later steals the server's private key:

```text
Past encrypted sessions
```

cannot normally be decrypted.

This is possible because each session uses ephemeral key exchange values rather than reusing long-term secrets.

---

# Relationship Between [[DNS]], TLS, and [[HTTP]]

When you open:

```text
https://www.example.com
```

the flow is typically:

```text
1. DNS
   www.example.com
   → 93.184.216.34

2. TCP or QUIC connection

3. TLS handshake
   - Verify identity
   - Establish encryption

4. HTTP request
   GET /

5. HTTP response
   200 OK
```

Or as a stack:

```text
Application
└─ HTTP

Security
└─ TLS

Transport
└─ TCP
   (or QUIC for HTTP/3)

Network
└─ IP
```

A useful way to remember the roles:

| Protocol | Main Question                                              |
| -------- | ---------------------------------------------------------- |
| [[DNS]]  | "Where is the server?"                                     |
| TLS      | "Can I trust the server, and can we communicate securely?" |
| [[HTTP]] | "What resource do I want?"                                 |
| TCP/QUIC | "How do the bytes get there reliably?"                     |
| IP       | "How are packets routed across networks?"                  |

Together, [[DNS]], TLS, and [[HTTP]] form the core sequence that occurs almost every time a browser loads a modern website.