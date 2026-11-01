## Investigation Report

**Date: October 30, 2026**
#
## Investigation Information

| Field | Details |
|---------|---------|
| Incident Name | Multiple-Source Failed Logons and Suspicious Administrator Authentication |
| Severity | High |
| Status | Escalated |
| Analyst | Mohammed Rashad Abdallah |

## Executive Summary

On 2026-10-29 04:11:10 UTC, a failed logon attempt was observed on device mtsdc.mts.local, targeting the administrator account. The same host recorded subsequent failed logon attempts against multiple usernames, including administrator, adm, admin, root, user, cristina, and Nicole. The attempts originated from multiple source IP addresses, with some IPs appearing repeatedly. This pattern is consistent with potential distributed password guessing or brute-force activity.

On 2026-10-29 06:12:28 UTC, the same host recorded a successful network logon using the administrator account and NTLM authentication. The event identified the remote device as ip172313486 and the source IP address as 185[.]225[.]69[.]222, geolocated to Budapest, Hungary.

According to the reported VirusTotal results, 6 out of 92 security vendors flagged 185[.]225[.]69[.]222 as malicious. The IP was also reported as associated with VPN, Tor, or hosting infrastructure. This warrants further investigation into the legitimacy of the successful authentication.

The successful logon IP was observed only during the successful authentication event and was not identified among the source IPs associated with the preceding failed logons. Therefore, the available evidence does not establish a direct relationship between the failed authentication attempts and the successful logon.

Domain-wide searches did not identify malicious activity involving the administrator account after the successful logon, or other malicious activity within the domain before and after the successful logon during the investigation. However, the absence of further observed activity does not rule out unauthorized access.

## Scope of Investigation

**Time Frame:**
- Start Time: 2026-10-29 04:11:10 UTC
- End Time: 2026-10-29 06:12:28 UTC

**Affected Assets:**

**Hostname:**
- mtsdc

**User Accounts:**
- administrator

## The 5 Ws and How

**Who?**

- Targeted account: administrator.
- Other usernames targeted: adm, admin, root, user, cristina, and Nicole.
- Successful logon account: administrator.
- Successful logon source: 185[.]225[.]69[.]222, associated with remote device ip172313486.

**What?**

- Multiple failed logon attempts against several usernames, originating from different IP addresses, with some IPs recurring.
- A subsequent successful network logon using the administrator account through NTLM from a source IP not observed in the preceding failed logon events.

**When?**

- Initial observed failed logon: 2026-10-29 at 04:11:10 UTC.
- Successful administrator logon: 2026-10-29 at 06:12:28 UTC.
- Elapsed time between these recorded events: approximately 2 hours, 1 minute, and 18 seconds.

**Where?**

- Target device: mtsdc.mts.local.
- Failed authentication attempts: Multiple source IP addresses, with some recurring.
- Successful authentication: 185[.]225[.]69[.]222.
- Reported geolocation: Budapest, Hungary.
- Reported IP reputation: 5 of 92 VirusTotal security vendors flagged the successful logon IP as malicious.

**Why?**

The repeated failed logons against multiple usernames may indicate password guessing or attempts to identify valid accounts. The successful administrator logon from a separately observed, suspiciously rated IP raises a separate concern about possible unauthorized access.

The available evidence does not establish whether the two activity patterns were connected or whether the successful logon was authorized.

**How?**

The failed logon activity involved multiple source IP addresses, some of which appeared repeatedly. The subsequent successful authentication used NTLM over a network logon from 185[.]225[.]69[.]222, which was not observed among the preceding failed logon sources.

This may represent separate authentication activity. Further correlation is required to determine whether the events are related.

## Incident Timeline

**2026-10-29 04:11:10 UTC**

A failed logon attempt was observed on device mtsdc.mts.local, targeting the administrator account.

**2026-10-29 06:12:28 UTC**

A successful network logon was recorded on the same host using the administrator account and NTLM authentication. The event identified the remote device as ip172313486 and the source IP address as 185[.]225[.]69[.]222, geolocated to Budapest, Hungary.

Between the first observed failed logon and the successful logon were subsequent failed logon attempts against multiple usernames, including administrator, adm, admin, root, user, cristina, and Nicole, on the same host. The attempts originated from multiple source IP addresses, with some IPs appearing repeatedly.

## Findings

- Multiple-source failed logon activity: Failed authentication attempts targeted several usernames and originated from multiple IP addresses, with some addresses recurring. This is consistent with potential distributed password guessing or brute-force activity.

- Successful administrator network logon: A successful NTLM network logon occurred on 2026-10-29 06:12:28 UTC using the administrator account.

- Suspicious successful logon source: The successful logon originated from 185[.]225[.]69[.]222. The reported VirusTotal results showed 5 of 92 security vendors flagging the IP, and the address was reported as associated with VPN, Tor, or hosting infrastructure.

- No direct source IP correlation: The successful logon IP was observed only during the successful event and was not identified among the preceding failed logon sources. A direct connection between the failed attempts and successful authentication has not been established.

- No additional malicious activity identified: Domain-wide searches did not identify malicious activity involving the administrator account after the successful logon or other confirmed malicious activity during the investigation.

- Unauthorized access remains unconfirmed: The successful administrator authentication warrants validation, but the available evidence does not establish account compromise or subsequent attacker activity.

## Threat Intelligence Findings

- According to AbuseIPDB, the IP (185[.]225[.]69[.]222) has been reported 328 times, has a confidence level of 100% abuse, and has been categorized with brute-force, port-scan, and web app attack-related activity at the time of the investigation. 

- VirusTotal reported that 6 out of 92 security vendors flagged it as malicious at the time of the investigation.

- IPinfo also reported the IP as associated with VPN, Tor, and hosting infrastructure.

## MITRE ATT&CK Mapping

| Time (UTC) | Technique ID | Technique Name |
|---|---|---|
| 20261029 06:12:28 UTC | ID: T1110.003 | Brute Force: Password Spraying (Potential) |

## Recommendations

- Review the failed logon events to identify recurring source IPs, targeted accounts, attempt frequency, and whether the activity is consistent with distributed password guessing.

- Validate the successful administrator logon with the system owner and determine whether the source IP and remote device are associated with approved access.
Further examine endpoint telemetry for suspicious process execution, PowerShell activity, scheduled task creation, service installation, account changes, and lateral movement.

- If the successful logon is confirmed as unauthorized, reset the affected account's credentials, enforce account lockout or password-attempt limits, and revoke active sessions where supported.

- Assess whether the failed logon source IPs require blocking or other controls, and investigate the successful logon IP separately.

- Continue monitoring for repeated authentication failures, successful logons from suspicious sources, and additional activity involving privileged accounts.

## Conclusion

The investigation identified two authentication patterns requiring attention: repeated failed logon attempts against multiple usernames from several source IP addresses, and a successful NTLM network logon using the administrator account from a separately observed, suspiciously rated IP address.

Because the successful logon IP was not observed among the preceding failed logon sources, the available evidence does not establish that the successful authentication resulted from the observed password-guessing activity. Domain-wide searches identified no additional confirmed malicious activity, but the legitimacy of the successful administrator logon remains unverified.

**Incident assessment:** Suspicious authentication activity involving multiple-source failed logons and a separate successful administrator logon.

**Recommended disposition:** Keep the incident under investigation until the successful logon is validated and the failed authentication activity is assessed.

## Screenshot

<img width="1847" height="832" alt="Initial observe" src="https://github.com/user-attachments/assets/53073074-bfab-43da-84d9-c1f7e494d69a" />

#

<img width="1847" height="245" alt="successfull logon" src="https://github.com/user-attachments/assets/0fcd6ff3-d19d-4298-ae71-a522be20b645" />

#

<img width="1816" height="797" alt="AbuseIPDB" src="https://github.com/user-attachments/assets/0e12a369-9011-4ffd-8eda-760985b2e3cf" />

#

<img width="1916" height="875" alt="VirusTotal" src="https://github.com/user-attachments/assets/3cbd2f27-6bc7-4fdc-adea-50d80851c48e" />

#

<img width="1917" height="846" alt="IPinfo" src="https://github.com/user-attachments/assets/4f20d08f-11e6-4a3b-a0b7-d51425debb00" />





