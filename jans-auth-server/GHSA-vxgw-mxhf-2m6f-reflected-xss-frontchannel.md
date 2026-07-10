# Reflected XSS (JavaScript context) in jans-auth-server front-channel logout

- **Target:** JanssenProject/jans — `jans-auth-server`
- **Vulnerability class:** Reflected Cross-Site Scripting (XSS), JavaScript-context injection
- **CWE:** [CWE-79](https://cwe.mitre.org/data/definitions/79.html)
- **CVSS:** 9.3 CRITICAL (CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:N)
- **GHSA:** [GHSA-vxgw-mxhf-2m6f](https://github.com/JanssenProject/jans/security/advisories/GHSA-vxgw-mxhf-2m6f)
- **Affected:** <= 2.1.0  **Patched:** >= 2.2.0 (and 0.0.0-nightly)
- **Location:** `io/jans/as/server/session/ws/rs/EndSessionUtils.java` — `createFronthannelHtml` (~L77-101)
- **Reported by:** ayadlin / Pardes (coordinated disclosure; published post-fix)

## Summary
The OAuth `state` parameter is concatenated into a `<script>` block without escaping, allowing arbitrary JavaScript execution in the front-channel logout page. Fix: JSON-encode parameter values before insertion into a JavaScript context.
