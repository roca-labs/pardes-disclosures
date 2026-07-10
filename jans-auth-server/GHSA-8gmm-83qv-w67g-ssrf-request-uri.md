# SSRF via request_uri in jans-auth-server consent rendering

- **Target:** JanssenProject/jans — `jans-auth-server`
- **Vulnerability class:** Server-Side Request Forgery (SSRF)
- **CWE:** [CWE-918](https://cwe.mitre.org/data/definitions/918.html)
- **CVSS:** 6.5 MODERATE (CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N)
- **GHSA:** [GHSA-8gmm-83qv-w67g](https://github.com/JanssenProject/jans/security/advisories/GHSA-8gmm-83qv-w67g)
- **Affected:** <= 2.1.0  **Patched:** >= 2.2.0 (and 0.0.0-nightly)
- **Location:** `io/jans/as/server/authorize/ws/rs/AuthorizeAction.java` — `getRequestedClaims` (~L566-600)
- **Reported by:** ayadlin / Pardes (coordinated disclosure; published post-fix)

## Summary
The consent-rendering (JSF) path fetches `request_uri` WITHOUT the allowlist/blocklist validation the REST handler applies — an authorization-logic inconsistency between two code paths. An authenticated attacker can drive outbound requests to internal services / cloud metadata.
