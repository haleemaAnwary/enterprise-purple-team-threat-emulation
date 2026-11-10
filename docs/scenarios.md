# Four controlled attack scenarios

## 1. API version downgrade / password reset
Burp Suite intercepted password-reset traffic. An alternate API version exposed a reset PIN, and the team demonstrated successful access to the controlled administrator account. Post-remediation, custom Wazuh detection identified the activity and an NGINX reverse proxy denied access to the vulnerable endpoint.

## 2. Broken Object Level Authorization (BOLA)
A controlled account-balance request was changed to reference another test account. The application returned data without enforcing ownership. The team deployed a Suricata rule for requests to the vulnerable balance endpoint. Network monitoring of an endpoint is not equivalent to a server-side object authorization fix.

## 3. Business logic abuse
The application accepted negative-value transfers, causing unintended changes to test balances. The baseline Wazuh configuration lacked Docker application log ingestion. The team configured log collection and custom Wazuh rules; repeated transfer abuse was then detected. **No server-side sign validation fix was deployed, so the attack remained successful.**

## 4. GraphQL trust-boundary failure
The group investigated a chained GraphQL application weakness that crossed trust boundaries through injection and overprivileged internal services. The remediation combined network intrusion signatures, PostgreSQL audit visibility, Sysmon event correlation, SSH credential restrictions and segmentation controls. Follow-up testing demonstrated multiple alert sources and that restricted SSH access impeded the original lateral-movement path.

All scenarios were performed inside an intentionally vulnerable academic lab.
