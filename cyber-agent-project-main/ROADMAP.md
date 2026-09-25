# Roadmap

Curated capabilities by hacking-methodology phase. Scope is deliberately limited
to a representative set of Kali tools that fit the lab (SEED VM / Docker
targets) — not all of Kali. Each unbuilt item lists the branch to use when work
starts (see [`CONTRIBUTING.md`](CONTRIBUTING.md)).

Status: `[x]` done · `[ ]` planned.

## Reconnaissance
- [x] `http_probe` — curl headers, body preview, CGI/Apache indicators
- [x] `linux_command` — restricted Kali-side read-only command capability
- [x] `whatweb` — web technology fingerprinting · `feat/recon-whatweb` (team flagged low value)
- [x] `dnsenum` — DNS enumeration · `feat/recon-dnsenum` (team flagged low value)

## Scanning / Enumeration
- [x] `nmap_scan` — `nmap -sV` service/version detection
- [x] `nikto` — web server vulnerability scan · `feat/enum-nikto`
- [x] `ssh_enum` — SSH banner, host keys, algorithms, and supported methods
- [x] `gobuster` — directory/file brute-forcing · `feat/enum-gobuster`
- [x] `enum4linux` — SMB/NetBIOS enumeration · `feat/enum-enum4linux` (team flagged low value)

## Vulnerability Analysis
- [x] `shellshock_check` — Shellshock (CVE-2014-6271) validation
- [x] `searchsploit` — map services to known exploits · `feat/vuln-searchsploit`
- [x] `nmap_nse` — nmap `--script vuln` checks · `feat/vuln-nmap-nse`
- [x] `idor_check` — insecure direct object reference probe · `feat/vuln-idor`

## Exploitation
- [x] `metasploit` — exploit framework integration · `feat/exploit-metasploit`
- [x] `redis_check` — unauthenticated Redis access validation · `feat/exploit-redis_check`

## Post-Exploitation / Privilege Escalation
- [x] `linpeas` — local privilege-escalation enumeration · `feat/post-linpeas`
- [x] `loot` — collect evidence/artifacts from a foothold · `feat/post-loot`

## Documentation / Reporting
Controller-side (under `controller/reporting/`) — these synthesize collected evidence.
- [x] `markdown_report` — render findings into a structured report · `feat/report-markdown`

---
