# SSH Threat Log Analysis

![Cybersecurity](https://img.shields.io/badge/Focus-Cybersecurity-blue)
![Log Analysis](https://img.shields.io/badge/Skill-Log%20Analysis-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## Project Overview

This project presents an analysis of SSH authentication logs from a simulated security environment. It was developed from coursework completed for **ICTCYS407 – Gather, Analyse and Interpret Threat Data**.

The objective was to identify suspicious authentication activity, assess potential security risks and recommend appropriate mitigation strategies.

> All log entries and IP addresses used in this repository have been sanitised or replaced with documentation addresses for privacy and portfolio purposes.

## Scenario

A Linux server recorded several unsuccessful SSH authentication attempts involving invalid usernames, reverse-mapping failures and repeated password attempts against the root account.

The activity was reviewed to determine whether it represented normal administrative errors, automated scanning or a possible brute-force attack.

## Project Objectives

- Gather and review SSH authentication events
- Identify suspicious login patterns
- Document inconsistencies within the data
- Consider false positives and false negatives
- Evaluate the likelihood and impact of each threat
- Recommend controls to reduce unauthorised access risks
- Present the findings in a professional security report

## Analysis Approach

**Elasticsearch** was selected as the analytical platform in the assessment scenario because it can index, search and filter large volumes of log data efficiently.

The analysis focused on:

- Failed authentication attempts
- Invalid usernames
- Repeated attempts against privileged accounts
- Source IP addresses
- Reverse DNS mapping failures
- Frequency and timing of events

## Key Findings

| Time | Sanitised Source | Target User | Observation | Assessment |
|---|---|---|---|---|
| 06:55:46 | 203.0.113.10 | `webmaster` | Invalid-user attempt and reverse-mapping failure | Possible automated account probing |
| 07:07:38 | 198.51.100.24 | `test9` | Failed login and reverse-mapping failure | Suspicious username enumeration |
| 07:11:42 | 192.0.2.45 | `chen` | Authentication attempt using an invalid user | Possible automated SSH scanning |
| 07:13:31 | 203.0.113.76 | `root` | Multiple failed password attempts | Likely brute-force activity against a privileged account |

The events show a pattern consistent with automated SSH scanning and credential attacks. However, a reverse-mapping failure alone does not prove malicious activity because it may also result from a DNS configuration issue.

## Threats Identified

1. **Username enumeration** – Attempts were made using several invalid or unauthorised usernames.
2. **Password brute-force activity** – Repeated password attempts targeted the privileged root account.
3. **Unauthorised access attempts** – External sources attempted to authenticate to the SSH service.
4. **Reverse DNS inconsistencies** – Some sources could not be successfully resolved.

## Risk Assessment

| Threat | Likelihood | Impact | Risk Level |
|---|---|---|---|
| Brute-force attack against root | High | High | High |
| Invalid-user authentication attempts | Medium | Medium | Medium |
| Unauthorised SSH access | Medium | High | High |
| Reverse DNS inconsistency | Medium | Low | Low–Medium |

## False Positives and False Negatives

Some failed login attempts may have been caused by legitimate users entering incorrect credentials or by DNS misconfiguration. These would be potential false positives.

False negatives are also possible because a sophisticated attacker may use valid credentials, distribute attempts across several IP addresses or imitate normal authentication patterns.

For this reason, SSH logs should be correlated with firewall, endpoint and access-control data before confirming a security incident.

## Recommended Mitigations

- Disable direct SSH login for the root account
- Use SSH keys or multi-factor authentication
- Apply strong password and account lockout policies
- Restrict SSH access through firewall rules, VPN or IP allowlists
- Use rate limiting or tools such as Fail2ban
- Keep operating systems and SSH services patched
- Monitor authentication logs and configure alerts
- Apply least-privilege access to administrative accounts

  ## Project Files

- [Sanitised SSH Log Dataset](logs/sanitised-ssh.log)
- [Detailed Threat Analysis](analysis/threat-analysis.md)
- [Security Analysis Report](reports/security-report.md)
- [Threat Mitigation Plan](remediation/mitigation-plan.md)

## Repository Structure

```text
ssh-threat-log-analysis/
├── README.md
├── logs/
│   └── sanitised-ssh.log
├── analysis/
│   └── threat-analysis.md
├── reports/
│   └── security-report.md
└── remediation/
    └── mitigation-plan.md
```

## Skills Demonstrated

- Cybersecurity log analysis
- SSH security fundamentals
- Threat identification
- Risk assessment
- Incident documentation
- Security control recommendations
- False-positive and false-negative evaluation
- Technical report writing

## Future Improvements

- Import the dataset into Elasticsearch
- Build a Kibana dashboard for authentication events
- Automate log parsing using Python
- Create SIEM alerts for repeated login failures

## Disclaimer

This is an educational cybersecurity project based on a simulated case study. No live system was attacked, accessed or tested as part of this project.

## Author

**Vinicius Bolacel da Cruz**  
Diploma of Information Technology | Advanced Diploma of Information Technology – Cyber Security
