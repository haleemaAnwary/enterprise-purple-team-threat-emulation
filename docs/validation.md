# Detection, remediation and validation

The report documents a **test–remediate–retest** workflow and shows why detection coverage and actual blocking must be evaluated separately.

| Scenario | Post-remediation observation | Remaining caveat |
|---|---|---|
| API downgrade | Wazuh Rule 100010 flagged legacy password reset; NGINX returned HTTP **403** rather than previously accepted HTTP 200 | Reverse proxy protects the tested vulnerable route; it is not a complete application source fix |
| BOLA | A targeted Suricata signature was created and validated at the network sensor | Network signature cannot establish whether the requested account belongs to the authenticated user |
| Negative-amount transfer | Docker logs ingested; Wazuh Rules 100010 and 100011 flagged events / bursts | Attack **still altered balances**; server-side validation remained unfixed |
| GraphQL chain | Suricata detections at multiple stages, correlated PostgreSQL and Sysmon alerts; SSH forced-command restriction tested | Full application-layer repairs and full SOAR workflow validation were not demonstrated |

## GraphQL detection examples
The report documents:
- Suricata SID **1000001** for introspection → Wazuh **100100**
- Suricata SID **1000002** for SQL injection activity → Wazuh **100101**
- Suricata SID **1000003** for database command-execution activity → Wazuh **100102**
- PostgreSQL log correlation → Wazuh **100201**
- Sysmon process activity → Wazuh **100300**

Rule IDs are quoted as **reported in the project**, not distributed as executable rules.

## Automation limitation
The team configured a Shuffle SOAR playbook and manually verified a custom `nft-block.sh` mechanism capable of adding an attacker IP to an nftables set. **The whole chain from Wazuh alert through Shuffle to automatic blocking could not be tested end-to-end** because of API credential constraints. Accordingly, this repository does not claim a fully operating production SOAR deployment.

## Takeaways
The project illustrates a critical distinction between observing malicious behavior and preventing it. In particular, Scenario 3 improved visibility but not application-layer security.
