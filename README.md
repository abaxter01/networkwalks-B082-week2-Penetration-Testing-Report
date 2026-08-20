# Penetration Testing Report 
## 🛡️ W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS - Phase 1 & 2: Footprinting and Network Scanning 

| Report Field | Details |
|--------------|---------|
|**Pentester Name**| Anieka Baxter |
|**Programme/Batch**|	B082 – Networkwalks|
|**Report Date**|	19 August 2026|
|**Modules Covered**|	W2-PM1 – Multiple Kali Tools; W2-PM5 – Zenmap Scanning |
|**Target/Scope**| Footprinting - networkwalks.com; Network Discovery - Tester's local LAN (192.168.43.0/24) |
|**Phases Covered**|	Phase 1: Reconnaissance & Footprinting; Phase 2: Scanning & Network Discovery |
|**Testing Status** |	Information gathering and host discovery only; no exploitation performed|

---

## 1. Legal Disclaimer & Authorization ⚠️
All activities documented in this report are intended for an authorized educational cybersecurity exercise. Reconnaissance and scanning should only be performed against systems, domains, and devices for which the tester has appropriate permission or ownership. The findings in this report describe observations produced by the supplied commands and scan results; they are not proof that a confirmed security vulnerability exists. No exploitation, credential attacks, denial-of-service activity, or destructive testing was performed as part of the activities covered here.

Any misuse of the techniques or information described in this report is the responsibility of the individual performing the activity. Testing outside an authorized scope may violate organizational policy and applicable law.

---

## 2. Introduction 📖
This report documents two practical penetration-testing activities. The first activity focused on reconnaissance and footprinting of the `networkwalks.com` domain using six Kali Linux tools: **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f**, and **DNSRecon**. The second activity used **Zenmap** (the graphical interface for Nmap) to perform a ping scan of the tester's local network subnet, `192.168.43.0/24`, in order to identify live hosts and associated MAC-address information.

The activities demonstrate the early stages of a penetration test, where a security professional gathers information before any deeper validation is attempted. The results can help establish an initial picture of the target's domain registration, DNS infrastructure, web technologies, HTTP configuration, defensive controls, and local network devices.

---

## 3. Tools Utilized ⚒️

| Tools | Purpose |
|--------------|---------|
|Kali Linux and Windows 11 Home | Operating systems used for reconnaissance activities |
| WHOIS	| Collect publicly available domain registration, registrar, status and name-server information. |
| WhatWeb |	Fingerprint web technologies, server software, CMS information and related website characteristics. |
| Nslookup |	Resolve the domain name to its IP address through DNS. |
|Curl (-I) |	Inspect HTTP response headers and identify information returned by the web server. |
| Wafw00f |	Identify whether a Web Application Firewall (WAF) is present. |
| DNSRecon |	Enumerate DNS records including NS, SOA, MX, A, TXT and SRV records. |
|Zenmap / Nmap |	Discover live hosts on the local subnet and collect IP and MAC-address information. |
| Windows CMD	| Local IP and MAC address identification |

---

## 4. Technical Assessment Activities 🖲️
### 4.1 Phase 1: Reconnaissance & Footprinting

#### 4.1.1 WHOIS

Command: `whois networkwalks.com`

| Attribute | Value |
|--------------|---------|
| Domain | NETWORKWALKS.COM |
| Registrar	| GoDaddy.com, LLC |
| Creation Date |	2019-11-06T22:51:46Z |
| Updated Date |	2025-11-12T10:08:43Z |
| Registry Expiry |	2027-11-06T22:51:46Z |
| Name Servers | NS6135.HOSTGATOR.COM; NS6136.HOSTGATOR.COM |
| DNSSEC | Unsigned |

The WHOIS result exposed registration metadata and identified HostGator name servers. This information contributes to an attacker's understanding of the domain's administrative and hosting environment, although the information is publicly available and does not by itself indicate a vulnerability.

#### 4.1.2 WhatWeb

Command:  `whatweb networkwalks.com`

**Key Findings**
- **HTTP Response:** 200 OK (HTTPS)
- **Web Server:** Apache
- **Content Management System:** WordPress 7.0.4
- **Plugins:** WordPress Download Manager 3.3.58
- **JavaScript Libraries:** jQuery 3.7.1
- **CSS Framework:** Bootstrap 7.0.4
- **Analytics:** Google Tag Manager
- **Markup:** HTML5
- **Server IP:** `192.232.216.135`
- **Site Title:** `Networkwalks Academy`

From a reconnaissance perspective, the CMS and component information is useful because technology fingerprints can guide subsequent authorized security review. The presence of version information should therefore be treated as an information-exposure observation rather than a confirmed vulnerability.

#### 4.1.3 Nslookup

Command:  `nslookup networkwalks.com`

The DNS lookup, using Google's DNS resolver at `8.8.8.8`, returned the address `192.232.216.135` for **networkwalks.com**. This confirms the IP address associated with the domain at the time of testing.

#### 4.1.4 Curl

Command:  `curl -I https://networkwalks.com`

**Key Findings**
- **Protocol:** HTTP/2 200
- **Server:** Apache
- **WordPress Integration:** Cache information and REST API endpoints
- **Security Cookies:** __wpdm_client with Secure/HttpOnly attributes
- **API Endpoints:** /wp-json/ WordPress REST API
- **Security Headers:** permissions-policy, referrer-policy

The /wp-json/ endpoint is a normal WordPress REST API path and was not tested for unauthorized access or exploitation. Its presence is documented only as part of the observed attack-surface information.

#### 4.1.5 Wafw00f

Command:  `wafw00f networkwalks.com`

Wafw00f identified the site as being behind **ModSecurity (SpiderLabs)** and reported two requests during detection. This is a positive defensive observation because a WAF is present, while the specific WAF technology is also visible to reconnaissance tools.

#### 4.1.6 DNSRecon

Command:  `dnsrecon -d networkwalks.com`

**DNS Record Analysis:**
- **SOA Records:** ns6135.hostgator.com, ns6136.hostgator.com
- **NS Records:** HostGator name servers confirmed
- **MX Records:** mail.networkwalks.com (192.232.216.135)
- **A Records:** Primary domain resolution
- **TXT Records:** 2 records identified
- **SRV Records:** Multiple cPanel email discovery records
- **BIND Version:** 9.16.23-RH
- **DNSSEC Status:** No DNSSEC answer available

DNSRecon identified SOA and NS records for ns6135.hostgator.com and ns6136.hostgator.com, an MX record pointing to mail.networkwalks.com at 192.232.216.135, an A record for the domain, two TXT records, and multiple SRV records for cPanel email discovery. The tool also reported BIND version 9.16.23-RH on the identified name servers and an error indicating that no DNSSEC answer was available.

#### 4.1.7 Reconnaisssance & Footprinting Summary

| Category |	Key Observation|
|----------|-----------------|
| Domain/Registration | GoDaddy registrar; HostGator name servers; DNSSEC reported unsigned.|
| Web Technology |	Apache; WordPress 7.0.4; WP Download Manager 3.3.58; jQuery 3.7.1.|
| IP Resolution |	networkwalks.com resolved to 192.232.216.135. |
| HTTP Response |	HTTP/2 200; WordPress REST API links and several response headers observed. |
| WAF |	ModSecurity (SpiderLabs) detected. |
| DNS |	SOA, NS, MX, A, TXT and SRV records observed; BIND version identified. |

---

### 4.2 Phase 2: Network Scanning & Discovery with Zenmap

For the network-scanning activity, Zenmap was used to perform a **ping scan** against the local subnet `192.168.43.0/24`. The supplied Nmap output shows that 256 IP addresses were scanned and **five hosts** were identified as active. The scan was a host-discovery exercise rather than a vulnerability or service-enumeration assessment.

**Observed command:**  `nmap -T4 -F 192.168.43.0/24`

Zenmap displayed the following live hosts. Hostnames are included only where they were returned by the scan.

| Host | IP	Status | MAC Address | Vendor / Identification |
|------|-----------|-------------|-------------------------|
| 192.168.43.1 |	Up |	02:9D:6B:2B:12:3D |	Unknown |
| 192.168.43.22 |	Up | 24:3F:75:40:71:86 |	Hui Zhou Gaoshengda Technology |
| 192.168.43.174 (Redmi-Pad-SE) | Up |	6E:6D:72:57:9F:39 |	Unknown |
| 192.168.43.241 (Redmi-Note-8-Pro)|	Up |	4C:63:71:19:A7:EF |	Xiaomi Communications |
| 192.168.43.29	| Up |	Not shown in supplied text |	Not shown |

The scan completed in approximately 5.24 seconds and reported five hosts up. The results demonstrate that several devices were visible on the local network, including a Redmi Pad SE, a Redmi Note 8 Pro, my local computer, and two additional IP addresses. The presence of a live host does not indicate that the device is compromised; it simply confirms that the host responded to the discovery scan.

#### 4.2.1 Network Discovery Interpretation

|Observation|	Security Significance|
|-----------|----------------------|
| 5 live hosts discovered |	Provides an initial inventory of active devices on the local subnet.|
| Device names visible |	Hostnames such as Redmi-Pad-SE and Redmi-Note-8-Pro provide useful device-identification clues.|
| MAC/Vendor information	| Vendor data can help distinguish device types and support asset inventory.|
| Unknown MAC vendors present |	Unknown vendor identification should be verified against the organization's device inventory.|

*No exploitation performed	The scan establishes reachability only; it does not confirm vulnerabilities or compromise.*

---

## 5. Risk Analysis / Impact 🚨

|# |	Finding / Observation |	Evidence |	Potential Impact |	Risk |
|--|------------------------|----------|-------------------|-------|
|1|	Web technology information exposed|	WhatWeb identified WordPress 7.0.4 and WP Download Manager 3.3.58.|	Technology/version information can assist targeted reconnaissance and vulnerability research. |	🟧Medium |
|2|	Public IP address identifiable|	Nslookup and WhatWeb returned 192.232.216.135.|	Provides information about the network location hosting the web service.|	🟡Low |
|3|	HTTP and API information exposed |	Curl returned server/WordPress-related headers and /wp-json/ links.|	May assist technology fingerprinting and further authorized enumeration.|	🟡Low |
|4|	WAF technology identifiable|	Wafw00f identified ModSecurity (SpiderLabs).|	Reveals part of the defensive architecture to a remote observer.|	🟡Low |
|5|	DNS infrastructure exposed |	DNSRecon identified NS, SOA, MX, TXT and SRV records and BIND version information.|	Can help build a broader infrastructure profile and identify services for further review.|	🟧Medium |
|6|	DNSSEC reported unsigned |	WHOIS/DNSRecon indicated no DNSSEC answer.|	May reduce protection against certain DNS tampering scenarios, depending on the broader DNS architecture.| 🟡Low |
|7|	Multiple live LAN hosts visible|	Zenmap identified five live hosts on 192.168.43.0/24.|	Unexpected or unmanaged devices may increase the local attack surface.|	🟧Medium |

> **Risk key:** 🚩Critical / ⭕High / 🟧Medium / 🟡Low. 

*The footprinting and ping-scan activities did not validate exploitable weaknesses. Further authorized testing would be required before assigning a vulnerability severity to any of these observations.*

---

## 6. Security Strengths Observed 🔐

|Security Control| Implementation Status | Assessment |
|----------------|-----------------------|------------|
|HTTPS Encryption | ✅ Implemented | HTTP/2 over TLS with proper certificate handling |
|Secure Cookie Configuration | ✅ Implemented | __wpdm_client cookie with Secure and HttpOnly attributes |
| Web Application Firewall | ✅ Implemented | ModSecurity (SpiderLabs) actively protecting web applications |
| Security Headers | ✅ Partially Implemented | Permissions-Policy and Referrer-Policy headers observed |
| Network Visibility | ✅ Baseline Established | Network discovery provides foundation for asset management |

---

## 7. Recommendations 💡

### 7.1 Information Security Enhancements
**1.  Technology Disclosure Management**
- Implement server signature suppression where operationally feasible
- Review CMS and plugin version disclosure policies
- Consider implementing custom error pages to minimize information leakage

**2.  Web Application Security**
- Maintain WordPress core and plugins at current supported versions
- Implement regular security update procedures
- Review plugin necessity and remove unused components

**3.  HTTP Security Hardening**
- Expand security header implementation (CSP, HSTS, X-Frame-Options)
- Review and minimize unnecessary technical disclosures
- Implement comprehensive security header policies

### 7.2 DNS & Infrastructure Security
**1.  DNS Security Enhancement**
- Evaluate DNSSEC implementation requirements and benefits
- Review public DNS record necessity and accuracy
- Implement DNS monitoring for unauthorized changes
  
**2.  Web Application Firewall Optimization**
- Maintain ModSecurity rule sets with regular updates
- Implement comprehensive logging and monitoring
- Review WAF configuration for false positive optimization

### 7.3 Network Security Management
**1.  Asset Inventory Management**
- Correlate discovered hosts with authorized device inventory
- Implement regular network discovery for baseline maintenance
- Establish device onboarding and decommissioning procedures

**2.  Unknown Device Investigation**
- Investigate unidentified MAC vendor associations
- Verify device ownership and network authorization
- Implement network access control where appropriate

---

## 8. Conclusion 🎯

During Week 2 of my Cybersecurity & Ethical Hacking internship, I completed practical activities covering footprinting, reconnaissance and network scanning.

For the footprinting and network-scanning exercises, I collected information about the `networkwalks.com` domain and identified active hosts on my local network. The six Kali Linux tools provided complementary information: **WHOIS** exposed registration and name-server details; **WhatWeb** identified web technologies; **Nslookup** resolved the domain to an IP address; **Curl** displayed HTTP response information; **Wafw00f** detected ModSecurity; and **DNSRecon** provided additional DNS and service records.

The Zenmap scan of `192.168.43.0/24` identified five live hosts, including devices identified as **Redmi-Pad-SE** and **Redmi-Note-8-Pro**. The results demonstrate how network discovery can establish an initial asset picture before deeper testing is performed.

Overall, the exercises reinforced that reconnaissance and scanning are important early phases of penetration testing. They can reveal useful information about technology, infrastructure and connected devices without requiring exploitation. At the same time, the observations in this report should not be interpreted as confirmed vulnerabilities. Any subsequent security testing should remain within the authorized scope and should validate the security impact of individual findings.

---

# Appendix - Evidence Observed

| Kali Tools | 
|-------|
| **WHOIS** |
| <img width="1365" height="663" alt="whois" src="https://github.com/user-attachments/assets/f94f795c-a8d2-4874-ab4c-ad1225f3b78c" />|
| **WhatWeb**|
|<img width="1365" height="667" alt="whatweb" src="https://github.com/user-attachments/assets/fac369ec-59a7-4535-bdf0-58c808918fc6" />|
| **Nslookup** |
|<img width="1365" height="667" alt="nslookup" src="https://github.com/user-attachments/assets/45c94085-cd91-4c9c-9d06-4cf78175a53f" />|
| **Curl** |
|<img width="1365" height="666" alt="curl" src="https://github.com/user-attachments/assets/c21384d3-b56d-4ba9-b742-9ceab709c883" />|
| **Wafw00f** |
| <img width="1365" height="667" alt="wafw00f" src="https://github.com/user-attachments/assets/35aca4ca-4484-4ae0-80af-5d671e632d00" />|
| **Dnsrecon** |
|<img width="1364" height="665" alt="dnsrecon" src="https://github.com/user-attachments/assets/46bc281c-3800-42ea-b6c4-fd8fd09479d0" />|

---

| Zenmap | 
|-------|
| <img width="1354" height="540" alt="Screenshot 2026-08-18 203639" src="https://github.com/user-attachments/assets/c5d8825d-e61a-4b99-9e94-92979e024d10" /> |

> END OF REPORT

---
## 👤 Author
**Anieka Baxter**

Cybersecurity Professional B082 Intern

LinkedIn: www.linkedin.com/in/anieka-baxter-b6156618b

---
### 📚 Program Information
**Program Name:** Cybersecurity at Networkwalks 

**Week:** 02 | **Project:** Penetration Testing Report | **Repository:** GitHub





