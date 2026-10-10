# Wazuh SIEM Lab

Deployed a real SIEM (Wazuh) in a home lab to detect, correlate, and
classify SSH brute-force attacks, building on my earlier manual log
analysis project.

## Lab Setup
- VirtualBox with a NAT Network (isolated lab)
- Wazuh server (indexer + manager + dashboard): Ubuntu Server, 10.0.2.5
- Monitored endpoint: Ubuntu Server with Wazuh agent, 10.0.2.4
- Attacker: Kali Linux, 10.0.2.15

## What I Did
1. Installed the Wazuh all-in-one stack (indexer, manager, dashboard)
2. Set up port forwarding so the dashboard was reachable from the host
3. Deployed and configured a Wazuh agent on the monitored server
4. Simulated SSH brute-force attempts from Kali
5. Observed Wazuh automatically detect, correlate, and escalate the
   alerts, mapping them to MITRE ATT&CK techniques and compliance
   frameworks (GDPR, HIPAA, PCI DSS)

## Results
- 44 total alerts generated in 24 hours from one test attack
- 9 authentication-failure alerts correctly attributed to the attack
- Alerts automatically classified under MITRE ATT&CK: Password
  Guessing, SSH, Brute Force
- Rule severity escalated from level 5 (single failed login) to
  level 10 ("User missed the password more than one time"),
  showing Wazuh's correlation engine in action

## Troubleshooting (real issues I solved)
- Fixed a DNS resolution failure on the monitored server
  (`resolvectl dns` to force a working DNS server)
- Diagnosed a VM CPU soft-lockup caused by insufficient host memory,
  resolved by freeing RAM and increasing VM allocation
- Fixed an agent misconfiguration where the manager IP was left as a
  placeholder (`MANAGER_IP`) by editing `/var/ossec/etc/ossec.conf`
  directly

## Files
- `report.md`: full write-up with evidence
- `screenshots/`: dashboard, alerts, and rule detail evidence

## Skills Practiced
SIEM deployment, Linux troubleshooting, DNS, systemd services,
log correlation, MITRE ATT&CK, Wazuh rule engine, incident analysis

## Related Project
[SSH Brute-Force Detection Lab](https://github.com/prachipt08-10/ssh-bruteforce-detection-lab) — the manual log-analysis version of this same attack, using journalctl, Bash, and fail2ban.
