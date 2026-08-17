# SSH Threat Mitigation Plan

## 1. Purpose

This plan defines the actions recommended after identifying suspicious SSH authentication activity, including invalid-user attempts and repeated password failures against the root account.

The objective is to reduce the likelihood of unauthorised access, improve detection capability and protect privileged accounts.

## 2. Prioritised Action Plan

| ID | Action | Priority | Responsible Role | Target Time | Validation |
|---|---|---|---|---|---|
| M-01 | Review complete authentication logs for successful access | Critical | Security Analyst | Immediate | Confirm whether any suspicious login succeeded |
| M-02 | Disable direct SSH access for `root` | Critical | System Administrator | Same day | Root SSH login is rejected |
| M-03 | Configure SSH keys or MFA for administrators | High | System Administrator | 3 days | Approved administrators authenticate securely |
| M-04 | Restrict SSH using firewall rules, VPN or allowlists | High | Network Administrator | 3 days | Unapproved sources cannot reach TCP port 22 |
| M-05 | Configure rate limiting or Fail2ban | High | System Administrator | 3 days | Repeated failures trigger temporary blocking |
| M-06 | Patch the operating system and OpenSSH service | High | System Administrator | 7 days | Updates installed and documented |
| M-07 | Centralise logs and configure authentication alerts | Medium | Security Analyst | 14 days | Test events generate alerts |
| M-08 | Review privileged access regularly | Medium | IT Manager | Monthly | Access review completed and recorded |

## 3. Immediate Investigation

Before treating the event as resolved, the security team should:

1. Preserve the original SSH and authentication logs.
2. Search for successful logins following the failed attempts.
3. Review root and administrator SSH keys.
4. Check for recently created users or unexpected privilege changes.
5. Review firewall events associated with the suspicious sources.
6. Inspect scheduled tasks, services and startup items for unauthorised changes.
7. Escalate the incident if evidence of successful access is discovered.

## 4. SSH Hardening

Recommended OpenSSH configuration settings include:

```text
PermitRootLogin no
MaxAuthTries 3
PubkeyAuthentication yes
PasswordAuthentication no
AllowGroups ssh-users
```

Before disabling password authentication, administrators must confirm that SSH-key access works correctly in a separate session. This prevents accidental administrative lockout.

After changing the configuration, validate it before reloading the SSH service:

```bash
sudo sshd -t
sudo systemctl reload ssh
```

These commands are provided for a controlled Linux environment and may need to be adjusted for the operating system or service configuration in use.

## 5. Authentication Controls

### SSH Keys

Administrative users should authenticate using individual SSH keys rather than shared credentials. Private keys must be protected and revoked when access is no longer required.

### Multi-Factor Authentication

Where supported, MFA should be enabled for administrative access. This reduces the likelihood of compromise when a password or private key is exposed.

### Privileged Access

Direct root login should remain disabled. Administrators should use named accounts and elevate privileges only when required, creating a clearer audit trail.

## 6. Network Controls

SSH access should not be exposed unnecessarily to the public internet.

Recommended controls include:

- Restricting TCP port 22 to approved management networks
- Requiring VPN access before administration
- Using IP allowlists where appropriate
- Blocking sources that repeatedly violate access policies
- Separating administrative traffic from normal user traffic
- Reviewing firewall rules regularly

## 7. Detection and Monitoring

Alerts should be generated when the environment records:

- Multiple failed logins from the same source
- Attempts against `root` or another privileged account
- Several invalid usernames from one source
- Authentication attempts outside approved hours
- Successful access following repeated failures
- Changes to SSH configuration or authorised keys

SSH events should be correlated with firewall, endpoint and access-control logs to improve detection accuracy.

## 8. Patch and Configuration Management

The operating system and OpenSSH service should be kept current with security updates.

Configuration changes must be:

1. Reviewed before implementation
2. Tested in a controlled environment
3. Backed up before modification
4. Validated after implementation
5. Documented for audit and recovery purposes

## 9. Success Criteria

The mitigation plan will be considered effective when:

- Direct root SSH login is disabled
- Administrative access uses approved secure authentication
- Repeated failures automatically trigger blocking or alerts
- SSH is accessible only from approved networks or users
- Security updates are current
- Authentication logs are centrally monitored
- Privileged access is reviewed regularly

## 10. Residual Risk

No control completely removes the possibility of unauthorised access.

Residual risks include compromised administrator devices, stolen SSH keys, unknown software vulnerabilities and attackers using approved network locations. Continuous monitoring and periodic control reviews remain necessary.

## 11. Implementation Status

This document presents recommended actions for a simulated security case study. No changes were performed against a live production server.
