# Penetration Testing Report 
# Phase 1 & 2: Footprinting and Network Scanning 

| Report Field | Details |
|--------------|---------|
|Pentester Name| Anieka Baxter |
|Programme / Batch|	B082 – Networkwalks|
|Report Date|	19 August 2026|
|Modules Covered|	W2-PM1 – Multiple Kali Tools; W2-PM5 – Zenmap Scanning |
|Target / Scope| Footprinting - networkwalks.com; Network Discovery - Tester's local LAN (192.168.43.0/24) |
|Phases Covered|	Phase 1: Reconnaissance & Footprinting; Phase 2: Scanning & Network Discovery |
|Testing Status |	Information gathering and host discovery only; no exploitation performed|

---

## 1. Legal Disclaimer & Authorization ⚠️
All activities documented in this report are intended for an authorized educational cybersecurity exercise. Reconnaissance and scanning should only be performed against systems, domains, and devices for which the tester has appropriate permission or ownership. The findings in this report describe observations produced by the supplied commands and scan results; they are not proof that a confirmed security vulnerability exists. No exploitation, credential attacks, denial-of-service activity, or destructive testing was performed as part of the activities covered here.

Any misuse of the techniques or information described in this report is the responsibility of the individual performing the activity. Testing outside an authorized scope may violate organizational policy and applicable law.

---

## 2. Introduction
This report documents two practical penetration-testing activities. The first activity focused on reconnaissance and footprinting of the `networkwalks.com` domain using six Kali Linux tools: **WHOIS, WhatWeb, Nslookup, Curl, Wafw00f**, and **DNSRecon**. The second activity used **Zenmap** (the graphical interface for Nmap) to perform a ping scan of the tester's local network subnet, `192.168.43.0/24`, in order to identify live hosts and associated MAC-address information.

The activities demonstrate the early stages of a penetration test, where a security professional gathers information before any deeper validation is attempted. The results can help establish an initial picture of the target's domain registration, DNS infrastructure, web technologies, HTTP configuration, defensive controls, and local network devices.

---

## 3. Tools Utilized

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

