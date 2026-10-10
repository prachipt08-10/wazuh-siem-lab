# Incident Report: SSH Brute-Force Detection via Wazuh SIEM

**Date:** October 2026
**Monitored host:** ubuntu-victim (10.0.2.4)
**Attacker:** Kali Linux (10.0.2.15)
**SIEM:** Wazuh 4.9.2 (self-hosted, 10.0.2.5)

## Summary
A Wazuh agent was deployed on the target server and connected to a
self-hosted Wazuh manager and dashboard. Simulated SSH brute-force
attempts were then launched from Kali Linux to validate detection.

## Detection Timeline
| Time (UTC) | Event | Rule ID | Level |
|---|---|---|---|
| 12:40:29–12:41:01 | sshd: Attempt to login using a non-existent user (x4) | 5710 | 5 |
| 12:41:01 | syslog: User missed the password more than one time | 2502 | 10 |
| 12:41:39 | PAM: User login failed | 5503 | 5 |
| 12:41:39 | unix_chkpwd: Password check failed | 5557 | 5 |
| 12:41:41 | sshd: authentication failed | 5760 | 5 |

## Key Observations
1. **Correlation over single events**: individual failed logins were
   scored level 5, but once Wazuh detected repeated failures from
   the same source, it escalated to level 10, the same kind of
   pattern-based thinking a human analyst applies.
2. **MITRE ATT&CK mapping**: alerts were automatically classified
   under Password Guessing, SSH, and Brute Force techniques, with no
   manual tagging required.
3. **Compliance relevance**: the triggering rule (5760) is mapped to
   GDPR, HIPAA, PCI DSS, and GPG13 controls, showing how detection
   rules tie back to real audit requirements.
4. **Rule family**: related rules (5701–5710) show escalating
   severity for related behaviors, e.g. "Possible breakin attempt"
   (level 10), "OpenSSH challenge-response exploit" (level 14).

## Comparison to Manual Detection
In an earlier project, the same type of attack was detected manually
using `journalctl` and a custom Bash script. Wazuh automated that
entire process: detection, correlation, severity scoring, and
framework mapping, in real time, through a dashboard.

## Recommended Actions
1. Enable active response rules to auto-block IPs after repeated
   failures (similar to fail2ban, but centrally managed)
2. Tune alert thresholds for the organization's risk tolerance
3. Forward high-severity alerts to a ticketing/notification system
4. Periodically review the agent's compliance mapping for audit prep

## Evidence
![Wazuh dashboard overview](screenshots/07-wazuh-dashboard.png)
![Agent connected](screenshots/08-wazuh-agent-connected.png)
![SSH alerts dashboard](screenshots/09-wazuh-ssh-alerts.png)
![Rule detail](screenshots/10-wazuh-rule-detail.png)
