# SSH Security Analysis Report

**Prepared by:** Vinicius Bolacel da Cruz  
**Report date:** 17 August 2026  
**Environment:** Simulated Linux SSH server  
**Overall status:** Suspicious activity identified — compromise not confirmed

## Executive Summary

An analysis of SSH authentication events identified patterns consistent with automated account probing and password guessing.

Several external sources attempted to authenticate using invalid usernames, while another source made repeated password attempts against the privileged `root` account. The root-account activity represents the most significant risk because successful access could provide complete administrative control of the server.

No successful authentication or confirmed compromise was identified in the provided dataset. However, the activity demonstrates that the SSH service was exposed to potentially malicious login attempts and requires additional security controls.

## Scope

The review examined a sanitised SSH log extract containing:

- Invalid-user login attempts
- Failed password attempts
- Root-account authentication failures
- Reverse DNS mapping warnings
- Authentication-failure disconnections

The supporting evidence is available in:

- [`logs/sanitised-ssh.log`](../logs/sanitised-ssh.log)
- [`analysis/threat-analysis.md`](../analysis/threat-analysis.md)

## Key Security Findings

### 1. Privileged Account Targeting

Four failed password attempts targeted the `root` account within approximately twelve seconds. This pattern is consistent with automated password guessing and creates a high risk because the account has elevated privileges.

### 2. Username Enumeration

Authentication attempts used the invalid usernames `webmaster`, `test9` and `chen`. Testing unrelated usernames may indicate automated account discovery or internet-wide SSH scanning.

### 3. Reverse DNS Warnings

Two source addresses generated reverse-mapping failures. These events require investigation but do not independently confirm spoofing or malicious activity.

### 4. No Confirmed Breach

The reviewed data does not contain a successful login following the failed attempts. Consequently, the evidence supports suspicious activity but not a confirmed system compromise.

## Risk Summary

| Risk | Likelihood | Impact | Rating |
|---|---|---|---|
| Brute-force attack against `root` | High | High | High |
| Unauthorised SSH access | Medium | High | High |
| Invalid-user authentication attempts | Medium | Medium | Medium |
| Reverse DNS inconsistency | Medium | Low | Low–Medium |

## Potential Business Impact

If an attacker successfully accessed a privileged account, possible consequences could include:

- Theft or modification of sensitive information
- Installation of malware or persistence mechanisms
- Disruption of business services
- Use of the server to attack other systems
- Financial and reputational damage
- Increased incident-response and recovery costs

## Recommended Actions

### Immediate Priority

1. Disable direct SSH login for the `root` account.
2. Use SSH keys or multi-factor authentication.
3. Restrict SSH access using firewall rules, VPN access or approved IP allowlists.
4. Configure automatic blocking for repeated authentication failures.

### Ongoing Security

1. Monitor SSH and system-authentication logs.
2. Create alerts for repeated failures and invalid usernames.
3. Apply operating-system and OpenSSH security updates.
4. Review administrative accounts and remove unnecessary access.
5. Correlate authentication activity with firewall and endpoint logs.
6. Regularly review access-control and password policies.

## Limitations

The supplied dataset represents a small period of activity and does not include successful logins, firewall events, endpoint telemetry or threat-intelligence data.

Further investigation would be required to determine whether the source addresses were associated with known malicious activity or whether any access occurred outside the reviewed period.

## Lessons Learned

This case demonstrates that unsuccessful login attempts can provide valuable early indicators of malicious activity.

Monitoring authentication logs, protecting privileged accounts and correlating multiple security-data sources are essential for distinguishing normal user errors from coordinated attacks.

It also demonstrates the importance of evaluating false positives and avoiding unsupported conclusions. A reverse DNS failure or unsuccessful login should be investigated, but neither event alone proves that a system was compromised.

## Final Assessment

The SSH activity should be treated as a legitimate security concern requiring hardening and continued monitoring.

The available evidence indicates probable automated scanning and brute-force activity, but no successful unauthorised access was confirmed.
