# Engagement Report

**Task:** Conduct a complete autonomous assessment of all discovered and authorized services in the lab. Perform relevant reconnaissance and vulnerability enumeration, validate evidence-supported vulnerabilities, establish controlled initial access where justified, execute approved privilege-escalation and lateral-movement actions when their evidence gates are satisfied, perform bounded rapid follow-on actions, confirm cleanup, and produce the complete evidence-based report. Continue until every discovered authorized service and each kill-chain phase has been evaluated or explicitly classified as unsupported, unavailable, blocked by policy, or not justified by evidence.
**Target:** `192.168.185.3, 192.168.185.3:8080, 192.168.185.3:5000, 192.168.185.3:6379, 192.168.185.3:22, 192.168.185.3:53, 192.168.185.3:139, 192.168.185.3:445`
**Started (UTC):** 2026-09-21T22:08:13.058994+00:00
**Tools executed:** 17

## Findings summary

- **Shellshock CVE-2014-6271** — `http://192.168.185.3:8080/victim.cgi`
- **Metasploit-confirmed Shellshock exploitability** — `192.168.185.3:8080`
- **Controlled Metasploit Shellshock proof** — `192.168.185.3:8080`

## Services

- {'port': '21/tcp', 'service': 'ftp vsftpd 3.0.5'}
- {'port': '22/tcp', 'service': 'ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0'}
- {'port': '23/tcp', 'service': 'telnet Linux telnetd'}
- {'port': '8080/tcp', 'service': 'http Apache httpd 2.4.10 (Debian)'}
- ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0

## Discovered Targets

- `192.168.185.3:21`
- `192.168.185.3:22`
- `192.168.185.3:23`
- `192.168.185.3:8080`

## Open Ports

- `21/tcp`
- `22/tcp`
- `23/tcp`
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
- **nmap_ssh_enum** on `192.168.185.3:22` — completed
- **nmap** on `192.168.185.3:8080` — completed
- **nmap** on `192.168.185.3:22` — completed
- **curl** on `192.168.185.3:8080` — completed
- **searchsploit** on `192.168.185.3:8080` — completed
- **searchsploit** on `192.168.185.3:22` — completed
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

Host-level port discovery was performed against the authorized host `192.168.185.3`. After discovery, only discovered services that were also in the authorized target list were assessed further.

---

## 1. Executive Summary

The assessment identified a vulnerable Apache CGI service on `192.168.185.3:8080` affected by Shellshock, CVE-2014-6271. The vulnerability was validated with command-execution evidence as the web server user `www-data`, then confirmed using an approved Metasploit check and a controlled exploitation proof. A bounded post-exploitation workflow was performed, including read-only host context collection, privilege-escalation enumeration, and an approved lateral-movement / privilege-escalation proof using backend-held lab credentials that were not exposed.

SSH was also discovered and assessed. It exposed OpenSSH `8.2p1 Ubuntu 4ubuntu0.13`, supported password and public-key authentication, and advertised the legacy `ssh-rsa` host-key algorithm. No exploit candidates were found via SearchSploit for the observed OpenSSH version, and Nmap NSE vulnerability scanning did not identify actionable SSH vulnerabilities.

FTP on port `21` and Telnet on port `23` were discovered but were not in the authorized target list as specific services. They were therefore not assessed further and are classified as blocked by policy.

Redis, DNS, SMB/NetBIOS, and the web service on port `5000` were authorized targets but were not discovered as open services during port discovery; service-specific checks against those ports were therefore not justified.

---

## 2. Reconnaissance Results

### 2.1 Port Discovery

Tool used:

- Bounded Nmap port/service discovery against `192.168.185.3`

Discovered open ports:

| Port | Service | Version / Banner | Authorization Status |
|---:|---|---|---|
| `21/tcp` | FTP | `vsftpd 3.0.5` | Discovered but not authorized as a specific target; not assessed further |
| `22/tcp` | SSH | `OpenSSH 8.2p1 Ubuntu 4ubuntu0.13` | Authorized and assessed |
| `23/tcp` | Telnet | `Linux telnetd` | Discovered but not authorized as a specific target; not assessed further |
| `8080/tcp` | HTTP | `Apache httpd 2.4.10 (Debian)` | Authorized and assessed |

Nmap reported the host as up and identified four open TCP services within the bounded discovery range.

---

## 3. Service Assessment Details

## 3.1 HTTP — `192.168.185.3:8080`

### Service Identification

Observed service:

- Apache HTTP Server `2.4.10`
- Debian default Apache page
- CGI indicators present
- Discovered `/cgi-bin/` path returning HTTP `403`
- Functional vulnerable CGI endpoint later identified as `/victim.cgi`

Evidence:

- HTTP headers:

```text
HTTP/1.1 200 OK
Server: Apache/2.4.10 (Debian)
Content-Type: text/html
```

- Page title:

```text
Apache2 Debian Default Page: It works
```

- WhatWeb fingerprint:

```text
Apache[2.4.10]
HTTPServer[Debian Linux][Apache/2.4.10 (Debian)]
Title[Apache2 Debian Default Page: It works]
```

### Web Content Discovery

Gobuster findings:

| Path | Status | Notes |
|---|---:|---|
| `/index.html` | `200` | Apache default page |
| `/cgi-bin/` | `403` | CGI directory present but listing denied |
| `/server-status` | `403` | Apache status endpoint present but restricted |
| `/.hta` | `403` | Access denied |
| `/.htaccess` | `403` | Access denied |
| `/.htpasswd` | `403` | Access denied |

### Nikto Findings

Nikto identified:

- Missing `X-Frame-Options`
- Missing `X-Content-Type-Options`
- Apache ETag inode leakage indicator
- Apache `2.4.10` appears outdated
- HTTP `OPTIONS` method enabled
- Apache default file `/icons/README` present

Relevant Nikto evidence:

```text
Server: Apache/2.4.10 (Debian)
The anti-clickjacking X-Frame-Options header is not present.
The X-Content-Type-Options header is not set.
Server may leak inodes via ETags.
Apache/2.4.10 appears to be outdated.
OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, OPTIONS.
```

### Nmap NSE Vulnerability Scan

Nmap NSE vulnerability scan against `8080/tcp` completed with no actionable script results and did not flag the service as potentially vulnerable through that scan.

Classification:

- NSE web vulnerability scan: no actionable findings.
- Manual / targeted Shellshock validation was still justified by CGI evidence.

---

## 4. Validated Vulnerability: Shellshock CVE-2014-6271

### Affected Service

- `192.168.185.3:8080`
- Apache CGI endpoint: `http://192.168.185.3:8080/victim.cgi`

### Validation Evidence

A Shellshock validation check was performed only after CGI evidence was observed. The check succeeded and returned command-execution proof as the Apache worker user:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Validation result:

```text
validated_vulnerability: Shellshock CVE-2014-6271
success: true
endpoint: http://192.168.185.3:8080/victim.cgi
```

### Metasploit Check

The approved Metasploit module was then used in `check` mode:

- Module: `exploit/multi/http/apache_mod_cgi_bash_env_exec`
- Target: `192.168.185.3:8080`
- Target URI: `/victim.cgi`

Evidence:

```text
[+] 192.168.185.3:8080 - The target is vulnerable.
```

Result:

- `check_vulnerable: true`

### Controlled Exploitation Proof

After validation and Metasploit check evidence, a controlled exploit run was performed using the approved payload:

- Module: `exploit/multi/http/apache_mod_cgi_bash_env_exec`
- Payload: `linux/x86/shell/reverse_tcp`
- Proof command: `id`

Tool evidence indicated:

- Session opened: yes
- Proof command output detected: yes
- Session cleanup attempted: yes
- Session closed: yes

This confirmed controlled initial access via Shellshock.

---

## 5. SSH — `192.168.185.3:22`

### Service Identification

Observed service:

```text
OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0
```

### SSH Enumeration

Authentication methods for the probed lab username `webuser`:

```text
publickey
password
```

Host-key fingerprints:

| Key Type | Bits | Fingerprint |
|---|---:|---|
| RSA | 3072 | `91289a6a5eb3bfcde5363ee813ab66ba` |
| ECDSA | 256 | `1afe856797e9470df76c343c354a278d` |
| ED25519 | 256 | `53fa03b615448cfd4be1762eef12a7c3` |

Advertised algorithms included modern algorithms and also the legacy host-key algorithm:

```text
ssh-rsa
```

### SSH Vulnerability Enumeration

Nmap NSE vulnerability scan against SSH completed without actionable findings.

SearchSploit query:

```text
OpenSSH 8.2p1 Ubuntu
```

Result:

```text
Exploits: No Results
Shellcodes: No Results
```

### SSH Assessment Conclusion

Findings:

- SSH is exposed and supports password authentication.
- Legacy `ssh-rsa` is advertised.
- No direct exploit candidate was identified from the observed OpenSSH version using available tools.
- No credential guessing, brute force, or unauthorized login attempts were performed.

Risk classification:

- Low to Medium, depending on policy requirements.
- The legacy algorithm exposure should be remediated as hardening.
- Password authentication should be reviewed and disabled if not required.

---

## 6. Services Discovered but Not Assessed Due to Authorization Limits

The following services were discovered during host-level reconnaissance but were not included in the authorized target list as specific service targets:

| Port | Service | Reason Not Assessed |
|---:|---|---|
| `21/tcp` | `vsftpd 3.0.5` | Blocked by policy: discovered but not authorized as `192.168.185.3:21` |
| `23/tcp` | `Linux telnetd` | Blocked by policy: discovered but not authorized as `192.168.185.3:23` |

No FTP or Telnet vulnerability validation, login testing, or exploitation was performed.

---

## 7. Authorized Services Not Discovered Open

The following services were in the authorized target list but were not discovered as open during bounded port discovery. Service-specific testing was therefore not justified by reconnaissance evidence.

| Authorized Target | Assessment Classification |
|---|---|
| `192.168.185.3:53` | Unavailable / not discovered open |
| `192.168.185.3:139` | Unavailable / not discovered open |
| `192.168.185.3:445` | Unavailable / not discovered open |
| `192.168.185.3:5000` | Unavailable / not discovered open |
| `192.168.185.3:6379` | Unavailable / not discovered open |

Because Redis was not discovered open, Redis checks and Redis canary write validation were not run.

Because SMB/NetBIOS was not discovered open, enum4linux was not run.

Because DNS was not discovered open, DNS enumeration was not run.

Because an application on port `5000` was not discovered open, HTTP probing and IDOR testing for that port were not run.

---

## 8. Post-Exploitation, Privilege Escalation, and Lateral Movement

These actions were performed only after controlled Shellshock foothold proof was established.

### 8.1 Read-Only Loot / Context Collection

Approved read-only host context collection was performed through the controlled Shellshock foothold.

Collected artifact categories:

- User identity
- Hostname
- Kernel / OS information
- Network interface information

Tool evidence indicated:

- Collection completed
- Identity collected
- Hostname collected
- OS information collected
- Network information collected
- Session opened
- Session cleanup attempted
- Session closed

The direct Shellshock validation also proved command execution as:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### 8.2 Privilege-Escalation Enumeration

Read-only privilege-escalation enumeration was performed.

Checks included:

- Current identity
- Hostname
- Kernel information
- `sudo -n -l`
- SUID file discovery
- Writable directory discovery
- `/etc/passwd` collection

Tool evidence indicated:

- Enumeration completed
- Identity collected
- Kernel information collected
- Sudo checked
- SUID findings present
- Writable paths checked
- `/etc/passwd` collected
- Session cleanup attempted
- Session closed

No uncontrolled local privilege-escalation exploit was performed against the Shellshock host.

### 8.3 Approved Lateral-Movement and Privilege-Escalation Proof

The approved bounded lateral-movement and privilege-escalation proof was performed only after the Shellshock foothold was validated.

Important controls:

- The credential was backend-held.
- The credential was not revealed, printed, recovered, or requested.
- The action was limited to the lab-defined proof path.
- Sessions were closed afterward.

Evidence summary:

```text
source_host: vm1-shellshock
source_external_target: 192.168.185.3:8080
destination_host: vm2-redis
destination_internal_address: 192.168.60.6
destination_user: webuser
lateral_authentication_succeeded: true
initial_user: webuser
initial_uid: 1000
privilege_escalation_succeeded: true
effective_user: root
effective_uid: 0
proof_command: sudo /usr/bin/find /tmp -maxdepth 0 -exec /usr/bin/id \;
credential_exposed: false
session_cleanup_attempted: true
session_closed: true
```

This demonstrates that compromise of the Shellshock-exposed host can support controlled lateral movement to an internal host and privilege escalation to root via the lab-defined sudo `find` path.

---

## 9. Cleanup Status

Cleanup-related evidence from controlled activities:

| Activity | Cleanup Evidence |
|---|---|
| Controlled Metasploit Shellshock proof | `session_cleanup_attempted: true`, `session_closed: true` |
| Loot collection | `session_cleanup_attempted: true`, `session_closed: true` |
| Privilege-escalation enumeration | `session_cleanup_attempted: true`, `session_closed: true` |
| Lateral movement / privilege escalation proof | `session_cleanup_attempted: true`, `session_closed: true`, credential not exposed |

No persistence was attempted.

No destructive action was performed.

No external downloads were performed.

No credential recovery or disclosure was attempted.

Metasploit command stagers created temporary randomized files under `/tmp` during controlled exploitation. The available tool evidence confirms session closure and cleanup attempts, but does not provide independent confirmation that every temporary file artifact was removed from the target filesystem. This is noted as cleanup partially confirmed by tool evidence and otherwise unavailable to verify with the provided toolset.

---

## 10. Kill-Chain Phase Classification

| Phase | Status | Evidence / Rationale |
|---|---|---|
| Reconnaissance | Completed | Port discovery found `21`, `22`, `23`, `8080`; HTTP, SSH, web content, and vulnerability enumeration performed where authorized |
| Initial Access | Completed for HTTP `8080` | Shellshock validated; Metasploit check confirmed; controlled exploitation proof succeeded |
| Privilege Escalation on initial host | Enumerated only | Read-only enumeration performed; no uncontrolled local privilege escalation executed |
| Lateral Movement | Completed through approved bounded proof | Backend-held credential used without exposure; lateral auth to internal VM2 succeeded |
| Privilege Escalation after lateral movement | Completed through approved bounded proof | Fixed sudo-find proof produced effective root context |
| Rapid Follow-On Actions | Completed within bounds | Host context collection, privilege-enumeration, and lateral/privesc proof performed |
| Cleanup | Partially confirmed | Sessions closed and cleanup attempted; no persistence; temp file cleanup not independently verifiable with available evidence |
| Reporting | Completed | Evidence-based report produced |

---

## 11. Findings

### Finding 1 — Critical: Shellshock Remote Command Execution on Apache CGI

Affected target:

- `192.168.185.3:8080`

Vulnerability:

- Shellshock, CVE-2014-6271

Evidence:

```text
endpoint: http://192.168.185.3:8080/victim.cgi
proof: uid=33(www-data) gid=33(www-data) groups=33(www-data)
success: true
```

Metasploit check evidence:

```text
[+] 192.168.185.3:8080 - The target is vulnerable.
```

Impact:

- Remote unauthenticated command execution as `www-data`
- Controlled foothold established
- Enabled approved lateral movement and downstream privilege escalation in the lab

Recommendation:

- Patch Bash and all CGI-exposed execution environments.
- Remove or restrict vulnerable CGI scripts.
- Disable CGI if not required.
- Upgrade Apache and underlying OS packages.
- Place CGI services behind strict authentication and input filtering where applicable.
- Review logs for exploitation indicators.

---

### Finding 2 — Medium: Outdated Apache HTTP Server

Affected target:

- `192.168.185.3:8080`

Evidence:

```text
Apache/2.4.10 (Debian)
Apache/2.4.10 appears to be outdated
```

Impact:

- Increased exposure to known vulnerabilities and hardening gaps.
- The observed Apache environment also hosted vulnerable CGI functionality.

Recommendation:

- Upgrade Apache to a supported patched release.
- Apply Debian security updates.
- Remove default content.
- Review enabled modules and disable unnecessary modules.

---

### Finding 3 — Low/Medium: Missing Web Security Headers

Affected target:

- `192.168.185.3:8080`

Evidence:

```text
X-Frame-Options header is not present
X-Content-Type-Options header is not set
```

Impact:

- Missing `X-Frame-Options` can increase clickjacking risk.
- Missing `X-Content-Type-Options` can increase MIME-sniffing risk.

Recommendation:

Add security headers, for example:

```text
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Content-Security-Policy: default-src 'self'
```

---

### Finding 4 — Low: Apache Default Files and Restricted Administrative Paths Present

Affected target:

- `192.168.185.3:8080`

Evidence:

```text
/index.html        200
/cgi-bin/          403
/server-status     403
/icons/README      Apache default file found
```

Impact:

- Default files disclose server information.
- Restricted paths confirm potentially sensitive functionality exists.

Recommendation:

- Remove default Apache files.
- Ensure `/server-status` is disabled or restricted to trusted administrative networks.
- Ensure `/cgi-bin/` is disabled unless required.

---

### Finding 5 — Low/Medium: SSH Password Authentication Enabled and Legacy Algorithm Advertised

Affected target:

- `192.168.185.3:22`

Evidence:

Authentication methods:

```text
publickey
password
```

Legacy algorithm:

```text
ssh-rsa
```

Impact:

- Password authentication increases exposure to password attacks if network controls fail.
- `ssh-rsa` is deprecated in many security baselines.

Recommendation:

- Disable password authentication if possible.
- Enforce key-based authentication.
- Disable `ssh-rsa` where compatibility allows.
- Use modern host-key and signature algorithms.
- Continue patching OpenSSH through the OS vendor.

---

## 12. Explicitly Unsupported / Blocked / Not Justified Actions

| Action | Classification | Reason |
|---|---|---|
| FTP assessment on `21/tcp` | Blocked by policy | Port discovered but `192.168.185.3:21` not in authorized target list |
| Telnet assessment on `23/tcp` | Blocked by policy | Port discovered but `192.168.185.3:23` not in authorized target list |
| Redis validation / canary write | Not justified by evidence | Redis port `6379` was authorized but not discovered open |
| DNS enumeration | Not justified by evidence | DNS port `53` was authorized but not discovered open |
| SMB enumeration | Not justified by evidence | SMB/NetBIOS ports `139/445` authorized but not discovered open |
| IDOR testing | Not justified by evidence | No object-style endpoints such as `/profile/<id>` were discovered |
| Shellshock testing before CGI evidence | Not performed | Shellshock validation was only performed after CGI indicators were observed |
| Credential brute force | Blocked by policy / not performed | No credential guessing or password attacks were authorized or needed |
| Persistence | Blocked by policy / not performed | No persistence attempted |
| Destructive actions | Blocked by policy / not performed | No destructive operations performed |
| External downloads | Blocked by policy / not performed | No external downloads performed |

---

## 13. Overall Risk Assessment

Overall risk: **Critical**

Primary reason:

- The Apache CGI service on `8080/tcp` is vulnerable to Shellshock and allows unauthenticated remote command execution as `www-data`.

Compounding factors:

- The service runs an outdated Apache version.
- CGI functionality is exposed.
- A controlled foothold enabled bounded lateral movement and privilege escalation to root on an internal lab host.
- SSH allows password authentication and advertises a deprecated algorithm.

---

## 14. Prioritized Remediation Plan

1. **Immediately patch or disable vulnerable CGI/Shellshock exposure**
   - Patch Bash.
   - Remove `/victim.cgi` or any vulnerable CGI handlers.
   - Disable CGI if not required.

2. **Upgrade Apache and OS packages**
   - Bring Apache and Debian packages to supported versions.
   - Remove default pages and files.

3. **Harden the web service**
   - Add security headers.
   - Disable unnecessary HTTP methods if not required.
   - Restrict or disable `/server-status`.
   - Audit all CGI scripts.

4. **Harden SSH**
   - Disable password authentication where possible.
   - Remove `ssh-rsa` from accepted algorithms where compatibility allows.
   - Enforce key-based authentication and MFA if supported.

5. **Investigate lateral movement paths**
   - Review internal credential use.
   - Restrict SSH trust paths.
   - Review sudo permissions, especially commands such as `find` that can be abused for root execution.
   - Apply least privilege to internal hosts.

6. **Monitoring and detection**
   - Review Apache logs for suspicious User-Agent, Referer, or header payloads containing Bash function patterns.
   - Monitor unexpected outbound connections from web servers.
   - Alert on CGI command-execution patterns.

---

## 15. Final Assessment Conclusion

The assessment completed all evidence-supported and authorized phases. The most significant issue is a validated Shellshock vulnerability on the Apache CGI service at `192.168.185.3:8080`, resulting in unauthenticated remote command execution as `www-data`. Controlled exploitation was proven, post-exploitation enumeration was performed, and an approved bounded lateral-movement and privilege-escalation proof succeeded without exposing credentials.

SSH was enumerated and hardened recommendations were identified, but no direct SSH exploit was supported by evidence. FTP and Telnet were discovered but not assessed because their specific ports were outside the authorized target list. Redis, DNS, SMB, and port `5000` were authorized but not discovered open, so service-specific checks were not justified.