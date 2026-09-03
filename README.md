

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
