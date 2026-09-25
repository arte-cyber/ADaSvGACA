# Engagement Report

**Task:** Conduct a complete autonomous assessment of all discovered and authorized services in the lab. Perform relevant reconnaissance and vulnerability enumeration, validate evidence-supported vulnerabilities, establish controlled initial access where justified, execute approved privilege-escalation and lateral-movement actions when their evidence gates are satisfied, perform bounded rapid follow-on actions, confirm cleanup, and produce the complete evidence-based report. Continue until every discovered authorized service and each kill-chain phase has been evaluated or explicitly classified as unsupported, unavailable, blocked by policy, or not justified by evidence.
**Target:** `192.168.185.3, 192.168.185.3:8080, 192.168.185.3:5000, 192.168.185.3:6379, 192.168.185.3:22, 192.168.185.3:53, 192.168.185.3:139, 192.168.185.3:445`
**Started (UTC):** 2026-09-21T23:31:15.751422+00:00
**Tools executed:** 21

## Findings summary

- **Unauthenticated Redis access** — `192.168.185.3:6379`
- **Shellshock CVE-2014-6271** — `http://192.168.185.3:8080/victim.cgi`
- **Metasploit-confirmed Shellshock exploitability** — `192.168.185.3:8080`
- **Controlled Metasploit Shellshock proof** — `192.168.185.3:8080`

## Services

- {'port': '21/tcp', 'service': 'ftp vsftpd 3.0.5'}
- {'port': '22/tcp', 'service': 'ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0'}
- {'port': '23/tcp', 'service': 'telnet Linux telnetd'}
- {'port': '6379/tcp', 'service': 'redis Redis key-value store 4.0.9'}
- {'port': '8080/tcp', 'service': 'http Apache httpd 2.4.10 (Debian)'}
- ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 Ubuntu Linux; protocol 2.0

## Discovered Targets

- `192.168.185.3:21`
- `192.168.185.3:22`
- `192.168.185.3:23`
- `192.168.185.3:6379`
- `192.168.185.3:8080`

## Open Ports

- `21/tcp`
- `22/tcp`
- `23/tcp`
- `6379/tcp`
- `8080/tcp`

## Technologies

- Apache
- CGI
- OpenSSH

## Candidate vulnerabilities

- Outdated Apache version
- Legacy SSH algorithms advertised
- Public exploit candidates found

## Evidence collected

- **nmap** on `192.168.185.3` — completed
- **curl** on `192.168.185.3:8080` — completed
- **whatweb** on `192.168.185.3:8080` — completed
- **nikto** on `192.168.185.3:8080` — completed
- **gobuster** on `192.168.185.3:8080` — completed
- **redis-cli** on `192.168.185.3:6379` — completed
- **nmap_ssh_enum** on `192.168.185.3:22` — completed
- **searchsploit** on `192.168.185.3:8080` — completed
- **searchsploit** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:22` — completed
- **curl** on `192.168.185.3:8080` — completed
- **redis_canary** on `192.168.185.3:6379` — completed
- **nmap** on `192.168.185.3:8080` — completed
- **nmap** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:8080` — completed
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

Authorized targets included:

- `192.168.185.3`
- `192.168.185.3:8080`
- `192.168.185.3:5000`
- `192.168.185.3:6379`
- `192.168.185.3:22`
- `192.168.185.3:53`
- `192.168.185.3:139`
- `192.168.185.3:445`

Port discovery was performed against the authorized host `192.168.185.3`. After discovery, only services both discovered and explicitly authorized by port were assessed further.

---

## Executive Summary

The assessment identified two significant validated security issues on authorized services:

1. **Apache CGI Shellshock Remote Command Execution**
   - Target: `192.168.185.3:8080`
   - Vulnerability: Shellshock, CVE-2014-6271
   - Impact: Remote command execution as `www-data`
   - Confirmed through:
     - CGI evidence
     - Shellshock validation returning `uid=33(www-data)`
     - Metasploit `check` confirming exploitability
     - Controlled Metasploit exploitation with session proof

2. **Unauthenticated Redis Access with Write Capability**
   - Target: `192.168.185.3:6379`
   - Redis version: `4.0.9`
   - Impact: Unauthenticated users can read server information and write keys
   - Confirmed through:
     - `INFO server` without authentication
     - Controlled temporary canary write/read/delete test
     - Cleanup confirmed

Controlled post-exploitation was performed only after the Shellshock foothold was validated. Read-only loot and privilege-escalation enumeration were conducted. A bounded lateral-movement and privilege-escalation proof was also completed using a backend-held lab credential; the credential was not exposed.

---

## Discovered Services

Port discovery found the following open ports on `192.168.185.3`:

| Port | Service | Scope Status |
|---:|---|---|
| `21/tcp` | `ftp vsftpd 3.0.5` | Discovered but not in authorized target list; not assessed |
| `22/tcp` | `OpenSSH 8.2p1 Ubuntu 4ubuntu0.13` | Authorized and assessed |
| `23/tcp` | `Linux telnetd` | Discovered but not in authorized target list; not assessed |
| `6379/tcp` | `Redis key-value store 4.0.9` | Authorized and assessed |
| `8080/tcp` | `Apache httpd 2.4.10 Debian` | Authorized and assessed |

Authorized but not discovered/listening during port discovery:

| Port | Status |
|---:|---|
| `5000/tcp` | Not discovered as open; unavailable |
| `53/tcp` | Not discovered as open; unavailable |
| `139/tcp` | Not discovered as open; unavailable |
| `445/tcp` | Not discovered as open; unavailable |

---

# Detailed Findings

---

## Finding 1: Shellshock Remote Command Execution via Apache CGI

### Severity

**Critical**

### Affected Target

- `192.168.185.3:8080`

### Evidence

HTTP probing confirmed Apache:

```text
Server: Apache/2.4.10 (Debian)
Title: Apache2 Debian Default Page: It works
```

Directory enumeration discovered a CGI path:

```text
/cgi-bin/             Status: 403
```

The HTTP probe also identified CGI-related evidence, and a specific vulnerable CGI endpoint was later validated:

```text
http://192.168.185.3:8080/victim.cgi
```

Shellshock validation succeeded and returned command execution as the web server user:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Metasploit check also confirmed exploitability:

```text
[+] 192.168.185.3:8080 - The target is vulnerable.
```

Controlled exploitation was performed after validation using the approved module:

```text
exploit/multi/http/apache_mod_cgi_bash_env_exec
payload: linux/x86/shell/reverse_tcp
```

The controlled proof opened a session, executed `id`, and then closed sessions:

```text
proof_command: id
proof_output_detected: true
session_opened: true
session_cleanup_attempted: true
session_closed: true
```

### Impact

An unauthenticated attacker can execute arbitrary commands through the vulnerable CGI handler. In this lab, command execution was achieved as:

```text
www-data
```

This provides an initial foothold on the host and enables further internal enumeration and lateral movement.

### Root Cause

The Apache CGI environment exposes a Bash interpreter vulnerable to Shellshock, allowing crafted HTTP headers or environment variables to trigger command execution.

### Recommendation

- Patch Bash to a version not vulnerable to CVE-2014-6271 and related Shellshock issues.
- Remove or disable vulnerable CGI scripts.
- Disable CGI execution unless explicitly required.
- Restrict access to `/cgi-bin/`.
- Upgrade Apache; version `2.4.10` is outdated.
- Add monitoring for suspicious CGI requests containing Bash function payloads such as `() { :;};`.

---

## Finding 2: Unauthenticated Redis Access with Write Capability

### Severity

**High**

### Affected Target

- `192.168.185.3:6379`

### Evidence

Redis service was discovered:

```text
6379/tcp open redis Redis key-value store 4.0.9
```

Unauthenticated access to Redis server information succeeded:

```text
redis_version:4.0.9
redis_mode:standalone
os:Linux 5.15.0-139-generic x86_64
tcp_port:6379
config_file:/etc/redis/redis.conf
```

The Redis check confirmed:

```text
redis_reachable: true
auth_required: false
unauthenticated_access: true
redis_version_disclosed: true
```

A controlled canary write test was then performed. The tool:

1. Reconfirmed unauthenticated access.
2. Wrote a randomized temporary key with a 60-second expiration.
3. Read the key back.
4. Deleted the key.
5. Confirmed cleanup.

Result:

```text
precondition_met=True
write_succeeded=True
readback_matches=True
cleanup_confirmed=True
```

Evidence summary:

```text
unauthenticated_write_access: true
write_succeeded: true
readback_matches: true
cleanup_confirmed: true
```

### Impact

An unauthenticated network user can interact with Redis and write arbitrary keys. Depending on Redis configuration, deployment context, filesystem permissions, and network exposure, this can lead to:

- Data exposure
- Data tampering
- Service disruption
- Potential remote code execution in some misconfigured Redis deployments

In this assessment, exploitation was intentionally limited to a temporary canary key and cleanup was confirmed.

### Recommendation

- Require Redis authentication using a strong password or ACLs.
- Bind Redis to localhost or trusted internal interfaces only.
- Restrict access with host firewall rules.
- Disable dangerous commands or rename administrative commands where appropriate.
- Upgrade Redis; version `4.0.9` is outdated.
- Monitor Redis logs for unauthenticated access attempts.

---

## Finding 3: Outdated Apache Version and Web Hardening Issues

### Severity

**Medium**

### Affected Target

- `192.168.185.3:8080`

### Evidence

Nikto identified Apache as outdated:

```text
Apache/2.4.10 appears to be outdated
```

Additional web hardening issues were identified:

```text
The anti-clickjacking X-Frame-Options header is not present.
The X-Content-Type-Options header is not set.
Server may leak inodes via ETags.
OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, OPTIONS.
Apache default file found: /icons/README
```

Discovered web paths:

```text
/.htpasswd      403
/.hta           403
/.htaccess      403
/cgi-bin/       403
/index.html     200
/server-status  403
```

### Impact

These issues increase the attack surface and may aid attackers during reconnaissance. The outdated Apache instance is especially concerning because the same web service also exposes a CGI Shellshock issue.

### Recommendation

- Upgrade Apache to a supported version.
- Remove default files such as `/icons/README`.
- Disable or restrict unnecessary HTTP methods.
- Set security headers:
  - `X-Frame-Options`
  - `X-Content-Type-Options`
  - `Content-Security-Policy`
  - `Referrer-Policy`
- Review `ETag` behavior and disable inode leakage if applicable.
- Restrict access to sensitive paths and status pages.

---

## Finding 4: SSH Allows Legacy `ssh-rsa` Host Key Algorithm

### Severity

**Low**

### Affected Target

- `192.168.185.3:22`

### Evidence

SSH enumeration found:

```text
OpenSSH 8.2p1 Ubuntu 4ubuntu0.13
```

Supported authentication methods:

```text
publickey
password
```

Host keys:

```text
RSA:     91:28:9a:6a:5e:b3:bf:cd:e5:36:3e:e8:13:ab:66:ba
ECDSA:   1a:fe:85:67:97:e9:47:0d:f7:6c:34:3c:35:4a:27:8d
ED25519: 53:fa:03:b6:15:44:8c:fd:4b:e1:76:2e:ef:12:a7:c3
```

Legacy algorithm detected:

```text
ssh-rsa
```

### Impact

`ssh-rsa` relies on SHA-1 signatures in legacy contexts and is generally discouraged. Its availability may increase cryptographic downgrade risk depending on client/server negotiation behavior.

No credential guessing, password submission, or SSH exploitation was performed.

### Recommendation

- Disable legacy `ssh-rsa` where operationally feasible.
- Prefer modern host key algorithms such as `ssh-ed25519` or RSA SHA-2 variants.
- Disable password authentication if not required.
- Enforce key-based authentication and MFA where applicable.
- Keep OpenSSH updated.

---

# Controlled Initial Access

Initial access was justified by validated Shellshock evidence.

### Target

- `192.168.185.3:8080`

### Method

Approved Metasploit module:

```text
exploit/multi/http/apache_mod_cgi_bash_env_exec
```

Approved payload:

```text
linux/x86/shell/reverse_tcp
```

### Proof

The controlled exploitation opened a session and executed a bounded identity proof:

```text
id
```

The proof confirmed command execution as the web service user.

### Cleanup

The Metasploit-controlled session was closed:

```text
session_cleanup_attempted: true
session_closed: true
```

No persistence was attempted.

---

# Post-Exploitation and Loot Collection

Read-only host identity and context collection was performed after controlled foothold proof.

Collected artifact categories included:

- Identity
- Hostname
- Kernel information
- OS release information
- Network information

Evidence flags:

```text
collection_completed: true
identity_collected: true
hostname_collected: true
os_info_collected: true
network_info_collected: true
session_opened: true
session_closed: true
```

This phase was limited to read-only collection for reporting.

---

# Privilege-Escalation Enumeration

Read-only privilege-escalation enumeration was performed after Shellshock foothold validation.

The enumeration checked:

- Current identity
- Hostname
- Kernel information
- Non-interactive sudo privileges
- SUID binaries
- Writable directories
- `/etc/passwd`

Evidence flags:

```text
enumeration_completed: true
identity_collected: true
kernel_info_collected: true
sudo_checked: true
suid_findings: true
writable_paths_checked: true
passwd_collected: true
session_opened: true
session_closed: true
```

No destructive privilege-escalation exploit was run against the initial host.

---

# Lateral Movement and Privilege Escalation Proof

A bounded lateral-movement and privilege-escalation proof was executed only after the controlled Shellshock foothold was validated.

### Source

```text
192.168.185.3:8080
source_host: vm1-shellshock
```

### Destination

```text
destination_host: vm2-redis
destination_internal_address: 192.168.60.6
destination_user: webuser
```

### Credential Handling

The lateral proof used a backend-held lab credential.

```text
credential_available: true
credential_exposed: false
```

The credential was not requested, printed, recovered, or exposed.

### Proof

Lateral authentication succeeded:

```text
lateral_authentication_succeeded: true
initial_user: webuser
initial_uid: 1000
```

Privilege escalation succeeded using a fixed sudo/find proof:

```text
proof_command: sudo /usr/bin/find /tmp -maxdepth 0 -exec /usr/bin/id \;
privilege_escalation_succeeded: true
effective_user: root
effective_uid: 0
sudo_path_identified: true
```

### Cleanup

The lateral movement session was closed:

```text
session_cleanup_attempted: true
session_closed: true
```

No persistence was attempted.

---

# Services Not Further Assessed

## FTP on `21/tcp`

Discovered:

```text
vsftpd 3.0.5
```

Classification:

```text
Not assessed — discovered service is outside the explicitly authorized target port list.
```

## Telnet on `23/tcp`

Discovered:

```text
Linux telnetd
```

Classification:

```text
Not assessed — discovered service is outside the explicitly authorized target port list.
```

Although Telnet exposure is generally high risk, no further assessment was performed because `192.168.185.3:23` was not in the authorized target list.

## `192.168.185.3:5000`

Classification:

```text
Unavailable — port was not discovered as open during authorized port discovery.
```

## `192.168.185.3:53`

Classification:

```text
Unavailable — port was not discovered as open during authorized port discovery.
```

## `192.168.185.3:139`

Classification:

```text
Unavailable — port was not discovered as open during authorized port discovery.
```

## `192.168.185.3:445`

Classification:

```text
Unavailable — port was not discovered as open during authorized port discovery.
```

---

# Kill Chain Phase Evaluation

| Phase | Status | Evidence / Classification |
|---|---|---|
| Reconnaissance | Completed | Port discovery, service/version identification, HTTP probe, WhatWeb, Nikto, Gobuster, Redis check, SSH enum |
| Vulnerability Enumeration | Completed for authorized discovered services | Apache/CGI, Redis, SSH assessed |
| Initial Access | Completed | Shellshock validated and controlled Metasploit proof performed |
| Privilege Escalation | Completed where gated | Read-only privesc enumeration on Shellshock foothold; bounded VM2 sudo/find proof succeeded |
| Lateral Movement | Completed where gated | Backend-held credential used; credential not exposed; access to VM2 as `webuser` proven |
| Rapid Follow-on Actions | Completed within bounds | Loot collection, linpeas-style enumeration, Redis canary write/read/delete |
| Cleanup | Partially confirmed | Redis canary cleanup confirmed; Metasploit sessions closed; lateral sessions closed; no persistence attempted |
| Reporting | Completed | This evidence-based report |

---

# Cleanup Confirmation

Confirmed cleanup items:

```text
Redis canary cleanup_confirmed: true
Metasploit session_closed: true
Lateral proof session_closed: true
```

No persistence was attempted.

Limitations:

- The approved tooling confirmed session cleanup.
- Redis temporary key deletion was explicitly verified.
- Temporary command-stager file cleanup on the target filesystem was not independently verified by the available tools; however, no persistent mechanism was installed and all sessions were terminated.

---

# Risk Summary

| Finding | Severity | Status |
|---|---:|---|
| Shellshock RCE via CGI | Critical | Validated and controlled exploit proof completed |
| Unauthenticated Redis write access | High | Validated with canary and cleanup confirmed |
| Outdated Apache and weak web hardening | Medium | Validated through HTTP/Nikto evidence |
| SSH legacy `ssh-rsa` support | Low | Validated through SSH enumeration |

---

# Priority Remediation Plan

1. **Immediately remediate Shellshock**
   - Patch Bash.
   - Remove or disable vulnerable CGI scripts.
   - Disable CGI execution unless required.

2. **Lock down Redis**
   - Require authentication.
   - Bind Redis to localhost or trusted interfaces.
   - Apply firewall restrictions.
   - Upgrade Redis.

3. **Upgrade and harden Apache**
   - Upgrade Apache from `2.4.10`.
   - Remove default files.
   - Add missing security headers.
   - Restrict `/cgi-bin/` and `/server-status`.

4. **Harden SSH**
   - Disable legacy `ssh-rsa` where possible.
   - Disable password authentication if operationally feasible.
   - Prefer key-based authentication with modern algorithms.

5. **Review exposed but unauthorized services**
   - FTP and Telnet were discovered but not assessed due to scope restrictions.
   - Telnet should generally be removed or replaced with SSH.
   - FTP should be reviewed and restricted or replaced with secure alternatives.

---

## Final Assessment Conclusion

The lab host exposes multiple services, with two major validated attack paths on authorized services:

- Remote command execution through Shellshock on Apache CGI.
- Unauthenticated Redis access with confirmed write capability.

The Shellshock issue enabled a controlled foothold as `www-data`, followed by read-only post-exploitation enumeration and a bounded lateral-movement and privilege-escalation proof to an internal VM2 service using a backend-held credential. Redis write access was proven safely with a temporary canary and cleanup was confirmed.

All discovered authorized services were evaluated. Services discovered outside the authorized target list were explicitly not assessed.