# Pardes Disclosures

Public archive of coordinated security disclosures identified by [Pardes](https://pardes.systems) and reported through responsible-disclosure channels.

Each entry is published **after** the upstream project ships a fix and the disclosure embargo lifts. We never publish details that could expose users to active exploitation.

---

## What Pardes is

Pardes is a multi-source static security analysis platform that combines large-language-model cross-validation with deterministic code-property-graph taint analysis. It is currently in private beta, used internally to audit production codebases and select open-source bounty targets.

→ [pardes.systems](https://pardes.systems)

---

## Disclosure principles

1. **Coordinated, not surprise.** Findings are reported privately via the upstream project's published security channel. We wait for the fix to ship before publishing here.
2. **Credit is mutual.** When upstream credits the discoverer, we credit upstream's responsiveness and the maintainers who shipped the fix.
3. **No exploit-for-exploit's-sake.** We publish post-fix because the goal is normalizing the report→fix→credit cycle, not adversarial PR.
4. **Reproducibility matters.** Each entry includes the file:line, the vulnerability class (CWE), the conditions under which it was exploitable, and a link to the upstream patch.

---

## Index

| Date submitted | Target | Vulnerability | Severity | Status | Entry |
|---|---|---|---|---|---|
| 2026-04-30 | cal.com / cal.diy | SSRF + OAuth credential disclosure in Zoho Calendar callback | HIGH | Filed GHSA after 26-day email silence; in triage | _coming after embargo_ |
| 2026-05-12 | Janssen jans-auth-server | SSRF (CWE-918) in `request_uri` → `AuthorizeAction.getRequestedClaims` | HIGH | Fix shipped (PR #14086); GHSA pending publication | _coming after embargo_ |
| 2026-05-12 | Janssen jans-auth-server | Reflected XSS (CWE-79) in `EndSessionUtils.createFronthannelHtml` | HIGH | Fix shipped (PR #14103); GHSA pending publication | _coming after embargo_ |
| 2026-05-12 | Janssen jans-auth-server | SSRF (CWE-918, DCR-gated) in `sector_identifier_uri` → `RedirectionUriService.getSectorRedirectUris` | HIGH | Fix shipped (PR #14111); GHSA pending publication | _coming after embargo_ |

_(Entries are added once upstream confirms remediation and the embargo expires.)_

---

## Reporting your own findings

Pardes does not accept third-party submissions to this archive. If you've found a vulnerability and want to disclose it through Pardes' channel, contact [security@pardes.systems](mailto:security@pardes.systems) and we'll coordinate.

For inquiries about Pardes the platform, contact [admin@pardes.systems](mailto:admin@pardes.systems).

---

© Roca LLC. Pardes is a product of Roca LLC.
