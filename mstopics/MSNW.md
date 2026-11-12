# MSNW — Computer Networks (Microsoft)

> Weightage: ~3–5%. Least asked of the fundamentals, but "type a URL and press enter" is near-guaranteed if CN comes up.
> SDE-1+ rounds turn it practical: latency, throughput, p99, request sizing, API gateways.

## 1. What happens when you type google.com and press Enter? (the big one)
1. **Parse the URL.** The browser decides it's a URL (not a search) and adds https.
2. **DNS lookup:** browser cache -> OS cache -> hosts file -> resolver (ISP or 8.8.8.8). The resolver asks root -> .com TLD -> Google's authoritative server, then returns the IP. DNS usually uses UDP on port 53.
3. **TCP 3-way handshake** with that IP on port 443: SYN -> SYN-ACK -> ACK.
4. **TLS handshake:** agree on a cipher, the server sends its certificate, the browser verifies it against trusted CAs, and both sides derive a shared session key.
5. **HTTP request:** `GET /` with headers (cookies, user-agent) over the encrypted channel.
6. **The server side:** load balancer -> web server -> app -> maybe cache/DB -> response (status code + HTML).
7. **Rendering:** parse HTML into the DOM, CSS into the CSSOM, run JS, layout, paint. Each extra resource (CSS, JS, images) is another request, often reusing the same connection.
**How to win here:** Go deep on whichever step they poke, and mention caching at every layer (DNS TTL, CDN, browser cache).

## 2. TCP vs UDP
| | TCP | UDP |
|---|---|---|
| Connection | Handshake first | None |
| Reliability | Retransmits lost data, in order | No guarantees |
| Overhead | Higher (20-byte+ header, ACKs) | Low (8-byte header) |
| Flow/congestion control | Yes | No |
| Use | Web, email, file transfer, DB | Video calls, gaming, DNS, live streaming |
**Analogy:** TCP is registered post with tracking and signature. UDP is shouting across a room.
**Smart point:** HTTP/3 runs on **QUIC over UDP**. It rebuilds reliability in user space so a single lost packet doesn't stall every stream (TCP's head-of-line blocking).

## 3. Why 3-way handshake, not 2?
Both sides need to confirm that *their own* starting sequence number got through. With only 2 steps, the server can't know the client received its sequence number. That also guards against old duplicate SYNs creating ghost connections.
**Closing** takes 4 steps (FIN, ACK, FIN, ACK) because each direction closes independently. TIME_WAIT keeps the socket around briefly so late packets don't confuse a new connection.

## 4. OSI vs TCP/IP model
OSI (7 layers): Physical, Data Link, Network, Transport, Session, Presentation, Application.
TCP/IP (4 layers): Link, Internet (IP), Transport (TCP/UDP), Application (HTTP, DNS...).
Mnemonic for OSI from bottom to top: **P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way.
| Layer | Unit | Device | Example |
|---|---|---|---|
| Data Link | Frame | Switch | MAC, Ethernet |
| Network | Packet | Router | IP, ICMP |
| Transport | Segment | — | TCP, UDP |

## 5. HTTP vs HTTPS
HTTPS = HTTP inside TLS. It gives encryption (no snooping), integrity (no tampering), and authentication (the certificate proves the server is really google.com).
**Concept:** **Asymmetric** crypto (public/private key) is used *only* in the handshake to agree on a key. After that, **symmetric** crypto (e.g. AES) encrypts the data, because it's much faster.

## 6. HTTP/1.1 vs HTTP/2 vs HTTP/3
- 1.1: one request at a time per connection (browsers open about 6 connections to work around this). Keep-alive reuses connections.
- 2: multiplexes many streams over one TCP connection, binary framing, header compression. Still suffers TCP head-of-line blocking.
- 3: QUIC over UDP, so no TCP head-of-line blocking, and faster setup (0-RTT on resumed connections).

## 7. "100MB as 25×4MB vs 12×8MB requests, which is better?" (SDE-1 L60 report)
First, notice that 12 × 8MB = 96MB, not 100MB. You need 13 requests. Pointing that out costs nothing and shows you're paying attention.
**Answer:** Usually **fewer, larger requests win on throughput**, because each request pays a fixed overhead: a round-trip, headers, maybe a handshake, and TCP slow-start ramping up. Network latency usually dominates over payload size.
**But smaller chunks win when:**
- The network is flaky. A failure means retrying 4MB instead of 8MB (resumable uploads chunk for exactly this reason).
- Server memory is tight, or there are request-size limits or timeouts.
- You can send chunks in parallel.
**The best answer:** "It depends on latency, loss rate, and parallelism. With keep-alive or HTTP/2 the per-request overhead shrinks, so the gap narrows. I'd measure."

## 8. Latency vs throughput vs p99
- **Latency:** how long one request takes.
- **Throughput:** how many requests (or bytes) per second the system handles.
- **p99 latency:** 99% of requests finish faster than this. It matters because averages hide the slow tail, and one page load that makes 50 backend calls almost always hits a tail-latency call.
**Analogy:** A highway. Latency = how long one car takes to cross. Throughput = cars per hour. p99 = the commute time of the unluckiest 1% of drivers.

## 9. DNS records
A (name -> IPv4), AAAA (-> IPv6), CNAME (alias -> another name), MX (mail server), NS (which servers are authoritative for the domain), TXT (verification/SPF). TTL = how long resolvers may cache the answer.

## 10. Load balancer: L4 vs L7
L4 routes by IP and port (fast, can't see content). L7 reads HTTP (routes by URL, header, or cookie; can terminate TLS). Algorithms: round robin, least connections, consistent hashing (for sticky sessions or caches).

## 11. API Gateway (SDE-1 report)
A single front door for many backend services. It handles auth, rate limiting, routing, request/response transformation, logging, and TLS termination, so every service doesn't reimplement them.
**Difference from a load balancer:** an LB spreads traffic across copies of *one* service. A gateway routes to *different* services and enforces policy.

## 12. Authentication vs authorization (SDE-1 report)
AuthN = *who are you?* (password, OAuth login, a JWT proving identity). AuthZ = *what are you allowed to do?* (roles, permissions). AuthN always happens first. A 401 means "not authenticated"; a 403 means "authenticated but not allowed."

## 13. IP basics
- IPv4 = 32 bits; IPv6 = 128 bits.
- `/24` = the first 24 bits are the network part, leaving 256 addresses (254 usable).
- Private ranges: 10.x, 172.16–31.x, 192.168.x.
- **NAT** lets many private devices share one public IP.
- **ARP** maps an IP address to a MAC address on the local network.
- **DHCP** hands out IP addresses automatically.

## 14. TCP flow control vs congestion control
Flow control protects the **receiver** (sliding window; the receiver advertises how much buffer it has left). Congestion control protects the **network** (slow start, then additive increase, then multiplicative decrease on loss).

## 15. Common status codes
200 OK, 201 Created, 301/302 redirect, 304 Not Modified (cache), 400 bad request, 401, 403, 404, 429 too many requests (rate limited), 500 server error, 502 bad gateway, 503 unavailable.
