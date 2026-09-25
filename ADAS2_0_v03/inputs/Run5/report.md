# Engagement Report

**Task:** Conduct a complete autonomous assessment of all discovered and authorized services in the lab. Perform relevant reconnaissance and vulnerability enumeration, validate evidence-supported vulnerabilities, establish controlled initial access where justified, execute approved privilege-escalation and lateral-movement actions when their evidence gates are satisfied, perform bounded rapid follow-on actions, confirm cleanup, and produce the complete evidence-based report. Continue until every discovered authorized service and each kill-chain phase has been evaluated or explicitly classified as unsupported, unavailable, blocked by policy, or not justified by evidence.
**Target:** `192.168.185.3, 192.168.185.3:8080, 192.168.185.3:5000, 192.168.185.3:6379, 192.168.185.3:22, 192.168.185.3:53, 192.168.185.3:139, 192.168.185.3:445`
**Started (UTC):** 2026-09-21T01:23:31.145304+00:00
**Tools executed:** 29

## Findings summary

- **Unauthenticated Redis access** — `192.168.185.3:6379`
- **Insecure Direct Object Reference** — `/profile/<user_id>`
- **Shellshock CVE-2014-6271** — `http://192.168.185.3:8080/victim.cgi`
- **Metasploit-confirmed Shellshock exploitability** — `192.168.185.3:8080`
- **Controlled Metasploit Shellshock proof** — `192.168.185.3:8080`

## Services

- {'port': '21/tcp', 'service': 'ftp vsftpd 3.0.5'}
- {'port': '22/tcp', 'service': 'ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0'}
- {'port': '23/tcp', 'service': 'telnet Linux telnetd'}
- {'port': '5000/tcp', 'service': 'upnp'}
- {'port': '6379/tcp', 'service': 'redis Redis key-value store 4.0.9'}
- {'port': '8080/tcp', 'service': 'http Apache httpd 2.4.10 (Debian)'}
- ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0

## Discovered Targets

- `192.168.185.3:21`
- `192.168.185.3:22`
- `192.168.185.3:23`
- `192.168.185.3:5000`
- `192.168.185.3:6379`
- `192.168.185.3:8080`

## Open Ports

- `21/tcp`
- `22/tcp`
- `23/tcp`
- `5000/tcp`
- `6379/tcp`
- `8080/tcp`

## Technologies

- Apache
- CGI
- OpenSSH

## Candidate vulnerabilities

- Outdated Apache version
- Legacy SSH algorithms advertised

## Evidence collected

- **nmap** on `192.168.185.3` — completed
- **curl** on `192.168.185.3:8080` — completed
- **whatweb** on `192.168.185.3:8080` — completed
- **nikto** on `192.168.185.3:8080` — completed
- **gobuster** on `192.168.185.3:8080` — completed
- **curl** on `192.168.185.3:5000` — completed
- **whatweb** on `192.168.185.3:5000` — completed
- **nikto** on `192.168.185.3:5000` — error
- **gobuster** on `192.168.185.3:5000` — completed
- **redis-cli** on `192.168.185.3:6379` — completed
- **nmap_ssh_enum** on `192.168.185.3:22` — completed
- **curl** on `192.168.185.3:5000` — completed
- **curl** on `192.168.185.3:8080` — completed
- **redis_canary** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:8080` — completed
- **searchsploit** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:5000` — completed
- **searchsploit** on `192.168.185.3:22` — completed
- **nmap** on `192.168.185.3:8080` — completed
- **nmap** on `192.168.185.3:6379` — completed
- **nmap** on `192.168.185.3:5000` — completed
- **nmap** on `192.168.185.3:22` — completed
- **run_metasploit** on `192.168.185.3:8080` — blocked
- **metasploit** on `192.168.185.3:8080` — completed
- **metasploit** on `192.168.185.3:8080` — completed
- **loot** on `192.168.185.3:8080` — completed
- **linpeas** on `192.168.185.3:8080` — completed
- **lateral_privesc_proof** on `192.168.185.3:8080` — completed
- **run_linux_command** on `local` — blocked

## Controlled Exploitation Proof

- **Target:** `192.168.185.3:8080`
- **Module:** `exploit/multi/http/apache_mod_cgi_bash_env_exec`
- **Payload:** `linux/x86/shell/reverse_tcp`
- **Proof command:** `id`
- **Session opened:** `True`
- **Session closed:** `True`


## Analyst narrative (model)

## Autonomous Lab Assessment Report

### 1. Scope and Authorization

**Authorized target host:** `192.168.185.3`  
**Authorized service targets:** `192.168.185.3`, `:8080`, `:5000`, `:6379`, `:22`, `:53`, `:139`, `:445`

Host-level discovery was authorized and performed. After discovery, only services that were both discovered and explicitly authorized by port were assessed further.

### 2. Executive Summary

The assessment identified multiple exploitable weaknesses on the authorized services:

| Finding | Severity | Status |
|---|---:|---|
| Shellshock CVE-2014-6271 via Apache CGI on `8080` | Critical | Validated and controlled access proven |
| Insecure Direct Object Reference on Werkzeug app `5000` | High | Validated |
| Unauthenticated Redis access on `6379` | High | Validated, unauthenticated write proven, cleanup confirmed |
| Successful lateral movement and privilege escalation proof from Shellshock foothold | Critical | Validated using approved bounded workflow |
| Outdated Apache `2.4.10` and web hardening issues | Medium | Enumerated |
| SSH legacy `ssh-rsa` algorithm advertised | Low/Medium | Enumerated |
| FTP `21` and Telnet `23` discovered | Not assessed | Blocked by policy; ports not in authorized target list |

Controlled exploitation was performed only after evidence-supported validation. No persistence or destructive actions were attempted. Redis canary cleanup was confirmed. Metasploit sessions were terminated. Temporary exploit artifacts created by the exploit module were not independently confirmed removed because the available tools did not provide a safe, explicit artifact cleanup mechanism.

---

## 3. Reconnaissance Results

### 3.1 Port and Service Discovery

Nmap service discovery against `192.168.185.3` found:

| Port | Service | Assessment Status |
|---:|---|---|
| `21/tcp` | `ftp vsftpd 3.0.5` | Discovered, not assessed; not in authorized port list |
| `22/tcp` | `OpenSSH 8.2p1 Ubuntu 4ubuntu0.13` | Assessed |
| `23/tcp` | `Linux telnetd` | Discovered, not assessed; not in authorized port list |
| `5000/tcp` | HTTP service, Werkzeug/Python | Assessed |
| `6379/tcp` | Redis `4.0.9` | Assessed |
| `8080/tcp` | Apache HTTPD `2.4.10 Debian` | Assessed |

Ports `53`, `139`, and `445` were authorized but were not discovered as open during port discovery; no further service-specific assessment was justified.

---

## 4. Service Assessment Details

### 4.1 HTTP Apache Service — `192.168.185.3:8080`

#### Observed Evidence

- HTTP server: `Apache/2.4.10 (Debian)`
- Landing page: Apache2 Debian default page
- Discovered paths:
  - `/index.html` — `200 OK`
  - `/cgi-bin/` — `403 Forbidden`
  - `/server-status` — `403 Forbidden`
  - `/.hta`, `/.htaccess`, `/.htpasswd` — `403 Forbidden`
- Nikto findings:
  - Missing `X-Frame-Options`
  - Missing `X-Content-Type-Options`
  - ETag inode leakage indicator
  - Apache version appears outdated
  - HTTP methods allowed: `GET`, `HEAD`, `POST`, `OPTIONS`
  - `/icons/README` Apache default file present
- WhatWeb confirmed Apache `2.4.10`.

#### Shellshock Validation

A Shellshock validation check was performed only after web/CGI evidence was identified.

Validated endpoint:

```text
http://192.168.185.3:8080/victim.cgi
```

Proof returned:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

This validates **Shellshock CVE-2014-6271** command execution as the Apache web server user `www-data`.

#### Metasploit Check and Controlled Exploitation

Metasploit module:

```text
exploit/multi/http/apache_mod_cgi_bash_env_exec
```

Check result:

```text
[+] 192.168.185.3:8080 - The target is vulnerable.
```

Controlled exploit proof:

- Payload: `linux/x86/shell/reverse_tcp`
- Session opened successfully.
- Proof command: `id`
- Proof output detected.
- Session cleanup attempted and session closed.

#### Risk

**Critical.** Remote unauthenticated command execution was validated and controlled initial access was proven.

#### Recommendation

- Remove or disable vulnerable CGI scripts.
- Patch Bash and CGI execution environment.
- Restrict CGI usage where not required.
- Upgrade Apache and underlying OS packages.
- Disable unnecessary default content such as `/icons/README`.
- Add defensive headers:
  - `X-Frame-Options`
  - `X-Content-Type-Options`
  - Content Security Policy where appropriate
- Limit HTTP methods to only those required.

---

### 4.2 Werkzeug/Python Web Application — `192.168.185.3:5000`

#### Observed Evidence

HTTP probe identified:

```text
Server: Werkzeug/3.1.8 Python/3.9.25
```

Landing page:

```html
<h1>Internal User Portal</h1>
<p>Your profile: <a href='/profile/2'>View my profile</a></p>
<p>Try: /profile/1, /profile/2, /profile/3</p>
```

Discovered path:

```text
/console — HTTP 400
```

Nikto partial findings:

- Missing `X-Frame-Options`
- Missing `X-Content-Type-Options`
- HTTP methods allowed: `HEAD`, `OPTIONS`, `GET`
- Nikto scan timed out before full completion.

#### IDOR Validation

The application exposed object-style endpoints:

```text
/profile/<user_id>
```

The IDOR check requested multiple profile IDs without changing authorization context.

Accessible records:

| Profile ID | Result |
|---:|---|
| `1` | Accessible |
| `2` | Accessible |
| `3` | Accessible |

Evidence showed multiple distinct user records accessible by changing the numeric ID. Profile `1` exposed an admin record and a sensitive secret value.

The secret itself is not repeated in full here; the evidence showed an exposed lab flag-style secret associated with:

```text
user=alice, role=admin
```

#### Validated Vulnerability

**Insecure Direct Object Reference / Broken Object Level Authorization**

#### Risk

**High.** Any unauthenticated or improperly authorized user can enumerate and access other users’ profile data, including sensitive admin-associated data.

#### Recommendation

- Enforce authorization checks server-side for every `/profile/<id>` request.
- Do not trust user-supplied object IDs.
- Use session identity to derive the accessible profile.
- Return `403 Forbidden` or `404 Not Found` for unauthorized profile IDs.
- Remove instructional hints such as `Try: /profile/1, /profile/2, /profile/3`.
- Disable Werkzeug debug console in production-like environments.
- Add web security headers.

---

### 4.3 Redis — `192.168.185.3:6379`

#### Observed Evidence

Redis check:

```text
redis_version:4.0.9
redis_mode:standalone
os:Linux 5.15.0-139-generic x86_64
tcp_port:6379
config_file:/etc/redis/redis.conf
```

The Redis server allowed `INFO server` without authentication.

#### Validated Vulnerability

**Unauthenticated Redis access**

Evidence:

```text
auth_required: false
unauthenticated_access: true
redis_version_disclosed: true
```

#### Controlled Write Proof

A bounded Redis canary test was performed after confirming unauthenticated access.

Actions:

1. Reconfirmed unauthenticated Redis access.
2. Wrote a randomized temporary key with 60-second expiration.
3. Read the key back.
4. Deleted the key.
5. Confirmed cleanup.

Evidence:

```text
precondition_met=True
write_succeeded=True
readback_matches=True
cleanup_confirmed=True
```

#### Risk

**High.** Unauthenticated Redis access allows data disclosure and unauthorized modification. Depending on Redis configuration and file permissions, such exposure can sometimes lead to further compromise.

#### Recommendation

- Bind Redis to localhost or a restricted management network.
- Require authentication using `requirepass` or Redis ACLs.
- Firewall port `6379` from untrusted networks.
- Upgrade Redis to a supported version.
- Disable dangerous commands where appropriate using `rename-command`.
- Monitor Redis for unauthorized key creation or configuration changes.

---

### 4.4 SSH — `192.168.185.3:22`

#### Observed Evidence

Service:

```text
OpenSSH 8.2p1 Ubuntu 4ubuntu0.13
```

Authentication methods discovered for lab username probe:

```text
publickey
password
```

Host key fingerprints:

| Type | Fingerprint |
|---|---|
| RSA | `91:28:9a:6a:5e:b3:bf:cd:e5:36:3e:e8:13:ab:66:ba` |
| ECDSA | `1a:fe:85:67:97:e9:47:0d:f7:6c:34:3c:35:4a:27:8d` |
| ED25519 | `53:fa:03:b6:15:44:8c:fd:4b:e1:76:2e:ef:12:a7:c3` |

Algorithms included modern options, but also advertised legacy:

```text
ssh-rsa
```

SearchSploit query for `OpenSSH 8.2p1 Ubuntu` returned no results. Nmap NSE vulnerability scan returned no script findings.

#### Risk

**Low/Medium.** No direct exploitability was validated. However, legacy `ssh-rsa` support should be removed where possible.

#### Recommendation

- Disable `ssh-rsa` if legacy compatibility is not required.
- Prefer ED25519 or ECDSA host keys.
- Disable password authentication if public key authentication is feasible.
- Enforce MFA or equivalent compensating controls.
- Keep OpenSSH and Ubuntu packages updated.

---

### 4.5 FTP — `192.168.185.3:21`

Discovered service:

```text
vsftpd 3.0.5
```

Assessment status:

```text
Blocked by policy
```

Reason: `192.168.185.3:21` was discovered during authorized host-level discovery but was not included in the authorized target list for service-specific assessment.

No further enumeration or validation was performed.

---

### 4.6 Telnet — `192.168.185.3:23`

Discovered service:

```text
Linux telnetd
```

Assessment status:

```text
Blocked by policy
```

Reason: `192.168.185.3:23` was discovered during authorized host-level discovery but was not included in the authorized target list for service-specific assessment.

No further enumeration or validation was performed.

---

## 5. Initial Access

### 5.1 Supported Initial Access Path

Initial access was justified and performed through the validated Shellshock vulnerability on:

```text
192.168.185.3:8080/victim.cgi
```

Proof:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

A controlled Metasploit exploit established a temporary shell session. The session was used for proof and bounded follow-on actions only.

### 5.2 Unsupported or Not Justified Initial Access Paths

| Service | Status |
|---|---|
| SSH `22` | Not justified; no credentials, no validated exploit |
| Redis `6379` | Write capability proven, but no controlled shell path pursued |
| Werkzeug app `5000` | IDOR validated, but no RCE evidence found |
| FTP `21` | Blocked by policy |
| Telnet `23` | Blocked by policy |

---

## 6. Post-Exploitation and Loot Collection

After controlled Shellshock foothold validation, approved read-only evidence collection was performed.

Collected categories:

- User identity
- Hostname
- Kernel and OS information
- Network context
- `/etc/os-release`
- Network interfaces

The foothold identity was confirmed as:

```text
www-data
```

No destructive actions or persistence mechanisms were used.

---

## 7. Privilege Escalation Evaluation

### 7.1 Local Privilege Escalation on Shellshock Source Host

Read-only privilege escalation enumeration was performed using the approved linpeas-style workflow.

Actions included:

- `id`
- `hostname`
- `uname -a`
- `sudo -n -l`
- SUID binary discovery
- Writable directory checks
- `/etc/passwd` collection

Result:

```text
Enumeration completed.
```

No explicit local root compromise of the Shellshock source host was proven by the available evidence.

Classification:

```text
Local privilege escalation on source host: not proven / no validated path in collected evidence
```

### 7.2 Bounded Lateral Movement and Privilege Escalation Proof

Because a controlled Shellshock foothold was validated, the approved lateral movement and privilege escalation proof was executed.

Evidence:

- Source host: `vm1-shellshock`
- Destination host: `vm2-redis`
- Destination internal address: `192.168.60.6`
- Destination user: `webuser`
- Initial user: `webuser`
- Initial UID: `1000`
- Privilege escalation command:

```text
sudo /usr/bin/find /tmp -maxdepth 0 -exec /usr/bin/id \;
```

Result:

```text
effective_user: root
effective_uid: 0
lateral_authentication_succeeded: true
privilege_escalation_succeeded: true
credential_exposed: false
```

The backend-held lab credential was not requested, recovered, printed, or exposed.

Risk:

**Critical.** A web-facing Shellshock compromise can be used as a pivot to an internal host where privilege escalation to root is possible.

Recommendation:

- Eliminate the Shellshock foothold.
- Segment internal hosts from compromised web tiers.
- Restrict outbound SSH from web servers.
- Review sudoers configuration on the internal host.
- Remove or constrain dangerous sudo permissions involving `find`.
- Require strong authentication and monitoring for lateral movement paths.

---

## 8. Rapid Follow-On Actions

The following bounded follow-on actions were completed after evidence gates were satisfied:

| Action | Status |
|---|---|
| SearchSploit lookups for Apache/Shellshock, Redis, Werkzeug, OpenSSH | Completed; no exploit candidates returned |
| Nmap NSE vulnerability scans on authorized discovered services | Completed; no additional script findings |
| Redis write canary | Completed; cleanup confirmed |
| Shellshock Metasploit check | Completed; vulnerable |
| Controlled Shellshock exploit | Completed; session opened and closed |
| Loot collection | Completed |
| Privilege escalation enumeration | Completed |
| Lateral movement and privilege escalation proof | Completed |
| Local listener inspection | Blocked by policy due target outside authorized scope |

---

## 9. Cleanup Status

### Confirmed Cleanup

| Item | Status |
|---|---|
| Redis canary key | Deleted and cleanup confirmed |
| Metasploit sessions | Session cleanup attempted and sessions closed |
| Lateral movement sessions | Closed by approved workflow |
| Credentials | Backend-held credential not exposed |

### Not Independently Confirmed

The Metasploit exploit module generated temporary command-stager paths under `/tmp`, such as randomized names shown in tool output. The sessions were closed, but independent removal of temporary stager files was not confirmed because the available approved tools did not provide a safe explicit cleanup operation for those artifacts.

No persistence was attempted.

---

## 10. Kill-Chain Phase Classification

| Phase | Result |
|---|---|
| Reconnaissance | Completed for authorized host and discovered authorized services |
| Service enumeration | Completed for `22`, `5000`, `6379`, `8080` |
| FTP/Telnet assessment | Blocked by policy; discovered but not authorized by port |
| DNS/SMB assessment | Not justified; authorized ports not discovered open |
| Vulnerability validation | Completed where evidence justified |
| Initial access | Completed via Shellshock on `8080` |
| Loot collection | Completed after controlled foothold |
| Local privilege escalation on source host | Enumeration completed; root not proven |
| Lateral movement | Completed using approved bounded workflow |
| Privilege escalation on lateral host | Completed; root proof validated |
| Rapid follow-on | Completed within available controls |
| Cleanup | Redis and sessions confirmed; exploit temp file cleanup not independently confirmed |
| Reporting | Completed |

---

## 11. Prioritized Remediation Plan

### Immediate

1. Patch Bash and remove/disable vulnerable CGI endpoint `/victim.cgi`.
2. Restrict or disable CGI execution on Apache.
3. Lock down Redis:
   - Require authentication.
   - Bind to localhost or trusted interfaces only.
   - Firewall `6379`.
4. Fix IDOR in `/profile/<id>` by enforcing object-level authorization.
5. Review internal SSH access paths from the web host.
6. Remove dangerous sudo permissions enabling root escalation through `find`.

### Short Term

1. Upgrade Apache from `2.4.10`.
2. Remove default Apache files and pages.
3. Disable unnecessary HTTP methods.
4. Add security headers:
   - `X-Frame-Options`
   - `X-Content-Type-Options`
   - CSP where feasible
5. Disable Werkzeug console/debug functionality.
6. Disable legacy SSH `ssh-rsa`.

### Longer Term

1. Implement network segmentation between public-facing web services and internal hosts.
2. Monitor for Redis unauthenticated access attempts.
3. Add centralized logging for Apache CGI execution and SSH lateral movement.
4. Enforce least privilege in sudoers.
5. Regularly perform authenticated configuration reviews of exposed services.

---

## 12. Final Assessment Conclusion

The lab host exposes several significant risks. The most critical issue is a validated Shellshock vulnerability on the Apache CGI service at `192.168.185.3:8080`, which allowed controlled remote command execution as `www-data`. That foothold enabled a bounded, approved lateral movement and privilege escalation proof to an internal host, demonstrating a high-impact attack chain.

Redis on `6379` is unauthenticated and allows unauthorized writes, confirmed with a temporary canary and successful cleanup. The Werkzeug application on `5000` contains a validated IDOR vulnerability exposing multiple user records, including sensitive admin-associated data.

SSH was enumerated and did not present a validated exploit path, though legacy `ssh-rsa` support should be removed. FTP and Telnet were discovered but not assessed because they were outside the explicitly authorized service target list.