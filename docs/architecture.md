# Architecture and monitoring

The team deployed Vuln-Bank in an isolated virtualized environment representing a low-maturity financial institution.

| Zone | Role |
|---|---|
| External / simulated internet | Kali Linux attacker and gateway |
| DMZ | Dockerized Vuln-Bank web application and database components |
| Internal Server Zone | Windows Domain Controller, DNS and file services |
| Workstation Zone | Windows enterprise workstation |
| Security Management Zone | Wazuh SIEM, Shuffle SOAR, MISP / VECTR |

Suricata monitored traffic at the gateway; Wazuh centralized endpoint and network-related events. Subsequent validation used Docker and PostgreSQL logs plus Sysmon endpoint events to investigate attack behavior across layers.

This is an **architecture summary** derived from the academic group report, not a reproducible infrastructure-as-code release. The original report contains a topology diagram and detailed VM addressing.

## Workflow
Baseline attack → monitor and collect evidence → identify security gaps → implement detections and controls → retest → compare outcomes.
