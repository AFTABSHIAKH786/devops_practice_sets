# 🌐 Networking for DevOps & Platform Engineers

> **Goal:** Understand networking deeply enough to design, troubleshoot, and secure production systems — from TCP handshakes to Kubernetes CNI plugins.
>
> **Target:** 0–1 year experience → confident network troubleshooter
>
> **Philosophy:** Learn the theory, then immediately apply it in a terminal.

---

## 📚 Resources

### Primary
- **Julia Evans networking zines:** https://jvns.ca/networking-zine.pdf
- **Computer Networking: A Top-Down Approach** — Kurose & Ross (textbook, free PDFs around)
- **Beej's Guide to Network Programming:** https://beej.us/guide/bgnet/
- **IETF RFCs (official standards):** https://www.rfc-editor.org/

### Courses / Videos
- **Professor Messer's CompTIA Network+:** https://www.professormesser.com/network-plus/n10-008/n10-008-video/n10-008-training-course/
- **NetworkChuck networking videos:** https://youtube.com/c/NetworkChuck
- **TechWorld with Nana — Networking crash course:** https://youtu.be/xpXhudbQoRk
- **Practical Networking:** https://www.practicalnetworking.net/

### Tools / Practice
- **Cisco Packet Tracer (free simulator):** https://www.netacad.com/courses/packet-tracer
- **Wireshark:** https://www.wireshark.org/
- **GNS3 (network simulator):** https://www.gns3.com/
- **Subnetting practice:** https://subnettingpractice.com/

---

## Stage 0 — The OSI Model & TCP/IP Stack

### Learn
- The 7 OSI layers: Physical, Data Link, Network, Transport, Session, Presentation, Application
- The TCP/IP 4-layer model: Link, Internet, Transport, Application
- Why these models exist (troubleshooting mental model, not reality)
- Protocol Data Units (PDU): bits → frames → packets → segments → data
- Key protocols per layer

### Understand
- When a packet travels from your browser to a server — what happens at each layer?
- Why does knowing the OSI model make troubleshooting easier?

### Key Protocols by Layer
| Layer | Protocols |
|---|---|
| Application | HTTP, HTTPS, DNS, SMTP, FTP, SSH, DHCP |
| Transport | TCP, UDP |
| Network | IP (IPv4/IPv6), ICMP, ARP |
| Data Link | Ethernet, Wi-Fi (802.11), MAC |
| Physical | Cables, fiber, radio waves |

### Resources
- https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/
- https://www.computernetworkingnotes.com/networking-tutorials/osi-model-explained.html

### 🟢 Easy Exercises
1. Draw (or write out) the full OSI model from memory with one example protocol at each layer
2. Use `curl -v https://example.com` and identify which OSI layers are involved at each step of the output
3. Use `ping 8.8.8.8` and explain which OSI layers are involved in a ping
4. Use `traceroute 8.8.8.8` and identify what layer 3 device each hop represents

### 🟡 Medium Exercises
1. Capture a DNS query with Wireshark or `tcpdump` and identify the packet at each OSI layer
2. Compare a TCP connection vs a UDP "connection" — explain in writing the difference using OSI layers
3. What happens when you type `https://github.com` in a browser? Write a detailed step-by-step explanation using the OSI layers

### 🏗 DevOps Project
Write a cheat sheet document (for yourself) that maps the most common DevOps issues to OSI layers: "certificate error → Layer 6/7", "port unreachable → Layer 4", "routing issue → Layer 3", etc.

---

## Stage 1 — IP Addressing & Subnetting

### Learn
- IPv4 address structure: 4 octets, binary representation
- Classes A/B/C/D/E (historical context)
- Subnet masks and CIDR notation (/8, /16, /24, /32)
- Network address, broadcast address, usable host range
- Public vs private IP ranges (RFC 1918)
- IPv6 basics: format, global unicast, link-local, loopback

### Resources
- **Subnetting cheat sheet:** https://www.freecodecamp.org/news/subnet-cheat-sheet-24-subnet-mask-30-26-27-29-and-other-ip-address-cidr-network-references/
- **Subnetting practice site:** https://subnettingpractice.com/
- **IPv6 tutorial:** https://www.tutorialspoint.com/ipv6/

### 🟢 Easy Exercises
1. Given `192.168.10.50/24`, calculate: network address, broadcast, first host, last host, total usable hosts
2. Given `10.0.0.0/8`, how many host addresses are possible?
3. Is `172.20.5.1` a private or public IP? Which RFC 1918 range does it fall in?
4. List the three private IP ranges and their CIDR blocks from memory

### 🟡 Medium Exercises
1. You have `10.10.0.0/16`. Divide it into 4 equal subnets. List all 4 network addresses, their CIDR prefix, and host count.
2. A server has IP `192.168.1.100/26`. Can it communicate directly (no router) with `192.168.1.200`? Show your working.
3. Design the IP addressing scheme for a 3-tier app: web tier (20 hosts), app tier (10 hosts), DB tier (5 hosts) — choose the most efficient subnets

### 🔴 Hard Exercises
1. Write a Bash or Python script that takes a CIDR block as input and prints: network address, broadcast, all usable IPs, subnet mask, wildcard mask, and number of hosts
2. Design the network for a Kubernetes cluster: nodes subnet, pod CIDR, service CIDR — explain why they must not overlap

### 🏗 DevOps Project
Document the IP addressing scheme for a production-like environment: VPC (10.0.0.0/16) divided into public subnets, private app subnets, and private DB subnets across 3 availability zones.

---

## Stage 2 — DNS — The Internet's Phone Book

### Learn
- How DNS resolution works (recursive vs iterative)
- DNS record types: A, AAAA, CNAME, MX, TXT, NS, SOA, PTR, SRV
- DNS hierarchy: root servers → TLD → authoritative
- TTL and caching
- `/etc/resolv.conf`, `/etc/hosts`
- Tools: `dig`, `nslookup`, `host`

### Resources
- **"How DNS works" comic:** https://howdns.works/
- **DNS in One Lesson:** https://youtu.be/HsQOWfc3Wic (YouTube)
- https://www.cloudflare.com/learning/dns/what-is-dns/
- **dig manual:** https://linux.die.net/man/1/dig

### 🟢 Easy Exercises
1. Use `dig A github.com` and identify: the answer section, TTL, and authoritative nameserver
2. Look up the MX records for `gmail.com` — what mail servers does Google use?
3. Do a reverse DNS lookup for `8.8.8.8` — whose IP is this?
4. Use `dig +trace github.com` and describe each step of the resolution process
5. Add a custom entry to `/etc/hosts` to override DNS for a test domain and verify it works

### 🟡 Medium Exercises
1. Explain the full DNS resolution journey for `api.example.com` when it's not cached — include every server involved
2. What is a CNAME and why can't you use it for the zone apex (`example.com`)? What do you use instead?
3. Write a script that checks DNS propagation: queries a hostname from 5 different public DNS resolvers (8.8.8.8, 1.1.1.1, 9.9.9.9, etc.) and reports if they return the same answer

### 🔴 Hard Exercises
1. Set up a local DNS resolver using `bind9` or `dnsmasq` with custom records for a test domain and verify it resolves correctly
2. Explain and demonstrate DNS-based service discovery — set up SRV records for a fake microservice and query them

### 🏗 DevOps Project
Build a `dns_health_check.sh` script: reads a list of DNS records to check (from a YAML-like config file), queries each one, compares with expected values, and reports pass/fail — useful for post-deployment verification.

---

## Stage 3 — TCP, UDP & Ports

### Learn
- TCP: 3-way handshake (SYN, SYN-ACK, ACK), 4-way teardown
- TCP features: reliability, ordering, flow control, congestion control
- UDP: connectionless, unreliable, why it's faster
- Port numbers: well-known (0–1023), registered (1024–49151), dynamic
- Common ports: 22 (SSH), 80 (HTTP), 443 (HTTPS), 53 (DNS), 3306 (MySQL), 5432 (PostgreSQL), 6379 (Redis), 27017 (MongoDB)
- `netstat -tulpn`, `ss -tulpn`, `lsof -i`
- TIME_WAIT, CLOSE_WAIT states

### Resources
- https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/
- **Wireshark tutorial — TCP analysis:** https://youtu.be/r0l_54thSYU

### 🟢 Easy Exercises
1. List all listening TCP ports and the processes using them
2. Use `curl -v http://example.com` and identify the TCP handshake in the output
3. Capture a TCP 3-way handshake with `tcpdump` on port 80 to any host
4. Show all established TCP connections and count them

### 🟡 Medium Exercises
1. Explain why TIME_WAIT exists and under what production circumstances it becomes a problem
2. Write a Bash script using `/dev/tcp` (no curl/netcat) to check if a TCP port is open on a host
3. Use `ss -s` to get socket statistics and explain each counter

### 🔴 Hard Exercises
1. Tune kernel TCP parameters (`/proc/sys/net/ipv4/`) to handle high connection counts — document each parameter changed and the reason
2. Analyze a packet capture of a slow HTTP response using Wireshark — identify whether the delay is in the TCP layer or application layer

### 🏗 DevOps Project
Build a `port_scanner.sh` script (using only bash `/dev/tcp`, no nmap) that scans a host for open ports in a range, respects a timeout, and outputs a neat report.

---

## Stage 4 — HTTP & HTTPS

### Learn
- HTTP/1.1: request structure, response structure, methods (GET/POST/PUT/DELETE/PATCH)
- Status codes: 1xx, 2xx, 3xx, 4xx, 5xx — know the most common ones
- Headers: Host, Content-Type, Authorization, Cache-Control, Cookie, Set-Cookie, Location
- HTTP/2: multiplexing, header compression, why it's faster
- TLS/SSL: handshake process, certificates, CA chain, SNI
- HTTPS: TLS over TCP, certificate verification
- REST API concepts

### Resources
- **MDN HTTP reference:** https://developer.mozilla.org/en-US/docs/Web/HTTP
- **HTTP/2 explained:** https://http2-explained.haxx.se/
- **TLS 1.3 explained:** https://tls13.xargs.org/
- **curl manual:** https://curl.se/docs/manual.html

### 🟢 Easy Exercises
1. Use `curl -I https://github.com` and interpret every response header
2. Make a POST request with a JSON body using `curl -X POST -H "Content-Type: application/json" -d '{"key":"val"}'`
3. Follow a redirect manually: `curl -I http://google.com` — then `curl -I` the location header
4. Show the full TLS certificate chain for `github.com` using `openssl s_client`

### 🟡 Medium Exercises
1. Explain the difference between 301 and 302 redirects — when would you use each?
2. Decode a Base64-encoded Authorization header: `Authorization: Basic dXNlcjpwYXNz` — what username and password does it contain?
3. Use `curl` to test an API endpoint that requires Bearer token authentication

### 🔴 Hard Exercises
1. Implement a basic HTTP server using only Bash and `nc` (netcat) that responds with "Hello World" on port 8080
2. Capture a full HTTPS handshake with `tcpdump` and analyze the TLS records — identify: ClientHello, ServerHello, Certificate, and Finished messages

### 🏗 DevOps Project
Build an `api_health_checker.sh` script: reads a list of API endpoints + expected status codes from a config file, checks each with `curl`, measures response time, and outputs a pass/fail table.

---

## Stage 5 — Load Balancers, Proxies & CDNs

### Learn
- Layer 4 vs Layer 7 load balancing
- Load balancing algorithms: round-robin, least-connections, IP hash
- Reverse proxy vs forward proxy
- nginx as reverse proxy and load balancer
- HAProxy basics
- CDN concepts: edge nodes, cache hit/miss, origin
- SSL termination at the load balancer

### Resources
- **nginx docs:** https://nginx.org/en/docs/
- **HAProxy docs:** https://www.haproxy.org/download/2.8/doc/
- **Load Balancing 101:** https://www.nginx.com/resources/glossary/load-balancing/

### 🟢 Easy Exercises
1. Configure nginx as a reverse proxy for a backend server running on port 8080
2. Configure nginx to serve static files from `/var/www/html` with caching headers
3. Add HTTPS to nginx using a self-signed certificate
4. Set up basic HTTP authentication on an nginx location block

### 🟡 Medium Exercises
1. Configure nginx as a Layer 7 load balancer for 3 backend servers using round-robin, then test with a loop of curl requests and count hits per backend
2. Set up nginx to redirect all HTTP traffic to HTTPS permanently
3. Configure nginx rate limiting: max 10 requests/second per IP, returning 429 on excess

### 🔴 Hard Exercises
1. Set up HAProxy to load balance TCP traffic for a MySQL cluster — configure health checks and session persistence
2. Configure nginx with upstream keepalive connections and explain why it improves performance

### 🏗 DevOps Project
Build a production-ready nginx config for a 3-tier app: static assets from `/static`, API requests proxied to `api-backend:8080`, health check endpoint at `/health`, rate limiting, HTTPS only, and proper logging format.

---

## 🔗 Additional Networking Resources

### Deep Dives
- **"Computer Networks: A Top Down Approach" lectures:** https://gaia.cs.umass.edu/kurose_ross/lectures.php
- **Brendan Gregg's networking tools:** https://www.brendangregg.com/Perf/linux_observability_tools.png
- **Kubernetes networking explained:** https://iximiuz.com/en/posts/kubernetes-networking-visual-guide/

### Must-Read Articles
- "What really happens when you navigate to a URL" https://github.com/alex/what-happens-when
- "The Illustrated TLS 1.3 Connection" https://tls13.xargs.org/
- Julia Evans' networking posts: https://jvns.ca/#networking

### Cheat Sheets
- **TCP/IP ports reference:** https://www.iana.org/assignments/service-names-port-numbers/
- **curl cheat sheet:** https://curl.se/docs/manual.html
- **tcpdump cheat sheet:** https://www.tcpdump.org/manpages/tcpdump.1.html

---

*Progress: 0/6 stages complete · Est. time to complete: 4–6 weeks at 3 hrs/day*
