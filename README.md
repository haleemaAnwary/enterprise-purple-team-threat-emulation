# Enterprise Purple Team & Threat Emulation

**Securing Vuln-Bank: A Purple Team Exercise in Enterprise Threat Emulation and Automated Detection Engineering**  
**SPR708 – Cybersecurity Engineering | Academic group project | August 2026**

A Red/Blue collaborative security exercise against an intentionally vulnerable banking application deployed inside a segmented virtual enterprise. The team executed four attack scenarios, measured baseline monitoring gaps, implemented targeted detections and mitigations, and repeated the tests to validate improvements.

**Project evidence:** [Full group report (PDF)](<docs/Copy%20of%20Purple%20Team%20Project%20Report.pdf>) · [Attack scenarios](docs/scenarios.md) · [Detection/validation](docs/validation.md)

## Team
**Haleemah Anwary · Sharisse Esguerra · Govind Sharma · Joshua Petrone**

This is a **group project**. The technical work and findings below describe the team's documented implementation and results.

## Tools and methods
**Security monitoring:** Wazuh SIEM, Sysmon, Suricata IDS/IPS, PostgreSQL and Docker logs  
**Security automation and tracking:** Shuffle SOAR, MISP, VECTR (part of the project environment; end-to-end SOAR response not confirmed)  
**Application and systems:** Vuln-Bank, NGINX, Docker, PostgreSQL, Windows Server, Active Directory, Kali Linux  
**Testing:** Burp Suite, API/GraphQL testing, MITRE ATT&CK mapping, purple-team test–remediate–retest workflow

## Four attack scenarios

| Scenario | Baseline weakness | Validated improvement |
|---|---|---|
| **1. API version downgrade** | Legacy password reset API exposed reset information, enabling admin account takeover | Wazuh detection and NGINX HTTP 403 blocking of the vulnerable endpoint |
| **2. Broken Object Level Authorization (BOLA)** | Account balance endpoint returned another test account's information | Targeted Suricata signature to observe suspicious balance-endpoint requests; does **not** prove authorization enforcement |
| **3. Negative-amount transfer** | Invalid transfer input modified balances without relevant application logs reaching Wazuh | Docker log collection and Wazuh detection; **underlying business-logic flaw remained exploitable** |
| **4. GraphQL trust-boundary failure** | Injection and excessive permissions allowed a multi-step pivot toward internal systems | Layered Suricata, database and endpoint detections; restricted SSH credentials and segmented traffic controls |

See [scenario details](docs/scenarios.md) and [validation findings](docs/validation.md).

## Enterprise architecture
The lab separated an external attacker, a DMZ application, internal server services, corporate workstation, and security management into different network zones. See [architecture](docs/architecture.md).

## Important limitations
- The Shuffle SOAR workflow was configured, and a custom nftables blocking script was manually tested, **but end-to-end automated containment was not validated** because of Wazuh API credential constraints.
- Detection should not be mistaken for prevention: the negative-amount transfer remained successful after SIEM coverage improved.
- The available portfolio is **documentation-based**; complete original infrastructure and source assets have not been provided here.
- All testing took place against an intentionally vulnerable, controlled lab environment.

## Documentation
- [Complete group project report (PDF)](<docs/Copy%20of%20Purple%20Team%20Project%20Report.pdf>)
- [Architecture and monitoring](docs/architecture.md)
- [Attack scenarios](docs/scenarios.md)
- [Detection and validation results](docs/validation.md)
- [Publication notes and attribution](docs/publication-notes.md)

**Source:** *Securing Vuln-Bank: A Purple Team Exercise in Enterprise Threat Emulation and Automated Detection Engineering*, SPR708 group report, August 12, 2026.
