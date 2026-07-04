# Bug Bounty Session — July 4, 2026

**HackerOne User:** stix26  
**Date:** 2026-07-04  
**Duration:** 1 session  
**Target:** Rockstar Games

---

## Summary

Full recon pipeline against Rockstar Games' bug bounty program.

**Repos created:**
- `github.com/stix26/rockstar-bounty-recon`

**Report filed:** CORS misconfiguration on `view.rockstargames.com` (prepared for manual submission)

---

## Tools Used

| Tool | Purpose |
|------|---------|
| subfinder v2.14.0 | Subdomain enumeration (543 found) |
| curl | Live probing, CORS testing, exposed file checks |
| HackerOne API | Program discovery, scope analysis |

---

## Reconnaissance

### 1. Subdomain Enumeration

- 543 subdomains found via subfinder for `rockstargames.com`
- DNS sources: Certificate Transparency, search engines, DNS record aggregation

### 2. Live Host Probing

- 113 live hosts identified via curl
- Key targets: `view.rockstargames.com`, `bomgar.rockstargames.com`, `signin.rockstargames.com`, `store.rockstargames.com`, `socialclub.rockstargames.com`

### 3. CORS Testing

- Tested all live hosts with `Origin: https://evil.com` and `Origin: null`
- **Finding: `view.rockstargames.com` reflects arbitrary Origin with `Access-Control-Allow-Credentials: true`**
- All other hosts properly restricted or no CORS headers

### 4. Exposed File Scanning

- Checked: `.env`, `.git/config`, `dump.sql`, `config.php`, `phpinfo.php`, `robots.txt`, `sitemap.xml`, `.well-known/security.txt`
- All paths returned proper 404 responses — no exposed files found

---

## Key Lessons

1. Rockstar Games has a well-configured security posture on most endpoints
2. CORS reflection with credentials is present on `view.rockstargames.com`
3. The `view.rockstargames.com` endpoint is likely part of the Social Club / Rockstar Games Launcher authentication flow
4. Several subdomains are properly excluded from bounty scope (bomgar, emailcontent, faspex)
