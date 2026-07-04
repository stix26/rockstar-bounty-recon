# Rockstar Games – Bug Bounty Reconnaissance

**Target:** Rockstar Games (`*.rockstargames.com`)  
**HackerOne Program:** [Rockstar Games](https://hackerone.com/rockstargames)  
**Date:** 2026-07-04  
**Researcher:** stix26  

## Summary

Reconnaissance against Rockstar Games' HackerOne bug bounty program. Wildcard scope `*.rockstargames.com` with max severity **critical**.

### Key Finding

| Finding | Severity | Status |
|---------|----------|--------|
| CORS misconfiguration on `view.rockstargames.com` (origin reflection + credentials) | Low | Report prepared |

## Scope

In-scope wildcard: `*.rockstargames.com` (eligible for bounty, max severity critical)

Excluded subdomains: `bomgar`, `emailcontent`, `faspex`, `any-invalid-domains`

## Recon Data

See `2026-07-04/` directory for:
- `subdomains.txt` — 543 subdomains enumerated
- `live-hosts.txt` — 113 live hosts detected
- `cors-tests.txt` — CORS testing results
- `exposed-files.txt` — Exposed file scanning

## Report

- **Title:** view.rockstargames.com: CORS misconfiguration reflecting arbitrary origin with Access-Control-Allow-Credentials: true
- **Type:** CORS misconfiguration (CWE-942)
- **Severity:** Low
- **Status:** Report prepared for manual submission via HackerOne
