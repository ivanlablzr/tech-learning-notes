---
type: note
tags: networking, osi, protocols, http, layer 7 - application
---


**HTTP (Hypertext Transfer Protocol)** is the application-layer protocol that powers most communication on the web. It defines how a **client** (such as a web browser, mobile app, or API consumer) requests (HTTP methods) information from a **server**, and how the server responds (Status codes).

A typical interaction looks like:

```text
Client                    Server
   |---- HTTP Request ---->|
   |<--- HTTP Response ----|
```

For example, when you visit a website, your browser sends an HTTP request to the site's server, and the server returns an HTTP response containing HTML, images, data, or other resources.

---

## Core Concepts

### 1. Resources

HTTP is designed around **resources** identified by URLs.

Examples:

```text
https://example.com/
https://example.com/products/123
https://api.example.com/users/42
```

Each URL points to a resource that can be retrieved or manipulated.

---

### 2. Requests

An HTTP request typically contains:

- A method (what action to perform)
    
- A URL
    
- Headers (metadata)
    
- Optionally, a body (data being sent)
    

Example:

```http
GET /products/123 HTTP/1.1
Host: example.com
Accept: application/json
```

---

### 3. Responses

The server returns:

- A status code
    
- Headers
    
- Optionally, a response body
    

Example:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "name": "Laptop"
}
```

---

## HTTP Methods

Methods describe the intended action.

|Method|Purpose|
|---|---|
|GET|Retrieve data|
|POST|Create a resource or submit data|
|PUT|Replace an existing resource|
|PATCH|Partially update a resource|
|DELETE|Remove a resource|
|HEAD|Retrieve headers only|
|OPTIONS|Discover supported operations|

Examples:

```http
GET /users/42
```

Retrieve a user.

```http
POST /users
```

Create a new user.

```http
DELETE /users/42
```

Delete a user.

---

## Status Codes

Status codes indicate the outcome of a request.

### 2xx — Success

|Code|Meaning|
|---|---|
|200|OK|
|201|Created|
|204|No Content|

### 3xx — Redirection

|Code|Meaning|
|---|---|
|301|Moved Permanently|
|302|Found (temporary redirect)|
|304|Not Modified|

### 4xx — Client Errors

|Code|Meaning|
|---|---|
|400|Bad Request|
|401|Unauthorized|
|403|Forbidden|
|404|Not Found|
|429|Too Many Requests|

### 5xx — Server Errors

|Code|Meaning|
|---|---|
|500|Internal Server Error|
|502|Bad Gateway|
|503|Service Unavailable|

---

## Headers

Headers provide additional information about the request or response.

Request headers:

```http
Authorization: Bearer abc123
Accept: application/json
User-Agent: Chrome/137
```

Response headers:

```http
Content-Type: application/json
Content-Length: 150
Cache-Control: max-age=3600
```

Common uses include:

- Authentication
    
- Content negotiation
    
- Caching
    
- Compression
    
- Cookies
    

---

## Request Body

Methods such as POST, PUT, and PATCH often include a body.

Example:

```http
POST /users HTTP/1.1
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

The server processes the submitted data and returns a response.

---

## Statelessness

One of HTTP's defining characteristics is that it is **stateless**.

This means:

- Each request is independent.
    
- The server does not automatically remember previous requests.
    

Example:

```text
Request 1: Login
Request 2: View profile
Request 3: Buy item
```

The server must be given a way to identify the user on each request.

Common solutions:

- Cookies
    
- Session IDs
    
- Authentication tokens (JWTs, OAuth tokens)
    

---

## Cookies

Cookies allow servers to store small pieces of information on the client.

Example response:

```http
Set-Cookie: session_id=xyz123
```

Future requests include:

```http
Cookie: session_id=xyz123
```

This helps maintain user sessions.

---

## HTTP vs HTTPS

### HTTP

```text
http://example.com
```

Traffic is sent in plaintext.

### HTTPS

```text
https://example.com
```

HTTP is carried over **TLS (Transport Layer Security)**.

Benefits:

- Encryption
    
- Integrity protection
    
- Server authentication
    

Today, nearly all major websites use HTTPS.

---

## HTTP Versions

### HTTP/1.1

The classic version.

Features:

- Persistent connections
    
- Request pipelining (rarely used)
    

### HTTP/2

Major improvements:

- Multiplexing multiple requests on one connection
    
- Header compression
    
- Better performance
    

### HTTP/3

Built on the transport protocol QUIC instead of TCP.

Benefits:

- Faster connection establishment
    
- Better handling of packet loss
    
- Improved performance on unreliable networks
    

---

## Example: Loading a Web Page

When you enter:

```text
https://example.com
```

A simplified sequence is:

1. Browser resolves the domain name to an IP address (DNS).
    
2. Browser establishes a TCP/TLS or QUIC connection.
    
3. Browser sends:
    

```http
GET / HTTP/1.1
Host: example.com
```

4. Server returns:
    

```http
HTTP/1.1 200 OK
Content-Type: text/html

<html>...</html>
```

5. Browser parses the HTML.
    
6. Browser requests referenced CSS, JavaScript, images, and fonts.
    
7. Page is rendered.
    

---

## HTTP and APIs

Modern web APIs typically use HTTP with JSON.

Example:

### Request

```http
GET /api/users/42
Accept: application/json
```

### Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 42,
  "name": "Alice"
}
```

This is the foundation of most REST APIs.

---



A useful mental model is: **HTTP is the language that clients and servers use to ask for and deliver resources across the web.** 