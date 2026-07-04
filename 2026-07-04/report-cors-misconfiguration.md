# view.rockstargames.com: CORS Misconfiguration

**Program:** Rockstar Games  
**Severity:** Low  
**CWE:** CWE-942 (Insufficient CORS Configuration)  
**Scope:** `*.rockstargames.com` (wildcard scope ID: 39584)

## Summary

`view.rockstargames.com` has a CORS misconfiguration that reflects the `Origin` header value in the `Access-Control-Allow-Origin` response header and sets `Access-Control-Allow-Credentials: true`. This affects all paths on the domain.

## Steps to Reproduce

```bash
curl -s -D- -H "Origin: https://evil.com" https://view.rockstargames.com/
```

Response:
```
HTTP/1.1 303 See Other
location: https://view.rockstargames.com/?includeNativeClientLaunch=true
access-control-allow-origin: https://evil.com
access-control-allow-credentials: true
```

This also works with the null origin:
```bash
curl -s -D- -H "Origin: null" https://view.rockstargames.com/
```

And any arbitrary origin:
```bash
curl -s -D- -H "Origin: https://attacker.com" https://view.rockstargames.com/
```

## Impact

An attacker can make cross-origin credentialed requests to `view.rockstargames.com` from any malicious website. The domain sets an `ACCESSPOINTSESSIONID` cookie that would be included in these requests. While the current response is a 303 redirect to a 404 page (no sensitive data), this misconfiguration could be leveraged in combination with other attack vectors.

## Remediation

- Restrict `Access-Control-Allow-Origin` to specific trusted origins (e.g., `https://rockstargames.com`, `https://socialclub.rockstargames.com`)
- Remove `Access-Control-Allow-Credentials: true` unless specifically required for endpoints that serve authenticated content
- Disable CORS entirely on endpoints that do not need cross-origin access

## References

- CWE-942: https://cwe.mitre.org/data/definitions/942.html
- Rockstar Games Bounty Program: https://hackerone.com/rockstargames
