

I am Murtaza Ahmad, a proactive and analytical cybersecurity specialist dedicated to securing enterprise networks through threat monitoring, log analysis, and incident response. Grounded in computer science fundamentals and driven by a commitment to data integrity, I leverage system diagnostics, virtual testing environments, and vulnerability management tools to mitigate risks before exploitation. I aim to deliver resilient defense strategies that maintain business continuity and safeguard critical organizational assets.

---

## Portfolio Sections

- 📁 **Labs & Simulations:** Hands-on virtual machine environments, VirtualBox configurations, and diagnostics.
- 📁 **Security Projects:** Network analysis, log monitoring, and threat assessment write-ups.
- 📁 **Certifications & Documentation:** Professional achievements and coursework documentation.

- ---

## 🛡️ Featured Project: Botium Toys Security Audit

### 📌 Project Overview
This project presents an internal security audit, compliance assessment, and risk management strategy for **Botium Toys**, evaluating its IT infrastructure to identify vulnerabilities, measure regulatory compliance, and provide technical and administrative recommendations.

### 1. Controls Checklist

| Control Category | Included? | Context / Explanation |
| :--- | :---: | :--- |
| **Least Privilege** | **No** | Access controls are not currently configured according to least privilege; employees have excess access rights across systems. |
| **Disaster Recovery Plans** | **No** | Botium Toys lacks a formal disaster recovery plan to ensure business continuity and data recovery in the event of an incident. |
| **Password Policies** | **No** | Password complexity and rotation requirements are not currently established or enforced. |
| **Separation of Duties** | **No** | Responsibilities are not divided among multiple personnel, creating single points of failure and operational risks. |
| **Firewall** | **Yes** | A perimeter firewall is deployed to monitor and control incoming/outgoing network traffic. |
| **Access Control Lists (ACLs)** | **Yes** | ACLs are implemented on network devices to filter traffic based on IP addresses and protocols. |
| **Antivirus Software** | **Yes** | Antivirus/anti-malware software is installed and running on organizational endpoints. |
| **Encryption** | **No** | Sensitive data (such as customer payment details and personal data) is not encrypted both at rest and in transit. |
| **Password Management System** | **No** | Employees do not utilize a centralized enterprise password manager to generate and store credentials securely. |
| **Security Awareness Training** | **No** | No formalized security training exists for staff to recognize phishing, social engineering, or operational risks. |

### 2. Compliance Checklist

| Regulation / Compliance Standard | Compliant? | Context / Explanation |
| :--- | :---: | :--- |
| **PCI DSS** *(Payment Card Industry Data Security Standard)* | **No** | Internal processing and storing of payment card details lack mandatory encryption, segmenting, and strict access controls required by PCI DSS. |
| **GDPR** *(General Data Protection Regulation)* | **No** | Processing and holding personal data of EU customers without required data protection controls, explicit consent mechanisms, or data encryption violates GDPR mandates. |
| **SOC 1 / SOC 2** *(System and Organization Controls)* | **No** | Internal controls related to security, availability, and processing integrity are not formally defined, audited, or maintained. |

### 3. Recommendations (IT Manager)

* **Technical Safeguards:**
  * **Encryption:** Enable end-to-end encryption (TLS/HTTPS for data in transit; AES-256 for data at rest) for sensitive customer and financial data to comply with PCI DSS and GDPR.
  * **Access Control & Least Privilege:** Restrict user permissions using Role-Based Access Control (RBAC) so employees only have access to data necessary for their roles.

* **Administrative & Policy Controls:**
  * **Password Policy & Manager:** Deploy an enterprise password manager and enforce strict password policy guidelines (e.g., length, multi-factor authentication).
  * **Security Training:** Conduct routine security awareness training for all staff focusing on phishing, data handling, and password hygiene.
  * **Disaster Recovery (DR) & Business Continuity:** Formulate and test a formal Disaster Recovery Plan to maintain operational continuity and minimize downtime during security incidents.

---------------------------------------------------------------------------------------------------------------------------

# Cybersecurity Incident Report: DNS Resolution Failure

## Project Overview

* **Project Title:** Network Traffic & Incident Analysis (DNS/ICMP Failure)
* **Role:** Cybersecurity Analyst
* **Core Skills & Focus:** Network Protocol Analysis, Packet Inspection, Incident Diagnosis, Root Cause Identification, Incident Response Reporting.
* **Tools Used:** `tcpdump`, Packet Capture Analysis, Linux Command Line utilities (`systemctl`, `journalctl`, `iptables`, `dig`).

### Executive Summary
This project analyzes a simulated network incident involving service disruption for the client web domain `www.yummyrecipesforme.com`. When users were unable to load the site, packet capture data was gathered using `tcpdump` to inspect traffic between the client host and the authoritative DNS server. 

Through analysis of UDP and ICMP packet interactions, the root issue was pinpointed to a failure at the transport/application layer: DNS resolution queries on port 53 were failing due to the target host actively returning ICMP "Port Unreachable" responses. This report documents the technical findings, packet-level data interpretation, suspected root causes, and recommended containment and remediation procedures for engineering teams.

---

## Section 1: Summary of the Problem (tcpdump Log Analysis)

### Protocols Identified
* **UDP** (User Datagram Protocol)
* **DNS** (Domain Name System)
* **ICMP** (Internet Control Message Protocol)

### Summary of Traffic
* **Outgoing Traffic:** The host machine (`192.51.100.15`) sent an initial outgoing DNS request via **UDP** to port `53` of the destination DNS server (`203.0.113.2.domain`) to resolve the domain `www.yummyrecipesforme.com`.
* **Incoming Traffic:** In response to the UDP packet, the DNS server returned an **ICMP** error packet back to the host machine containing the message: `udp port 53 unreachable`.

### Key Log Details & Interpretation
| Attribute | Detail / Value |
| :--- | :--- |
| **Timestamp** | `13:24:32.192571` (1:24 PM) |
| **Source IP** | `192.51.100.15` (Host Machine) |
| **Destination IP** | `203.0.113.2` (DNS Server) |
| **Destination Port** | Port `53` (DNS Service) |
| **Query ID & Flags** | `35084` (`A?` record request mapping domain to IPv4) |
| **Error Returned** | ICMP `destination port unreachable` (`udp port 53 unreachable`) |

* **Log Trend:** The log demonstrates three consecutive UDP attempts from the browser to the DNS server, all resulting in identical ICMP unreachable error responses.
* **Impacted Service:** **DNS Service (Port 53 / UDP)**. Because DNS resolution failed, the client browser was unable to obtain the IP address required to initiate an HTTPS connection to display `www.yummyrecipesforme.com`.

---

## Section 2: Analysis of the Data and Next Steps

### Overview & Initial Reporting
* **Time First Reported:** 1:24 PM (`13:24:32.192571`)
* **Scenario & Reported Symptoms:** Multiple client customers reported being unable to access `www.yummyrecipesforme.com`, encountering a `destination port unreachable` error after extended loading times. The issue was reproduced upon investigation and confirmed via `tcpdump` packet capture.
* **Current Status:** Escalate to security engineers for system-level troubleshooting and service restoration.

### Investigation Findings
The capture of ICMP Type 3 / Code 3 (`Destination Unreachable - Port Unreachable`) error messages confirms that the network pathway to host `203.0.113.2` is open, but **no active process is listening on UDP port 53** on the target server.

### Suspected Root Cause
1. **DNS Daemon Failure:** The primary DNS service daemon (e.g., `bind9`, `named`, `unbound`) on server `203.0.113.2` crashed or was stopped unexpectedly.
2. **Firewall / Security Rules:** A recent firewall rule update (host-based `iptables`/`ufw` or network-level ACL) is dropping/rejecting incoming traffic on UDP port 53.
3. **Denial of Service (DoS):** Resource exhaustion from excessive queries caused the DNS daemon to crash.

### Recommended Next Steps
- [ ] **1. Inspect DNS Daemon Status:** SSH into target server `203.0.113.2` and inspect process status (`systemctl status named` or `systemctl status bind9`).
- [ ] **2. Restart Service & Analyze Logs:** Restart the DNS service if inactive and review system logs (`/var/log/syslog` or `journalctl -u bind9`) for crash origins.
- [ ] **3. Validate Firewall Rules:** Check active host rules (`sudo iptables -L -n -v`) to confirm UDP port 53 is open to incoming requests.
- [ ] **4. Test Resolution:** Perform verification testing using `dig @203.0.113.2 www.yummyrecipesforme.com` to confirm A record query resolution.
---------------------------------------------------------------------------------------------------------------------------

# Security Risk Assessment Report: Network Hardening & Vulnerability Remediation

## Project Description
This project is a comprehensive security risk assessment conducted for a social media organization following a major data breach that exposed customer personally identifiable information (PII). As a Security Analyst, I audited the internal infrastructure, identified four critical security vulnerabilities, analyzed the associated risks, and established a standardized network hardening roadmap to protect the organization from future unauthorized access and breaches.

---

## Part 1: Vulnerability Assessment & Risk Analysis

The security audit revealed four major vulnerabilities within the organization's network infrastructure:

### 1. Employees Sharing Passwords
* **Risk Level:** High
* **Threat & Impact:** Sharing credentials destroys individual accountability and non-repudiation. If a compromised account performs unauthorized actions, security logs cannot trace the activity back to a specific individual. Shared credentials also increase the likelihood of password leaks through unencrypted channels (chat applications, sticky notes, plain text files).

### 2. Default Database Admin Password
* **Risk Level:** Critical
* **Threat & Impact:** Default passwords for common database systems (such as MySQL or PostgreSQL) are publicly documented in manufacturer manuals and easily accessible to attackers. Automated scanning scripts routinely scan internet-facing and internal networks for default credentials to gain full administrative access to sensitive customer databases.

### 3. Lack of Firewall Filtering Rules
* **Risk Level:** Critical
* **Threat & Impact:** Without inbound and outbound packet filtering rules, the firewall acts as an open gateway. Untrusted traffic from the internet can enter the internal network freely, and malicious actors or compromised internal hosts can communicate with external command-and-control (C2) servers without restriction.

### 4. Absence of Multi-Factor Authentication (MFA)
* **Risk Level:** High
* **Threat & Impact:** Relying solely on single-factor authentication (passwords) leaves the organization highly vulnerable to credential harvesting, phishing, brute-force attacks, and credential stuffing. Once a password is compromised, the attacker gains immediate access to systems.

---

## Part 2: Security Hardening Recommendations & Justification

To address these vulnerabilities and establish baseline security practices, the following remediation measures must be implemented.

| Vulnerability | Recommended Hardening Practice | Implementation Frequency |
| :--- | :--- | :--- |
| **Shared Passwords** | Password Manager & Unique Account Policy | One-time setup / Continuous compliance |
| **Default Database Password** | Immediate Credential Rotation & Password Complexity Rules | One-time change / 90-day updates |
| **Missing Firewall Rules** | Configure Inbound & Outbound Access Control Lists (ACLs) | One-time setup / Quarterly audits |
| **Missing MFA** | Mandatory Multi-Factor Authentication Deployment | Continuous enforcement |

---

### Hardening Practice Explanations

#### 1. Implement Multi-Factor Authentication (MFA) & Password Management
* **Effectiveness:** MFA adds a secondary verification step (such as a time-based authenticator code or hardware security token) that an attacker cannot replicate with a stolen password alone. Combined with unique user accounts managed through an enterprise password manager, employees no longer need to share credentials or reuse simple passwords.
* **Frequency:** **Continuous.** MFA must be strictly required for every user login attempt across all corporate applications, VPNs, and administrative portals.

#### 2. Rotate Default Credentials & Implement Complexity Standards
* **Effectiveness:** Changing default database passwords to unique, high-entropy passphrases neutralizes automated password-guessing tools and default credential databases. Enforcing strict database access controls ensures that only authorized application service accounts can query customer databases.
* **Frequency:** **Immediate one-time remediation**, followed by mandatory password rotation every **90 to 180 days** or whenever administrative personnel change roles.

#### 3. Establish Stateful Firewall Rules & Default-Deny Policies
* **Effectiveness:** Configuring firewall Access Control Lists (ACLs) with an explicit "Default Deny" posture ensures that all inbound and outbound traffic is blocked unless explicitly permitted by business requirements. Inspecting traffic at the perimeter prevents unauthorized external connections and stops infected internal machines from exfiltrating customer data.
* **Frequency:** **Initial setup with regular quarterly reviews.** Firewall rule sets must be audited quarterly to eliminate stale rules and update configurations as network requirements evolve.
___________________________________________________________________________________________________________________________
