# SSRF via sector_identifier_uri in jans-auth-server Dynamic Client Registration

- **Target:** JanssenProject/jans — `jans-auth-server`
- **Vulnerability class:** Server-Side Request Forgery (SSRF) — DCR-gated
- **CWE:** [CWE-918](https://cwe.mitre.org/data/definitions/918.html)
- **CVSS:** 7.5 HIGH (CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N)
- **GHSA:** [GHSA-9qw4-gwgf-3q27](https://github.com/JanssenProject/jans/security/advisories/GHSA-9qw4-gwgf-3q27)
- **Affected:** <= 2.1.0  **Patched:** >= 2.2.0 (and 0.0.0-nightly)
- **Location:** `io/jans/as/server/service/RedirectionUriService.java` — `getSectorRedirectUris` (~L70-101)
- **Reported by:** ayadlin / Pardes (coordinated disclosure; published post-fix)

## Summary
During Dynamic Client Registration the `sector_identifier_uri` is fetched with no scheme/host/IP validation, enabling server-side requests to cloud metadata endpoints and internal services (when DCR is enabled).
