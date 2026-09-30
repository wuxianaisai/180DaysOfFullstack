#  Web Fundamentals & HTML Basics

## Part I: Computer Networking

### 1. What Is the Internet?

The Internet is **not a cloud**—it is physical infrastructure composed of:

- Copper and fiber-optic cables
- Mobile networks and satellites
- **99% of intercontinental traffic travels through undersea cables**

A **network** is two or more devices exchanging data. For communication to work, **protocols** are required—rules governing when to start, how to acknowledge delivery, and what to do during collisions.

#### Key Roles

| Role | Description |
|------|-------------|
| **Server** | Computer with a unique IP address that stores web pages/data |
| **Client** | Home device that connects via an Internet Service Provider (ISP) |
| **ISP** | Intermediary between clients and the broader Internet |

---

### 2. Historical Timeline

| Era | Event |
|-----|-------|
| Cold War | U.S. seeks a communication network resilient to nuclear attacks |
| Packet Switching | Paul Baran and Donald Davies independently propose splitting data into packets |
| **October 29, 1969** | First ARPANET message sent from UCLA to Stanford. The intended word "LOGIN" crashed after two letters, so the first message was **"LO"** |
| **1970** | Norman Abramson develops **ALOHAnet**, the first wireless random-access packet network |
| **1971** | Ray Tomlinson sends the first **network email** between two TENEX systems on ARPANET |
| **1986** | Dennis Jennings adopts TCP/IP for NSFNET, a critical decision for the protocol's dominance |
| **1989–1990** | Tim Berners-Lee creates the **World Wide Web** (HTTP, HTML, URL, first browser) |
| **1990** | ARPANET formally decommissioned (July 1990) |
| **1995** | NSFNET backbone decommissioned; Internet fully transitions to private enterprise |

#### Notable Contributions

- **Norman Abramson**: ALOHAnet's random-access protocol directly inspired Robert Metcalfe's work on **Ethernet and CSMA/CD**
- **John Postel**: Central figure in Internet addressing
- **BGP**: Reportedly designed "on a napkin over lunch"

#### Internet Folklore

- **RFC 1149**: "A Standard for the Transmission of IP Datagrams on Avian Carriers"—an April Fools' RFC (1990) describing IP over carrier pigeons. Successfully implemented in 2001 by the Bergen Linux User Group, achieving 55% packet loss
- **RFC 6214**: Extension of RFC 1149 for IPv6 (2011)

---

### 3. OSI vs. TCP/IP Models

#### OSI Model (7 Layers) — Theoretical/Reference

| # | Layer | Function |
|---|-------|----------|
| 1 | Physical | Bits, cables, signals |
| 2 | Data Link | MAC addresses, frames, switches |
| 3 | Network | IP addresses, routers |
| 4 | Transport | Ports, TCP/UDP |
| 5 | Session | Connection maintenance |
| 6 | Presentation | Encryption, compression |
| 7 | Application | HTTP, FTP, SMTP, RDP |

#### TCP/IP Model (4 Layers) — Practical/Real-World

1. **Application** (combines OSI 5–7)
2. **Transport**
3. **Network**
4. **Data Link + Physical**

#### Encapsulation

Each layer adds its own header. Data flows **top-down** on send and **bottom-up** on receive (decapsulation). Lower layers never see upper-layer metadata.

> **Why TCP/IP Won Over OSI**: TCP/IP was simpler, free, and already working. OSI was expensive, complex, and late to market. By 1988–1989, the TCP/IP product market was visibly larger than OSI's. The NSF's 1985 mandate that funded Internet connections required TCP/IP, cementing its dominance.

---

### 4. Addressing

#### MAC Addresses (Layer 2)

- 48-bit unique identifier of a network interface card
- First half identifies the manufacturer
- Works **only within a local network**
- **Changes at every hop**

#### IP Addresses (Layer 3)

- Logical address, globally routable
- **Does not change** along the path
- **IPv4**: 4 octets (0–255), ~4.2 billion addresses
- **IPv6**: Solves address exhaustion; removes fragmentation and checksum fields; adds flow label and traffic class
- **Private (gray)** vs. **Public (white)** IPs

#### Subnets and Masks

- IP address splits into **network address** and **host address**
- Mask / CIDR defines subnet boundaries and host count
- Broadcast and reserved addresses exist

#### Ports (Layer 4)

Identify which process data is destined for:

| Port | Protocol |
|------|----------|
| 80 | HTTP |
| 443 | HTTPS |
| 22 | SSH |
| 53 | DNS |

#### NAT (Network Address Translation)

- Hides many devices behind one external IP
- Maintains a translation table
- Used extensively in home networks and cloud environments

#### DHCP (Dynamic Host Configuration Protocol)

Automatically assigns IP, mask, gateway, and DNS servers.

**Process**: Discover → Offer → Request → ACK

Can be static, automatic, or dynamic.

#### ARP (Address Resolution Protocol)

Maps IP addresses to MAC addresses within a local network.

---

### 5. Core Protocols

#### TCP — Reliable Delivery

- **Three-way handshake**: SYN → SYN/ACK → ACK
- Byte numbering, acknowledgments (ACK), retransmission timers
- Window management (prevents buffer overflow)
- Congestion control
- Flags: SYN, ACK, FIN, RST, PSH, URG
- **Used by**: HTTP, SSH, databases, email

#### UDP — Speed Without Guarantees

- No connection, no acknowledgments, no congestion control
- **Used by**: Streaming, VoIP, gaming, DNS

> **DNS primarily uses UDP on port 53** for standard queries—speed is prioritized over reliability.

#### ICMP (Internet Control Message Protocol)

Service messages: ping, traceroute, error reporting.

#### HTTP / HTTPS

| Aspect | Details |
|--------|---------|
| **HTTP** | Text-based request-response protocol |
| **Methods** | GET, POST, HEAD, PUT, DELETE, OPTIONS |
| **Status Codes** | 1xx–5xx (401 = unauthorized, 403 = forbidden, 407 = proxy auth) |
| **Caching Headers** | Expires, Last-Modified, ETag, Cache-Control |
| **Keep-Alive** | Multiple requests over one TCP connection (HTTP/1.1) |
| **HTTPS** | HTTP + TLS: encryption, certificates, MITM protection |
| **SNI** | Remains unencrypted; used for filtering/blocking |
| **Adoption** | Mass HTTPS migration occurred only in the 2010s |

#### DNS (Domain Name System)

- Translates domain names to IP addresses
- **Hierarchy**: Local cache → Resolver → Root servers → Zone server → Authoritative server
- **Record types**: A, CNAME, MX, TXT, PTR
- **Transport**: UDP on port 53
- Replaced the original single `hosts.txt` file
- Critical for load balancing, fault tolerance, and service discovery

---

### 6. Routing Protocols

| Protocol | Characteristics |
|----------|-----------------|
| **RIP** | Hop count metric, slow, outdated |
| **OSPF** | Builds network map, Dijkstra's algorithm, fast convergence |
| **BGP** | Connects the Internet; exchanges routes between Autonomous Systems (ASes); the "routing map of the Internet"; a **political protocol** based on trust |

#### BGP Vulnerabilities and Incidents

**The protocol operates on trust—errors and attacks can redirect traffic globally.**

| Incident | Year | Impact |
|----------|------|--------|
| **YouTube Hijack** | 2008 | Pakistan Telecom announced a more-specific prefix for YouTube, redirecting global traffic to Pakistan. YouTube countered with even more-specific announcements |
| **Amazon Route 53 Hijack** | 2018 | Attackers hijacked 1,300+ AWS Route 53 IPs via BGP, redirecting MyEtherWallet.com users to a phishing site. **~$17 million in ETH stolen** |
| **Cloudflare 1.1.1.1** | 2024 | A Brazilian ISP announced an overly specific route, overriding Cloudflare's Anycast announcement. Global traffic was rerouted to Brazil and failed |

#### RPKI (Resource Public Key Infrastructure)

A security framework for verifying the association between resource holders (IP addresses, AS numbers) and their Internet resources.

- **ROA** (Route Origin Authorization): A signed object authorizing an AS to originate a route for a prefix
- **RFC 3779**: Defines X.509 extensions binding IP addresses and AS identifiers to certificates
- **Limitations**: RPKI alone cannot prevent all hijacks—an attacker can still replicate the origin or use more specific prefixes
- **BGPSEC**: Proposed extension to secure the AS path, but deployment has stalled due to partial adoption challenges and high crypto overhead

---

### 7. Network Devices

| Device | Layer | Function |
|--------|-------|----------|
| **Hub** | L1 | Repeats signal to all ports |
| **Switch** | L2 | Connects devices by MAC; maintains MAC table |
| **Router** | L3 | Connects networks by IP |
| **Firewall** | — | Filters traffic |
| **VLAN** | L2 | Logical network segmentation |

---

### 8. Internet Acceleration: GeoDNS, Anycast, CDN

**Problem**: A server sees the resolver, not the client, potentially choosing the wrong location.

#### GeoDNS

- Authoritative server examines the request's origin
- Returns different IPs for different regions
- DNS acts as a load balancer

#### ECS (EDNS Client Subnet)

- Resolver forwards part of the client's IP
- Server selects a point closer to the user
- **Trade-off**: Speed vs. privacy

#### Anycast

- One IP physically exists in dozens of data centers
- BGP selects the shortest path
- **Automatic failover**: If a data center fails, it stops announcing its route
- Used by root DNS, public resolvers, and anti-DDoS services

#### CDN (Content Delivery Network)

- Cache nodes worldwide, often directly at ISPs
- Heavy content (YouTube) comes from the nearest node
- **GeoDNS + Anycast + CDN = minimal latency**

---

### 9. The Journey of a Request

1. User enters a URL in the browser
2. Cache checks → DNS query → IP obtained
3. TCP connection established (SYN → SYN/ACK → ACK)
4. TLS handshake (certificate, trust chain verification)
5. HTTP request sent
6. **Path**: Computer → Home router → ISP → Tier 1 backbone → Internet Exchange Point → Data center
7. Load balancer → Server
8. Response may take a different route
9. Browser processes HTML, caches resources

**The entire journey takes milliseconds, but each step involves different devices and protocols.**

---

### 10. Browsers and the Web

#### History

| Browser | Significance |
|---------|-------------|
| Mosaic | First mass-market graphical browser |
| Netscape Navigator | Captured early market share |
| Internet Explorer | Monopoly via Windows bundling |
| Firefox, Chrome, Safari | New standards for speed and security |
| Edge | Microsoft's replacement for IE |

#### URL Structure

`protocol://domain:port/path?parameters#anchor`

Non-standard characters are percent-encoded using UTF-8.

#### Browser Actions

1. Resolve IP via DNS
2. Establish TCP connection
3. Send HTTP request
4. Receive and process HTML

#### Caching

- Browser stores resources locally
- Partial requests allow resuming file downloads

#### Authorization

- **401**: Authentication required
- Methods: Basic, Bearer, Proxy-Auth
- **403**: Access forbidden; **407**: Proxy authentication required

---

### 11. Wi-Fi

#### Fundamentals

- Standard: **IEEE 802.11**
- "Wi-Fi" is a marketing name, not an abbreviation
- Access point (router) receives Internet via cable and broadcasts via radio waves

#### Frequencies and Channels

- Bands: **2.4 GHz and 5 GHz** (unlicensed)
- Newer standards add **6 GHz**
- Bands are divided into channels; scarcity causes collisions
- Household devices share these bands → interference

#### IP and NAT in Wi-Fi

- Only the access point receives a public IP
- Devices receive private IPs from the router via DHCP
- NAT replaces internal IPs with the external one

#### Wi-Fi Frames

- Types: management, control, data
- Up to **four MAC addresses** (sender, access point, router, etc.)
- Frame control field: type, direction, fragmentation, retry, power saving, Protected flag

#### Connection Process

1. Access point broadcasts frames with SSID, MAC, encryption type
2. Device scans (passively or actively)
3. Authentication → Association

#### Encryption Evolution

**WEP → WPA → WPA2 → WPA3**

Modern standards use AES. Device and access point generate a shared key.

#### Collisions

- Radio signals overlap → frames lost
- Acknowledgments (ACK) and retransmission timers
- Time intervals reduce collision probability

#### Fragmentation

Large frames split into fragments <1000 bytes, reassembled via flags and sequence numbers.

---

### 12. Security

| Technology | Purpose |
|------------|---------|
| **VPN** | Encrypted tunnel |
| **Zero Trust** | Verify every request |
| **2FA** | Two-factor authentication |
| **IDS/IPS** | Intrusion Detection/Prevention Systems |
| **TLS** | Encryption, certificates, MITM protection |
| **RPKI** | BGP route signing and validation |

#### Routing Security as Supply Chain Risk

A 2024 Cloudflare incident demonstrated that routing failures at an unknown network in another country can silently reroute traffic, opening the door to man-in-the-middle attacks even when TLS is in use. Organizations should treat routing as a supply chain risk, map dependencies, and verify provider posture continuously.

---

### 13. Networking in DevOps and Cloud

| Tool | Networking Aspect |
|------|-------------------|
| **Docker** | Virtual networks, bridge, NAT |
| **Kubernetes** | Each pod has its own IP; automatic routing |
| **Terraform** | Networks described as code (IaC) |
| **Ansible** | Mass configuration of network devices |
| **Prometheus, Grafana** | Network monitoring |

---

### 14. Diagnostic Tools

| Tool | Purpose |
|------|---------|
| **ping** | Check host reachability |
| **traceroute** | Trace packet path |
| **netstat** | Open ports |
| **dig** | DNS diagnostics |
| **tcpdump** | Network traffic analysis |

---

### 15. Key Takeaways (Networking)

1. **The Internet is a grown system**—working solutions won, not perfect ones.
2. **Core protocols last for decades** (TCP/IP, DNS, BGP).
3. **Understanding the packet's journey** is the key to network troubleshooting.
4. **The Internet runs on trust and agreements**, not centralized control.
5. **Optimization for speed and cost**: GeoDNS → where to go, Anycast → nearest point, CDN → content already there.
6. **The Internet is transport; the Web is a service built on top of it.**

---

## Part II: HTML Fundamentals

### 16. What Is HTML?

**HTML (HyperText Markup Language)** is a markup language that tells web browsers how to structure the web pages you visit. HTML consists of a series of **elements** used to enclose, wrap, or mark up different parts of content to make it appear or act in a certain way.

- HTML lives inside text files called **HTML documents** (or just documents), with a `.html` file extension.
- The most common HTML file is `index.html`, generally used for a website's home page.
- **Tags are not case-sensitive** (`<title>`, `<TITLE>`, `<TiTlE>` all work), but **lowercase is best practice** for consistency and readability.

---

### 17. Anatomy of an HTML Element

A complete element consists of:

1. **Opening tag**: Element name wrapped in angle brackets (e.g., `<p>`). Marks where the element begins.
2. **Content**: The content of the element (e.g., "My cat is very grumpy").
3. **Closing tag**: Same as opening tag but with a forward slash (e.g., `</p>`). Marks where the element ends.

> **Common beginner error**: Forgetting the closing tag.

#### Nesting Elements

Elements can be placed within other elements. Proper nesting requires closing tags in **reverse order** of opening:

```html
<p>My cat is <strong>very</strong> grumpy.</p>
```

**Wrong nesting** (overlapping tags):
```html
<p>My cat is <strong>very grumpy.</p></strong>
```

When tags overlap, the browser must guess your intent, which can produce unexpected results.

#### Void Elements

Some elements consist of a single tag and **cannot contain other HTML content**. These are called **void elements**.

Example: `<br>` inserts a line break.

```html
<p>
  This is a single paragraph, but we are going to <br />break it onto two lines.
</p>
```

> **Note**: The trailing `/` in `<br />` is a different markup style—not wrong, but not needed.

---

### 18. Attributes

Attributes contain extra information about the element that isn't part of its content. They look like this:

```html
<p class="editor-note">My cat is very grumpy</p>
```

An attribute should have:

1. A **space** between it and the element name
2. The **attribute name**, followed by an **equals sign** (`=`)
3. An **attribute value**, wrapped in **opening and closing quote marks**

#### The `<img>` Element

The `<img>` element displays an image and takes several attributes:

| Attribute | Required? | Description |
|-----------|-----------|-------------|
| `src` | **Yes** | URL of the image |
| `alt` | No (but strongly recommended) | Text description for accessibility |
| `width` | No | Width in pixels |
| `height` | No | Height in pixels |

Example:
```html
<img src="https://example.com/image.png" alt="The Firefox Nightly icon" width="300" />
```

#### Boolean Attributes

Attributes written **without values**. When added, their value is set to `true`; if omitted, `false`.

Example: `disabled` on form inputs:

```html
<input id="first-input" type="text" disabled="disabled" />
<!-- Shorthand: -->
<input id="second-input" type="text" disabled />
```

#### Quotes Around Attribute Values

**Always include quotes.** Omitting them can break markup:

```html
<!-- BAD: browser interprets as three attributes (title="The", Mozilla, homepage) -->
<a href=https://www.mozilla.org/ title=The Mozilla homepage>favorite website</a>
```

**Single vs. double quotes**: Both work. Choose one style, but **don't mix them** in the same attribute.

```html
<!-- Fine -->
<a href='https://www.example.com'>A link</a>
<a href="https://www.example.com">A link</a>

<!-- BREAKS: mixed quotes -->
<a href="https://www.example.com'>A link</a>
```

To include quotes inside quotes of the **same type**, use character references:

```html
<a href="https://www.example.com" title="An &quot;interesting&quot; reference">A link</a>
```

---

### 19. Anatomy of an HTML Document

A complete webpage:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

| Part | Purpose |
|------|---------|
| `<!doctype html>` | **Doctype**. Historically a link to a set of rules (DTD). Now a **historical artifact** needed for everything to work correctly. Shortest valid doctype. |
| `<html></html>` | **Root element**. Wraps all content on the page. |
| `<head></head>` | Container for **information about the page** (not visible to users): keywords, description, CSS, character set. |
| `<meta charset="utf-8">` | Specifies **character encoding**. UTF-8 covers most human languages. |
| `<title></title>` | Sets the **page title** shown in the browser tab and bookmarks. |
| `<body></body>` | Contains **all visible content**: text, images, videos, games, audio, etc. |

---

### 20. Whitespace in HTML

In most cases, whitespace is **optional** and used for readability. These two snippets render identically:

```html
<p id="noWhitespace">Dogs are silly.</p>

<p id="whitespace">Dogs
    are
        silly.</p>
```

The HTML parser **reduces each sequence of whitespace to a single space** (exceptions: `<pre>`).

**Common style**: Two spaces of indentation per nesting level.

```html
<section>
  <div>
    <p>A paragraph of content.</p>
  </div>
</section>
```

---

### 21. Character References

The characters `<`, `>`, `"`, `'`, and `&` are **special** in HTML. To include them literally, use **character references** (start with `&`, end with `;`):

| Literal | Character Reference |
|---------|---------------------|
| `<` | `&lt;` |
| `>` | `&gt;` |
| `"` | `&quot;` |
| `'` | `&apos;` |
| `&` | `&amp;` |

Example:
```html
<p>In HTML, you define a paragraph using the &lt;p&gt; element.</p>
```

> **Note**: You don't need entity references for other symbols—modern browsers handle them fine with UTF-8 encoding.

---

### 22. HTML Comments

Comments are **ignored by browsers** and invisible to users. They allow you to include notes in the code.

Wrap comments in `<!--` and `-->`:

```html
<p>I'm not inside a comment</p>

<!-- <p>I am!</p> -->
```

Only the first paragraph renders.

---

### 23. Key Takeaways (HTML)

1. **HTML is a markup language** for structuring web content.
2. An element = **opening tag + content + closing tag**.
3. **Nesting must be correct**—close tags in reverse order.
4. **Void elements** (`<br>`, `<img>`) have no content and no closing tag.
5. **Attributes** provide extra information; always quote values.
6. **Boolean attributes** are true when present, false when absent.
7. Every document needs a **doctype**, `<html>`, `<head>`, and `<body>`.
8. **Whitespace is collapsed** by the parser (except in `<pre>`).
9. Use **character references** for special characters.
10. **Comments** (`<!-- -->`) are for notes, not rendered.



**Materials:**
1. Устройство интернета для новичков в IT. Как работает интернет (Александр Буртовой): https://www.youtube.com/watch?v=xnx2JDSV87Y;
2. СЕТИ ЗА 14 МИНУТ (Просто Devops): https://www.youtube.com/watch?v=qsKiA35prDs;
3. ВСE ЧТО НАДО ЗНАТЬ ПРО СЕТИ (Просто Devops): https://www.youtube.com/watch?v=a55ecIWIkVc;
4. ВСЕ ЧТО НАДО ЗНАТЬ ПРО ИНТЕРНЕТ (Просто Devops): https://www.youtube.com/watch?v=Ce-HDCrXMtQ;
5. ВСЕ ЧТО НАДО ЗНАТЬ ПРО СЕТИ ЧАСТЬ 2 (Просто Devops): https://www.youtube.com/watch?v=ylfaQfnF-wM;
6. 8 ГЛАВЫХ ВОПРОСОВ ПРО СЕТИ НА СОБЕСЕДОВАНИИ (Просто Devops): https://www.youtube.com/watch?v=6tZG2Rx64WY;
7. Сети для несетевиков // OSI/ISO, IP и MAC, NAT, TCP и UDP, DNS (Yuriy Semyenkov): https://www.youtube.com/watch?v=PYHKOwBfsLI;
8. How DNS Work: https://howdns.works/;
9. КАК УСТРОЕН ИНТЕРНЕТ. НАЧАЛО (Alek OS): https://www.youtube.com/watch?v=tRijLaXxSwU;
10. КАК УСТРОЕН TCP/IP? (Alek OS): https://www.youtube.com/watch?v=EJzitviiv2c;
11. КАК РАБОТАЕТ БРАУЗЕР? (Alek OS): https://www.youtube.com/watch?v=EAqrn9debZ0;
12. КАК РАБОТАЕТ WIFI? (Alek OS): https://www.youtube.com/watch?v=Hjl3vVLrSFo&list=PLIJLLSrXDPojRJcdstx9-KYsjz80G3ov-&index=4;
13. Basic HTML syntax: https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax?utm_source=chatgpt.com;