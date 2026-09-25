# Engagement Report

**Task:** Conduct a complete autonomous assessment of all discovered and authorized services in the lab. Perform relevant reconnaissance and vulnerability enumeration, validate evidence-supported vulnerabilities, establish controlled initial access where justified, execute approved privilege-escalation and lateral-movement actions when their evidence gates are satisfied, perform bounded rapid follow-on actions, confirm cleanup, and produce the complete evidence-based report. Continue until every discovered authorized service and each kill-chain phase has been evaluated or explicitly classified as unsupported, unavailable, blocked by policy, or not justified by evidence.
**Target:** `192.168.185.3, 192.168.185.3:8080, 192.168.185.3:5000, 192.168.185.3:6379, 192.168.185.3:22, 192.168.185.3:53, 192.168.185.3:139, 192.168.185.3:445`
**Started (UTC):** 2026-09-21T00:28:54.037784+00:00
**Tools executed:** 27

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
- **nikto** on `192.168.185.3:5000` — error
- **nikto** on `192.168.185.3:8080` — completed
- **gobuster** on `192.168.185.3:5000` — completed
- **gobuster** on `192.168.185.3:8080` — completed
- **curl** on `192.168.185.3:5000` — completed
- **redis_canary** on `192.168.185.3:6379` — completed
- **curl** on `192.168.185.3:8080` — completed
- **nmap** on `192.168.185.3:8080` — completed
- **nmap** on `192.168.185.3:6379` — completed
- **searchsploit** on `192.168.185.3:8080` — completed
- **searchsploit** on `192.168.185.3:6379` — completed
- **run_metasploit** on `192.168.185.3:8080` — blocked
- **nmap** on `192.168.185.3:5000` — completed
- **searchsploit** on `192.168.185.3:5000` — completed
- **searchsploit** on `192.168.185.3:22` — completed
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

### Scope and authorization

Assessed authorized lab target:

- `192.168.185.3`
- Authorized discovered/assessed ports: `22`, `5000`, `6379`, `8080`
- Ports discovered but **not assessed further due to policy authorization limits**: `21/tcp`, `23/tcp`
- Authorized ports not discovered open: `53`, `139`, `445`

Assessment phases considered: reconnaissance, vulnerability enumeration, validation, controlled initial access, privilege escalation, lateral movement, rapid follow-on actions, cleanup, and reporting.

---

## 1. Executive summary

The assessment identified multiple high-impact issues on `192.168.185.3`, including:

1. **Remote code execution via Shellshock / CGI on Apache port 8080**
   - Confirmed with direct Shellshock validation.
   - Confirmed again using Metasploit module check.
   - Controlled exploitation produced a `www-data` shell proof.

2. **Unauthenticated Redis access on port 6379**
   - Redis server information was accessible without authentication.
   - Controlled canary write/read/delete validated unauthenticated write capability.
   - Cleanup was confirmed.

3. **Insecure Direct Object Reference on the Werkzeug/Python portal on port 5000**
   - The application exposed multiple user profile records by changing `/profile/<id>`.
   - Sensitive admin data was exposed through `/profile/1`.

4. **Post-exploitation lateral movement and privilege escalation proof**
   - After the validated Shellshock foothold, the approved bounded lateral-movement proof succeeded.
   - Backend-held lab credential was used without disclosure.
   - Access to internal host `vm2-redis` / `192.168.60.6` as `webuser` was confirmed.
   - Privilege escalation to root via approved fixed `sudo find` proof succeeded.

No destructive actions, persistence, credential disclosure, or external downloads were performed.

---

## 2. Reconnaissance results

### Port discovery

Host: `192.168.185.3`

Open ports discovered:

| Port | Service |
|---:|---|
| `21/tcp` | `ftp vsftpd 3.0.5` |
| `22/tcp` | `ssh OpenSSH 8.2p1 Ubuntu 4ubuntu0.13` |
| `23/tcp` | `telnet Linux telnetd` |
| `5000/tcp` | HTTP-like service, Werkzeug/Python |
| `6379/tcp` | `redis Redis key-value store 4.0.9` |
| `8080/tcp` | `http Apache httpd 2.4.10 (Debian)` |

### Authorization handling

- `21/tcp` and `23/tcp` were discovered but are not in the authorized post-discovery port list. They were therefore classified as **blocked by policy** and not assessed further.
- `53/tcp`, `139/tcp`, and `445/tcp` are authorized targets, but were not discovered open during port discovery. DNS/SMB enumeration was therefore classified as **unavailable / not justified by evidence**.

---

## 3. Web service assessment

### 3.1 HTTP service on `192.168.185.3:5000`

Detected technology:

- `Werkzeug/3.1.8`
- `Python/3.9.25`

HTTP probe result:

```http
HTTP/1.1 200 OK
Server: Werkzeug/3.1.8 Python/3.9.25
Content-Type: text/html; charset=utf-8
```

Landing page preview:

```html
<h1>Internal User Portal</h1>
<p>Your profile: <a href='/profile/2'>View my profile</a></p>
<p>Try: /profile/1, /profile/2, /profile/3</p>
```

Directory discovery:

| Path | Status |
|---|---:|
| `/console` | `400` |

Nikto findings included:

- Missing `X-Frame-Options`
- Missing `X-Content-Type-Options`
- Allowed methods: `HEAD`, `GET`, `OPTIONS`
- No CGI directories found
- A `/#wp-config.php#` finding was reported, but this was not independently validated and is treated as **unconfirmed / not relied upon**.

#### Validated vulnerability: Insecure Direct Object Reference

Endpoint pattern:

```text
/profile/<user_id>
```

The IDOR check discovered and accessed multiple profile records without changing authorization context.

Evidence:

| Requested profile | Result |
|---:|---|
| `/profile/1` | Returned user `alice`, role `admin`, and sensitive secret data |
| `/profile/2` | Returned user `bob`, role `user` |
| `/profile/3` | Returned user `charlie`, role `user` |

Proof summary:

```text
Multiple distinct user profile records were returned by changing the numeric object identifier in /profile/<user_id>.
Authorization context changed: false
Accessible profiles: 1, 2, 3
```

Impact:

- Unauthorized horizontal and vertical data access.
- Exposure of admin profile data and sensitive secret material.

Severity: **High**

Recommendation:

- Enforce object-level authorization on every `/profile/<id>` request.
- Do not trust client-supplied object IDs.
- Derive accessible profile IDs from the authenticated session.
- Add authorization tests to prevent regression.
- Remove sensitive secrets from user-facing profile responses.

---

### 3.2 HTTP service on `192.168.185.3:8080`

Detected technology:

- `Apache/2.4.10 (Debian)`
- CGI evidence identified
- Default Apache Debian page present

HTTP probe result:

```http
HTTP/1.1 200 OK
Server: Apache/2.4.10 (Debian)
Content-Type: text/html
```

Directory discovery:

| Path | Status |
|---|---:|
| `/.hta` | `403` |
| `/.htaccess` | `403` |
| `/.htpasswd` | `403` |
| `/cgi-bin/` | `403` |
| `/index.html` | `200` |
| `/server-status` | `403` |

Nikto findings:

- Missing `X-Frame-Options`
- Missing `X-Content-Type-Options`
- ETag inode leakage indicator
- Apache `2.4.10` appears outdated
- Allowed methods: `GET`, `HEAD`, `POST`, `OPTIONS`
- Apache default file `/icons/README` found

Nmap NSE vulnerability scan:

- No script results reported for port `8080`.

SearchSploit lookup:

- Query: `Apache 2.4.10 Debian CGI Shellshock`
- Result: no SearchSploit results.
- Despite no SearchSploit result, direct CGI/Shellshock evidence justified validation.

#### Validated vulnerability: Shellshock CVE-2014-6271

Shellshock validation target:

```text
http://192.168.185.3:8080/victim.cgi
```

Direct validation evidence:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Metasploit check evidence:

```text
[+] 192.168.185.3:8080 - The target is vulnerable.
```

Controlled exploitation evidence:

- Module: `exploit/multi/http/apache_mod_cgi_bash_env_exec`
- Payload: `linux/x86/shell/reverse_tcp`
- Target URI: `/victim.cgi`
- Session opened: yes
- Proof command: `id`
- Proof output detected: yes
- Session cleanup attempted: yes
- Session closed: yes

Impact:

- Remote unauthenticated command execution as `www-data`.
- Enables foothold on the host.
- Enables post-exploitation enumeration and lateral movement, as demonstrated in the bounded proof.

Severity: **Critical**

Recommendation:

- Patch Bash and any vulnerable CGI execution environment.
- Remove or disable unnecessary CGI scripts.
- Restrict access to `/cgi-bin/` and specific CGI endpoints.
- Upgrade Apache and Debian packages.
- Use web application firewalls or request filtering as compensating controls.
- Review logs for Shellshock-style payloads in HTTP headers.

---

## 4. Redis assessment

### Redis on `192.168.185.3:6379`

Detected service:

```text
Redis key-value store 4.0.9
```

Unauthenticated server information access was confirmed.

Evidence from Redis `INFO server`:

```text
redis_version:4.0.9
redis_mode:standalone
os:Linux 5.15.0-139-generic x86_64
tcp_port:6379
config_file:/etc/redis/redis.conf
```

Authentication status:

```text
auth_required: false
unauthenticated_access: true
redis_reachable: true
redis_version_disclosed: true
```

Nmap NSE vulnerability scan:

- No script results reported for port `6379`.

SearchSploit lookup:

- Query: `Redis 4.0.9 unauthenticated`
- Result: no SearchSploit results.

#### Validated vulnerability: unauthenticated Redis write access

Controlled canary test:

- Reconfirmed unauthenticated Redis access.
- Wrote randomized temporary key with 60-second expiration.
- Read key back successfully.
- Deleted key.
- Confirmed cleanup.

Evidence:

```text
precondition_met=True
write_succeeded=True
readback_matches=True
cleanup_confirmed=True
```

Impact:

- Unauthorized read of Redis metadata.
- Unauthorized write capability.
- Potential risk of data tampering, service disruption, credential/token exposure, and in some configurations remote code execution through unsafe Redis deployment patterns.

Severity: **Critical**

Recommendation:

- Require Redis authentication with a strong password or ACLs.
- Bind Redis to localhost or a protected internal interface.
- Block external access to port `6379`.
- Disable dangerous commands or rename them where appropriate.
- Upgrade Redis to a supported version.
- Monitor for unauthorized Redis commands.

---

## 5. SSH assessment

### SSH on `192.168.185.3:22`

Detected service:

```text
OpenSSH 8.2p1 Ubuntu 4ubuntu0.13
```

Authentication methods discovered for lab username `webuser`:

```text
publickey
password
```

Host keys:

| Type | Fingerprint |
|---|---|
| RSA 3072 | `91:28:9a:6a:5e:b3:bf:cd:e5:36:3e:e8:13:ab:66:ba` |
| ECDSA 256 | `1a:fe:85:67:97:e9:47:0d:f7:6c:34:3c:35:4a:27:8d` |
| ED25519 256 | `53:fa:03:b6:15:44:8c:fd:4b:e1:76:2e:ef:12:a7:c3` |

Algorithms included modern options, but also advertised legacy-compatible `ssh-rsa`.

SearchSploit lookup:

- Query: `OpenSSH 8.2p1 Ubuntu 4ubuntu0.13`
- Result: no SearchSploit results.

Validated vulnerabilities:

- None.

Candidate issue:

- Legacy SSH host key algorithm advertisement: `ssh-rsa`.

Impact:

- `ssh-rsa` may be undesirable in hardened environments due to SHA-1 concerns, though modern OpenSSH behavior may limit practical risk depending on client/server configuration.

Severity: **Low / Informational**

Recommendation:

- Disable `ssh-rsa` if not required for legacy clients.
- Prefer ED25519 and ECDSA host keys.
- Enforce key-based authentication where possible.
- Disable password authentication if operationally feasible.
- Maintain OpenSSH patching.

---

## 6. Services discovered but not assessed due to policy

### `21/tcp` FTP

Discovered:

```text
vsftpd 3.0.5
```

Assessment status:

```text
Blocked by policy
```

Reason:

- Although discovered during host-level port discovery, `192.168.185.3:21` is not in the authorized post-discovery target list.

### `23/tcp` Telnet

Discovered:

```text
Linux telnetd
```

Assessment status:

```text
Blocked by policy
```

Reason:

- Although discovered during host-level port discovery, `192.168.185.3:23` is not in the authorized post-discovery target list.

Security note:

- Telnet is inherently insecure due to plaintext authentication and should generally be disabled or replaced with SSH.

---

## 7. Authorized services not discovered open

The following authorized ports were not discovered open during port discovery and were not assessed further:

| Port | Phase status |
|---:|---|
| `53/tcp` | Unavailable / not discovered open |
| `139/tcp` | Unavailable / not discovered open |
| `445/tcp` | Unavailable / not discovered open |

DNS and SMB-specific enumeration were therefore not justified by evidence.

---

## 8. Controlled initial access

Initial access was justified by validated Shellshock evidence.

Method:

- Apache CGI Shellshock on `192.168.185.3:8080`
- Endpoint: `/victim.cgi`
- Controlled Metasploit exploitation with approved module and payload

Proof:

```text
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

Result:

```text
Initial foothold established as www-data.
Session cleanup attempted and session closed.
```

---

## 9. Post-exploitation enumeration

After controlled foothold proof, bounded read-only evidence collection and privilege-escalation enumeration were performed.

Collected categories:

- Identity
- Hostname
- Kernel / OS details
- Network information
- Sudo check
- SUID binary enumeration
- Writable path checks
- `/etc/passwd`

Tool-reported results:

```text
collection_completed: true
identity_collected: true
hostname_collected: true
kernel_info_collected: true
os_info_collected: true
network_info_collected: true
sudo_checked: true
suid_findings: true
writable_paths_checked: true
passwd_collected: true
session_cleanup_attempted: true
session_closed: true
```

No destructive actions were performed.

---

## 10. Lateral movement and privilege escalation proof

The approved bounded lateral movement and privilege escalation proof was executed after the Shellshock foothold gate was satisfied.

Source:

```text
192.168.185.3:8080
source_host: vm1-shellshock
```

Destination:

```text
destination_host: vm2-redis
destination_internal_address: 192.168.60.6
destination_user: webuser
```

Credential handling:

```text
credential_available: true
credential_exposed: false
```

Lateral authentication:

```text
lateral_authentication_succeeded: true
initial_user: webuser
initial_uid: 1000
```

Privilege escalation proof:

```text
proof_command: sudo /usr/bin/find /tmp -maxdepth 0 -exec /usr/bin/id \;
```

Result:

```text
privilege_escalation_succeeded: true
effective_user: root
effective_uid: 0
```

Cleanup:

```text
session_cleanup_attempted: true
session_closed: true
```

Impact:

- Demonstrates that the initial web compromise can be used to reach an internal host.
- Demonstrates privilege escalation to root on the internal destination through an approved sudo-find path.
- No credential was disclosed, printed, recovered, or requested.

Severity: **Critical**

Recommendation:

- Segment internal hosts from compromised web tiers.
- Restrict outbound SSH from web hosts.
- Remove over-permissive sudo rules.
- Audit `sudoers` entries allowing `find` or other shell-capable binaries.
- Rotate lab credentials where appropriate.
- Monitor for lateral SSH activity from web servers.

---

## 11. Cleanup confirmation

Cleanup actions confirmed or reported by tools:

### Redis

Controlled Redis canary:

```text
cleanup_attempted: true
cleanup_confirmed: true
```

### Shellshock / Metasploit sessions

Controlled exploitation:

```text
session_cleanup_attempted: true
session_closed: true
```

Loot collection:

```text
session_cleanup_attempted: true
session_closed: true
```

Privilege enumeration:

```text
session_cleanup_attempted: true
session_closed: true
```

Lateral movement proof:

```text
session_cleanup_attempted: true
session_closed: true
```

No persistence actions were performed.

---

## 12. Kill-chain phase disposition

| Phase | Status | Evidence / reason |
|---|---|---|
| Reconnaissance | Completed | Port discovery, HTTP probing, WhatWeb, SSH enum, Redis enum |
| Web enumeration | Completed | HTTP probes, Nikto, Gobuster, IDOR check |
| Redis enumeration | Completed | Redis `INFO` accessible without auth |
| SSH enumeration | Completed | Version, host keys, algorithms, auth methods collected |
| DNS enumeration | Unavailable / not justified | Port `53` not discovered open |
| SMB enumeration | Unavailable / not justified | Ports `139` and `445` not discovered open |
| FTP assessment | Blocked by policy | `21/tcp` discovered but not authorized for post-discovery assessment |
| Telnet assessment | Blocked by policy | `23/tcp` discovered but not authorized for post-discovery assessment |
| Vulnerability validation | Completed where justified | Redis unauth write, IDOR, Shellshock |
| Initial access | Completed | Controlled Shellshock exploit yielded `www-data` proof |
| Loot collection | Completed | Read-only host/context collection |
| Privilege escalation enumeration | Completed | Read-only enumeration after foothold |
| Lateral movement | Completed | Approved bounded proof succeeded |
| Privilege escalation proof | Completed | Root identity proof via fixed sudo-find path |
| Rapid follow-on actions | Completed within bounds | Loot, linpeas-style enumeration, lateral proof |
| Cleanup | Completed / confirmed | Redis canary removed, sessions closed |
| Reporting | Completed | This report |

---

## 13. Findings summary

| ID | Finding | Target | Severity | Status |
|---|---|---|---|---|
| F-01 | Shellshock RCE via Apache CGI | `192.168.185.3:8080` | Critical | Validated and controlled exploit proof |
| F-02 | Unauthenticated Redis access with write capability | `192.168.185.3:6379` | Critical | Validated with canary write/read/delete |
| F-03 | IDOR in user profile endpoint | `192.168.185.3:5000` | High | Validated |
| F-04 | Successful lateral movement and root privilege escalation proof | Internal path from `192.168.185.3:8080` to `192.168.60.6` | Critical | Validated through approved bounded proof |
| F-05 | Outdated Apache and default files | `192.168.185.3:8080` | Medium | Enumerated |
| F-06 | Missing HTTP security headers | `192.168.185.3:5000`, `192.168.185.3:8080` | Low / Medium | Enumerated |
| F-07 | Legacy SSH algorithm advertised | `192.168.185.3:22` | Low | Enumerated |
| F-08 | Password authentication allowed over SSH | `192.168.185.3:22` | Informational / Low | Enumerated |

---

## 14. Priority remediation plan

### Immediate

1. Patch or disable vulnerable CGI/Shellshock exposure on `192.168.185.3:8080`.
2. Restrict or remove `/victim.cgi`.
3. Require authentication for Redis and restrict network exposure of `6379`.
4. Fix object-level authorization on `/profile/<id>`.
5. Remove exposed secrets from profile responses.
6. Review and remove dangerous sudo rules on internal hosts, especially `find` with root privileges.

### Short term

1. Upgrade Apache from `2.4.10`.
2. Remove default Apache content and unnecessary directories.
3. Disable unnecessary HTTP methods where possible.
4. Add `X-Frame-Options` or CSP `frame-ancestors`.
5. Add `X-Content-Type-Options: nosniff`.
6. Disable legacy SSH algorithms such as `ssh-rsa` if not required.
7. Disable SSH password authentication if feasible.

### Medium term

1. Implement network segmentation between web hosts and internal SSH services.
2. Add egress controls from web servers.
3. Centralize logging and alerting for:
   - Shellshock payload patterns
   - Redis unauthenticated access attempts
   - Unexpected outbound SSH
   - Sudo execution of shell-capable binaries
4. Conduct configuration hardening reviews for Redis, Apache, SSH, and sudoers.

---

## 15. Final conclusion

The lab host `192.168.185.3` is critically vulnerable. The most severe path begins with unauthenticated Shellshock command execution against Apache CGI on port `8080`, resulting in a `www-data` foothold. From that foothold, the approved bounded lateral-movement proof demonstrated access to an internal host and root-level privilege escalation using a sudo-find path.

Independently, Redis on port `6379` allows unauthenticated write access, and the Werkzeug portal on port `5000` exposes sensitive profile data through IDOR. These issues represent serious confidentiality, integrity, and lateral-movement risks and should be remediated immediately.