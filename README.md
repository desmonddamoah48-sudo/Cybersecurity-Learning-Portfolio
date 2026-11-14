# 🔎 W2-PM1 | Footprinting and Reconnaissance with Multiple Tools

<p align="center">
  <strong>WEEK 2 CYBERSECURITY PRACTICAL</strong><br>
  Information Gathering and Website Reconnaissance
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Kali%20Linux-blue?style=for-the-badge&logo=kalilinux" alt="Kali Linux">
  <img src="https://img.shields.io/badge/Focus-Reconnaissance-red?style=for-the-badge" alt="Reconnaissance">
  <img src="https://img.shields.io/badge/Target-networkwalks.com-green?style=for-the-badge" alt="Training target">
</p>

---

## 📌 Project Overview

This practical focused on footprinting and reconnaissance against **networkwalks.com**, the training target provided for the Week 2 exercise. Six command-line tools in Kali Linux were used to collect and examine publicly visible domain, website, DNS, HTTP-header and web-application-firewall information.

The purpose was to understand how different reconnaissance tools reveal different parts of a website's public-facing environment and how to interpret their outputs responsibly.

## 🎯 Objectives

- Review domain registration details and name servers using WHOIS.
- Identify website technologies using WhatWeb.
- Resolve the domain name to an IP address using Nslookup.
- Inspect HTTP response headers using Curl.
- Check for a detectable web application firewall using WAFW00F.
- Enumerate returned DNS records using DNSRecon.
- Record findings and preserve supporting screenshots.

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Operating system used for the practical |
| WHOIS | Domain registration, registrar and name-server information |
| WhatWeb | Website and server technology fingerprinting |
| Nslookup | DNS name resolution |
| Curl (`-I`) | HTTP response-header inspection |
| WAFW00F | Web application firewall detection |
| DNSRecon | DNS record enumeration |

## 🧪 Practical Tasks and Findings

### 1. WHOIS – Domain Registration

**Command**
```bash
whois networkwalks.com
```

**Observed findings**
- Registrar: GoDaddy.com, LLC.
- Creation date: 6 November 2019.
- Expiry date: 6 November 2027.
- Registrant privacy service: Domains By Proxy, LLC.
- Name servers listed in the registry: `NS6135.HOSTGATOR.COM` and `NS6136.HOSTGATOR.COM`.
- Registrar section also displayed `NS29.DOMAINCONTROL.COM` and `NS30.DOMAINCONTROL.COM`.
- DNSSEC was reported as unsigned.

**Why it matters:** WHOIS can reveal domain registration history, registrar information and DNS-related details. The privacy service meant the registrant's personal details were not publicly displayed in the returned record.

### 2. WhatWeb – Website Technology Fingerprinting

**Command**
```bash
whatweb networkwalks.com
```

**Observed findings**
- HTTP returned `301 Moved Permanently` and redirected to HTTPS.
- HTTPS returned `200 OK`.
- Detected server: Apache.
- Resolved IP shown by the tool: `192.232.216.135`.
- Detected technologies included WordPress 7.1.4, WordPress Download Manager 3.3.76, jQuery 3.7.1, Bootstrap 7.1.4, Google Tag Manager, HTML5 and Open Graph Protocol.
- Page title: `Networkwalks Academy`.

**Why it matters:** Technology fingerprinting helps defenders identify software components that may need maintenance and updates. A detected version alone does not prove that the software is vulnerable.

### 3. Nslookup – DNS Resolution

**Command**
```bash
nslookup networkwalks.com
```

**Observed findings**
- DNS server queried: `10.74.230.65`.
- DNS port: `53`.
- Returned IPv4 address: `192.232.216.135`.
- The response was marked non-authoritative.

**Why it matters:** DNS resolution shows the IP address returned for a domain at the time of the query. A non-authoritative response is supplied by a resolver rather than directly by the domain's authoritative name server.

### 4. Curl – HTTP Response Headers

**Command**
```bash
curl -I https://networkwalks.com
```

**Observed findings**
- Response: `HTTP/1.1 200 OK`.
- Server header identified Apache.
- Content type: `text/html; charset=UTF-8`.
- WordPress REST API links included `/wp-json/` and `/wp-json/wp/v2/pages/53`.
- A cookie named `_wpdm_client` was set with `Secure` and `HttpOnly` attributes.
- Other returned headers included `Referrer-Policy: no-referrer-when-downgrade`, `X-Endurance-Cache-Level: 0` and `X-nginx-cache: WordPress`.

**Why it matters:** HTTP headers provide information about server responses and some security-related settings. The presence of a server name or API link is an observation, not proof of a security weakness.

### 5. WAFW00F – Web Application Firewall Detection

**Command**
```bash
wafw00f networkwalks.com
```

**Observed findings**
- WAFW00F version: 2.4.2.
- The tool checked `https://networkwalks.com`.
- It reported ModSecurity (SpiderLabs) as detected.
- The result was reached using two requests.

**Why it matters:** WAF detection identifies a defensive technology that may be protecting a web application. It does not establish how effectively the firewall is configured or whether it can be bypassed.

### 6. DNSRecon – DNS Record Enumeration

**Command**
```bash
dnsrecon -d networkwalks.com
```

**Observed findings**
- SOA record: `ns6135.hostgator.com` (`50.87.144.87`).
- MX record: `mail.networkwalks.com`, resolving to `192.232.216.135`.
- A record: `networkwalks.com`, resolving to `192.232.216.135`.
- Eight SRV records for `_autodiscover._tcp.networkwalks.com` targeted `cpanelemaildiscovery.cpanel.net` on port `443`.
- The output reported no answer for the DNSSEC query.
- Separate NS, TXT and SPF records did not appear in the captured output and are therefore not claimed as findings here.

**Why it matters:** DNS records can reveal information about domain services, mail routing and service endpoints. Findings are limited to the records returned by this enumeration.

## 📸 Screenshots and Evidence

The screenshots below were extracted from the submitted Week 2 report.

### WHOIS output
![WHOIS evidence 1](Screenshots/whois-01.png)

![WHOIS evidence 2](Screenshots/whois-02.png)

![WHOIS evidence 3](Screenshots/whois-03.png)

![WHOIS evidence 4](Screenshots/whois-04.png)

### WhatWeb output
![WhatWeb evidence](Screenshots/whatweb.png)

### Nslookup output
![Nslookup evidence](Screenshots/nslookup.png)

### Curl response headers
![Curl evidence](Screenshots/curl-headers.png)

### WAFW00F output
![WAFW00F evidence](Screenshots/wafw00f.png)

### DNSRecon output
![DNSRecon evidence](Screenshots/dnsrecon.png)

## 🧠 Skills Demonstrated

- Domain footprinting and registration research.
- Website technology fingerprinting.
- DNS lookup and record interpretation.
- HTTP response-header inspection.
- Web application firewall identification.
- Evidence collection and technical documentation.
- Responsible use of reconnaissance tools.

## ⚠️ Scope and Disclaimer

This activity was performed for educational purposes against **networkwalks.com**, the training target provided for the exercise. The report records observations from the captured tool outputs. No exploitation, password attacks or full vulnerability assessment were performed. Reconnaissance and security testing should only be conducted on systems you own or have explicit permission to assess.

## 🏁 Conclusion

The six tools provided complementary views of the target's public-facing information. WHOIS showed registration details; WhatWeb identified technologies; Nslookup resolved the domain; Curl displayed HTTP headers; WAFW00F detected ModSecurity; and DNSRecon returned selected DNS records. The exercise strengthened my ability to collect, interpret and document reconnaissance findings while staying within an authorized scope.

---

<p align="center">
  <strong>Cybersecurity Learning Portfolio | Week 2 | W2-PM1</strong>
</p>
