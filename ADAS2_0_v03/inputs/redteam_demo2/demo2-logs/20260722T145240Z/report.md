# Engagement Report

**Task:** Conduct a complete autonomous assessment of all discovered and authorized services in the lab. Perform relevant reconnaissance and vulnerability enumeration, validate evidence-supported vulnerabilities, establish controlled initial access where justified, execute approved privilege-escalation and lateral-movement actions when their evidence gates are satisfied, perform bounded rapid follow-on actions, confirm cleanup, and produce the complete evidence-based report. Continue until every discovered authorized service and each kill-chain phase has been evaluated or explicitly classified as unsupported, unavailable, blocked by policy, or not justified by evidence.
**Target:** `192.168.185.3, 192.168.185.3:8080, 192.168.185.3:5000, 192.168.185.3:6379, 192.168.185.3:22, 192.168.185.3:53, 192.168.185.3:139, 192.168.185.3:445`
**Started (UTC):** 2026-07-22T14:52:40.853832+00:00
**Tools executed:** 26

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

- Legacy SSH algorithms advertised
- Outdated Apache version

## Evidence collected

- **nmap** on `192.168.185.3` — completed
- **curl** on `192.168.185.3:5000` — completed
- **curl** on `192.168.185.3:8080` — completed
- **whatweb** on `192.168.185.3:5000` — completed
- **whatweb** on `192.168.185.3:8080` — completed
- **redis-cli** on `192.168.185.3:6379` — completed
- **nmap_ssh_enum** on `192.168.185.3:22` — completed
- **nikto** on `192.168.185.3:8080` — completed
- **nikto** on `192.168.185.3:5000` — error
- **gobuster** on `192.168.185.3:8080` — completed
- **gobuster** on `192.168.185.3:5000` — completed
- **curl** on `192.168.185.3:5000` — completed
- **redis_canary** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:8080` — completed
- **curl** on `192.168.185.3:8080` — completed
- **nmap** on `192.168.185.3:22` — completed
- **nmap** on `192.168.185.3:6379` — completed
- **nmap** on `192.168.185.3:5000` — completed
- **nmap** on `192.168.185.3:8080` — completed
- **run_metasploit** on `192.168.185.3:8080` — blocked
- **metasploit** on `192.168.185.3:8080` — completed
- **metasploit** on `192.168.185.3:8080` — completed
- **loot** on `192.168.185.3:8080` — completed
- **linpeas** on `192.168.185.3:8080` — completed
- **lateral_privesc_proof** on `192.168.185.3:8080` — completed

## Controlled Exploitation Proof

- **Target:** `192.168.185.3:8080`
- **Module:** `exploit/multi/http/apache_mod_cgi_bash_env_exec`
- **Payload:** `linux/x86/shell/reverse_tcp`
- **Proof command:** `id`
- **Session opened:** `True`
- **Session closed:** `True`


## Analyst narrative (model)

## Autonomous Lab Assessment Report

### Scope and Authorization

Authorized assessment targets included:

- `192.168.185.3`
- `192.168.185.3:8080`
- `192.168.185.3:5000`
- `192.168.185.3:6379`
- `192.168.185.3:22`
- `192.168.185.3:53`
- `192.168.185.3:139`
- `192.168.185.3:445`

Host-level port discovery was performed first. Only discovered services that were also explicitly authorized by port were assessed further.

---

## 1. Reconnaissance Summary

### Port Discovery

Nmap service discovery against `192.168.185.3` identified the host as up and found the following open TCP ports:

| Port | Service | Version / Banner |
|---:|---|---|
| 21/tcp | FTP | vsftpd 3.0.5 |
| 22/tcp | SSH | OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 |
| 23/tcp | Telnet | Linux telnetd |
| 5000/tcp | HTTP-like service | Werkzeug/3.1.8 Python/3.9.25 |
| 6379/tcp | Redis | Redis 4.0.9 |
| 8080/tcp | HTTP | Apache httpd 2.4.10 Debian |

### Authorization Handling

| Discovered Port | Assessed? | Reason |
|---:|---|---|
| 21/tcp FTP | No | Discovered but not explicitly authorized as `192.168.185.3:21`; classified as blocked by policy. |
| 22/tcp SSH | Yes | Explicitly authorized. |
| 23/tcp Telnet | No | Discovered but not explicitly authorized as `192.168.185.3:23`; classified as blocked by policy. |
| 5000/tcp HTTP/Werkzeug | Yes | Explicitly authorized. |
| 6379/tcp Redis | Yes | Explicitly authorized. |
| 8080/tcp HTTP/Apache | Yes | Explicitly authorized. |
| 53/tcp DNS | No | Authorized but not discovered open during port discovery; service unavailable/not exposed. |
| 139/tcp SMB/NetBIOS | No | Authorized but not discovered open; service unavailable/not exposed. |
| 445/tcp SMB | No | Authorized but not discovered open; service unavailable/not exposed. |

---

## 2. Service Assessment Details

## 2.1 SSH — `192.168.185.3:22`

### Evidence

Service/version:

- OpenSSH `8.2p1 Ubuntu 4ubuntu0.13`
- Protocol 2.0

SSH enumeration identified:

- Authentication methods:
  - `publickey`
  - `password`
- Host keys:
  - RSA 3072-bit
  - ECDSA 256-bit
  - ED25519 256-bit
- Algorithms included modern options, but also advertised legacy `ssh-rsa`.

### Findings

#### Finding: Legacy SSH Algorithm Advertised

- **Status:** Candidate / configuration weakness
- **Evidence:** SSH server host key algorithms included `ssh-rsa`.
- **Impact:** `ssh-rsa` relies on SHA-1 signatures and is deprecated in many modern SSH clients. Its presence may increase downgrade or compatibility risk depending on client configuration.
- **Validation:** No direct exploitability was demonstrated. Nmap NSE vulnerability scan against port 22 did not report a confirmed vulnerability.
- **Classification:** Evidence-supported hardening issue, not a validated exploit path.

### Initial Access Assessment

- No credential guessing, brute force, or password submission was performed.
- SSH initial access was **not justified by evidence**.

---

## 2.2 HTTP/Werkzeug Internal User Portal — `192.168.185.3:5000`

### Evidence

HTTP probe:

```text
HTTP/1.1 200 OK
Server: Werkzeug/3.1.8 Python/3.9.25
```

Landing page preview:

```html
<h1>Internal User Portal</h1>
<p>Your profile: <a href='/profile/2'>View my profile</a></p>
<p>Try: /profile/1, /profile/2, /profile/3</p>
```

WhatWeb confirmed:

- Werkzeug `3.1.8`
- Python `3.9.25`

Gobuster found:

| Path | Status |
|---|---:|
| `/console` | 400 |

Nikto reported:

- Missing `X-Frame-Options`
- Missing `X-Content-Type-Options`
- HTTP `OPTIONS` enabled
- Possible `/ #wp-config.php#` finding, but this appears unreliable in this context and was not further validated as a confirmed credential disclosure.

### Validated Vulnerability: Insecure Direct Object Reference

#### Evidence

The application exposed object-style endpoints:

```text
/profile/1
/profile/2
/profile/3
```

The current profile link pointed to `/profile/2`, but changing the numeric ID allowed retrieval of other user records without an authorization-context change.

Accessible profiles:

| Endpoint | Result |
|---|---|
| `/profile/1` | Returned Alice/admin record, including a secret value |
| `/profile/2` | Returned Bob/user record |
| `/profile/3` | Returned Charlie/user record |

Tool evidence showed:

```text
accessible_profile_count: 3
accessible_profiles: [1, 2, 3]
authorization_context_changed: false
unauthorized_access_possible: true
validated_vulnerability: Insecure Direct Object Reference
```

#### Impact

An unauthenticated or improperly authorized user can enumerate and access other users’ profile data by modifying the profile ID. One returned record disclosed administrative data and a lab secret.

#### Severity

High.

#### Recommendation

- Enforce object-level authorization checks server-side.
- Do not rely on client-visible IDs as access controls.
- Ensure users can only access profile objects they are authorized to view.
- Consider using indirect, non-sequential identifiers, but only as defense-in-depth; authorization checks are still required.
- Add regression tests for IDOR/BOLA-style access control failures.

---

## 2.3 Redis — `192.168.185.3:6379`

### Evidence

Redis check:

```text
redis_version:4.0.9
redis_mode:standalone
os:Linux 5.15.0-139-generic x86_64
tcp_port:6379
```

The Redis server disclosed `INFO server` data without authentication.

Tool evidence:

```text
redis_reachable: true
auth_required: false
unauthenticated_access: true
redis_version_disclosed: true
```

### Validated Vulnerability: Unauthenticated Redis Access

#### Evidence

A controlled Redis canary write/read/delete test was performed after unauthenticated access was confirmed.

Results:

```text
precondition_met=True
write_succeeded=True
readback_matches=True
cleanup_confirmed=True
```

Tool evidence:

```text
unauthenticated_write_access: true
write_succeeded: true
readback_matches: true
cleanup_attempted: true
cleanup_confirmed: true
```

#### Impact

An unauthenticated remote client can read server information and write arbitrary keys to Redis. Depending on Redis configuration, filesystem permissions, and deployment context, this can lead to data exposure, data tampering, denial of service, or in some configurations, remote code execution or persistence through unsafe Redis features.

#### Severity

Critical.

#### Recommendation

- Bind Redis to localhost or a private management interface only.
- Enable Redis authentication/ACLs.
- Restrict network access with firewall rules/security groups.
- Disable or restrict dangerous commands where appropriate.
- Upgrade Redis, as version `4.0.9` is outdated.
- Monitor for unauthorized keys/configuration changes.
- Do not expose Redis directly to untrusted networks.

### Cleanup

The temporary Redis canary key was deleted and cleanup was confirmed.

---

## 2.4 HTTP/Apache CGI — `192.168.185.3:8080`

### Evidence

HTTP probe:

```text
HTTP/1.1 200 OK
Server: Apache/2.4.10 (Debian)
```

Page title:

```text
Apache2 Debian Default Page: It works
```

WhatWeb confirmed:

- Apache `2.4.10`
- Debian Linux
- Default Apache page

Gobuster findings:

| Path | Status |
|---|---:|
| `/.htpasswd` | 403 |
| `/.htaccess` | 403 |
| `/.hta` | 403 |
| `/cgi-bin/` | 403 |
| `/index.html` | 200 |
| `/server-status` | 403 |

Nikto findings:

- Apache `2.4.10` appears outdated.
- Missing `X-Frame-Options`
- Missing `X-Content-Type-Options`
- ETag inode leakage indication
- HTTP `OPTIONS` enabled
- `/icons/README` Apache default file found

### Validated Vulnerability: Shellshock CVE-2014-6271

Shellshock validation was performed only after CGI evidence was identified.

#### Evidence

The Shellshock validation check against:

```text
http://192.168.185.3:8080/victim.cgi
```

returned command execution proof:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Tool evidence:

```text
success: true
validated_vulnerability: Shellshock CVE-2014-6271
endpoint: http://192.168.185.3:8080/victim.cgi
```

Metasploit check also confirmed exploitability:

```text
[+] 192.168.185.3:8080 - The target is vulnerable.
```

Tool evidence:

```text
check_vulnerable: true
vulnerable_indicator: true
```

### Controlled Initial Access

After Shellshock was validated and the Metasploit check confirmed exploitability, a controlled exploit was run using the approved module:

```text
exploit/multi/http/apache_mod_cgi_bash_env_exec
```

Approved payload:

```text
linux/x86/shell/reverse_tcp
```

Evidence:

```text
session_opened: true
proof_command: id
proof_output_detected: true
session_cleanup_attempted: true
session_closed: true
```

The foothold executed as:

```text
www-data
```

### Impact

The Apache CGI endpoint is vulnerable to Shellshock and allows unauthenticated remote command execution as the web server user. This provides an attacker with a foothold on the host and enables post-exploitation enumeration and potential lateral movement.

#### Severity

Critical.

#### Recommendation

- Patch Bash and any CGI execution paths affected by Shellshock.
- Remove unused CGI scripts and disable CGI where not required.
- Upgrade Apache and the underlying Debian stack.
- Restrict CGI execution to trusted, maintained scripts.
- Add WAF or request filtering for Shellshock patterns as compensating control.
- Review logs for suspicious User-Agent or header-based command injection attempts.
- Rotate credentials and secrets accessible to the web server user.

---

## 3. Post-Exploitation, Privilege Escalation, and Lateral Movement

### 3.1 Loot / Host Context Collection

After controlled foothold proof, bounded read-only host context collection was performed.

Collected artifact classes:

- User identity
- Hostname
- Kernel / OS information
- Network information

Tool evidence indicated:

```text
collection_completed: true
identity_collected: true
hostname_collected: true
os_info_collected: true
network_info_collected: true
session_closed: true
```

### 3.2 Privilege Escalation Enumeration

Read-only privilege escalation enumeration was performed after the controlled Shellshock foothold.

Collected checks included:

- `id`
- `hostname`
- `uname -a`
- `sudo -n -l`
- SUID binary enumeration
- Writable directory checks
- `/etc/passwd` collection

Tool evidence:

```text
enumeration_completed: true
identity_collected: true
kernel_info_collected: true
sudo_checked: true
suid_findings: true
writable_paths_checked: true
passwd_collected: true
session_closed: true
```

No direct local root escalation on `192.168.185.3` was executed outside the approved bounded workflow.

### 3.3 Lateral Movement and Privilege Escalation Proof

Because the Shellshock foothold was validated, the approved bounded lateral movement and privilege escalation proof was executed.

Important constraint honored:

- The backend-held credential was used by the tool only.
- The credential was not requested, printed, recovered, or exposed.

Evidence:

```text
source_external_target: 192.168.185.3:8080
source_host: vm1-shellshock
destination_host: vm2-redis
destination_internal_address: 192.168.60.6
destination_user: webuser
lateral_authentication_succeeded: true
initial_user: webuser
initial_uid: 1000
sudo_path_identified: true
privilege_escalation_succeeded: true
effective_user: root
effective_uid: 0
credential_exposed: false
session_closed: true
```

Privilege escalation proof command:

```text
sudo /usr/bin/find /tmp -maxdepth 0 -exec /usr/bin/id \;
```

### Impact

The initial Shellshock compromise enabled controlled lateral movement to an internal host and privilege escalation to root via a sudo misconfiguration involving `find`.

#### Severity

Critical.

#### Recommendation

- Remove unnecessary sudo privileges for `find`.
- Avoid granting users NOPASSWD sudo access to binaries with command execution features.
- Audit sudoers configuration against GTFOBins-style escalation paths.
- Segment internal hosts so web-tier compromise does not allow SSH access to backend systems.
- Rotate credentials potentially reachable from the compromised host.
- Monitor SSH authentication paths between lab/internal systems.

---

## 4. Additional Findings

### Outdated Apache Version

- **Target:** `192.168.185.3:8080`
- **Evidence:** Apache `2.4.10 (Debian)`
- **Status:** Candidate / confirmed outdated software, but no version-specific CVE was separately validated beyond Shellshock CGI exploitation.
- **Recommendation:** Upgrade Apache and Debian packages.

### Missing Web Security Headers

Observed on both web services:

- `X-Frame-Options` missing
- `X-Content-Type-Options` missing

#### Impact

These increase exposure to clickjacking and MIME-sniffing risks.

#### Recommendation

Add appropriate headers, for example:

```text
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Content-Security-Policy: default-src 'self'
```

### Default / Restricted Apache Paths

Observed on `192.168.185.3:8080`:

- Apache default page
- `/icons/README`
- `/server-status` restricted with 403
- `/cgi-bin/` restricted with 403 but vulnerable CGI script was reachable at `/victim.cgi`

#### Recommendation

- Remove default files.
- Ensure `server-status` remains restricted.
- Inventory CGI scripts and remove or patch vulnerable ones.

---

## 5. Kill Chain Phase Evaluation

| Phase | Status | Evidence / Classification |
|---|---|---|
| Reconnaissance | Completed | Port discovery, HTTP probing, WhatWeb, SSH enum, Redis check, Gobuster, Nikto. |
| Vulnerability Enumeration | Completed for authorized discovered services | SSH, Redis, HTTP 5000, HTTP 8080 assessed. FTP/Telnet blocked by policy. DNS/SMB unavailable/not open. |
| Initial Access | Completed where justified | Shellshock on Apache CGI validated and exploited in controlled manner as `www-data`. |
| Redis Exploitation Proof | Completed | Unauthenticated write/read/delete canary succeeded; cleanup confirmed. |
| Web App Access Control Validation | Completed | IDOR confirmed on `/profile/<id>`. |
| Privilege Escalation Enumeration | Completed | Read-only enumeration performed after foothold. |
| Local Privilege Escalation on source host | Not directly exploited | No approved direct local root exploit was executed on `192.168.185.3`; classified as not justified/limited to enumeration. |
| Lateral Movement | Completed under approved bounded workflow | Lateral SSH to internal VM2 as `webuser` succeeded using backend-held credential; credential not exposed. |
| Privilege Escalation on lateral host | Completed under approved bounded workflow | Sudo `find` proof executed; effective user root. |
| Rapid Follow-On Actions | Completed | Loot/context collection, linpeas-style enumeration, Redis canary validation, lateral/privesc proof. |
| Cleanup | Partially completed / partially unavailable | Redis canary cleanup confirmed; Metasploit sessions closed. Explicit removal of temporary Metasploit stager files was not available through the provided tool interface and is classified as unavailable in the environment/tooling. |
| Reporting | Completed | Evidence-based report produced. |

---

## 6. Confirmed Vulnerabilities

| Severity | Vulnerability | Target | Evidence |
|---|---|---|---|
| Critical | Shellshock CVE-2014-6271 Remote Command Execution | `192.168.185.3:8080/victim.cgi` | Returned `uid=33(www-data)`; Metasploit check confirmed target vulnerable; controlled session opened. |
| Critical | Unauthenticated Redis Access with Write Capability | `192.168.185.3:6379` | `INFO server` accessible without auth; canary `SET/GET/DEL` succeeded; cleanup confirmed. |
| Critical | Lateral Movement + Privilege Escalation Path | From Shellshock foothold to internal VM2 | Backend-held credential enabled SSH as `webuser`; sudo `find` proof returned root identity; credential not exposed. |
| High | Insecure Direct Object Reference | `192.168.185.3:5000/profile/<id>` | Multiple distinct user profiles accessible by changing numeric ID without authorization-context change. |
| Medium | Legacy SSH Algorithm Advertised | `192.168.185.3:22` | `ssh-rsa` advertised. |
| Medium | Outdated Apache / Default Apache Exposure | `192.168.185.3:8080` | Apache `2.4.10`; default files and default page exposed. |
| Low / Medium | Missing Web Security Headers | `192.168.185.3:5000`, `192.168.185.3:8080` | Missing `X-Frame-Options` and `X-Content-Type-Options`. |

---

## 7. Cleanup Summary

| Item | Cleanup Status |
|---|---|
| Redis canary key | Confirmed deleted. |
| Metasploit sessions | Closed by tooling; session cleanup attempted and confirmed closed. |
| Lateral movement session | Closed by tooling. |
| Backend credential | Not exposed. |
| Temporary Metasploit stager files | Explicit file deletion was not available through the provided tool interface; classify as unavailable in environment/tooling. |

---

## 8. Highest-Priority Remediation Plan

1. **Patch Shellshock immediately**
   - Update Bash.
   - Remove or fix vulnerable CGI scripts.
   - Disable CGI if not required.

2. **Restrict Redis**
   - Do not expose Redis unauthenticated.
   - Bind to localhost/private interface.
   - Enable ACLs/authentication.
   - Firewall port `6379`.

3. **Fix the IDOR**
   - Add server-side authorization checks on `/profile/<id>`.
   - Ensure users can only access their own profile or authorized records.

4. **Correct sudo misconfiguration on internal host**
   - Remove unsafe sudo permissions for `find`.
   - Audit sudoers for GTFOBins-capable binaries.

5. **Improve segmentation**
   - Prevent web-tier hosts from directly SSHing to sensitive backend hosts unless required.
   - Limit east-west movement paths.

6. **Upgrade outdated services**
   - Upgrade Apache/Debian packages.
   - Upgrade Redis.
   - Review OpenSSH algorithm policy.

7. **Harden web headers and defaults**
   - Add security headers.
   - Remove Apache default content.
   - Review exposed management/status endpoints.

---

## Final Assessment Conclusion

The lab host exposes multiple high-impact vulnerabilities across authorized services. The most severe issue is the vulnerable Apache CGI endpoint on `192.168.185.3:8080`, which allowed validated Shellshock command execution and a controlled foothold as `www-data`. From that foothold, the approved bounded workflow demonstrated successful lateral movement to an internal host and privilege escalation to root using a sudo `find` misconfiguration.

Separately, Redis on `192.168.185.3:6379` allows unauthenticated access and confirmed unauthenticated write capability. The Werkzeug application on `192.168.185.3:5000` contains a confirmed IDOR vulnerability exposing other users’ profile data, including administrative data.

All discovered and authorized services were evaluated. Discovered but unauthorized services were not assessed further, and authorized but undiscovered services were classified as unavailable/not exposed.