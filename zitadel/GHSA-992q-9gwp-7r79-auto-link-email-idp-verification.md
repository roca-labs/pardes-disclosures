# Auto-linking by email without IdP-side verification in ZITADEL

- **Project:** [zitadel/zitadel](https://github.com/zitadel/zitadel) — external identity-provider handler
- **Vulnerability class:** Improper Authentication / unauthorized account linking (auth-logic path-inconsistency)
- **CWE:** [CWE-287](https://cwe.mitre.org/data/definitions/287.html)
- **CVSS:** 4.8 Moderate (CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:L/A:N)
- **GHSA:** [GHSA-992q-9gwp-7r79](https://github.com/zitadel/zitadel/security/advisories/GHSA-992q-9gwp-7r79)
- **CVE:** [CVE-2026-56666](https://www.cve.org/CVERecord?id=CVE-2026-56666)
- **Status:** Fixed upstream (v4.15.3)
- **Reporters:** [`@ayadlin`](https://github.com/ayadlin) (Pardes / Roca LLC), with [Android-Login-Analysis](https://github.com/Android-Login-Analysis)
- **Advisory published:** 2026-06-22

## Summary

When *auto-linking by email* is enabled for an external identity provider, ZITADEL links an
incoming federated identity to an existing local account whenever the email addresses match. The
linking logic checks that the **local** user's email is verified — but does **not** cross-check
whether the **upstream IdP** actually verified ownership of that email address.

This is an authentication-logic *path inconsistency*: one side of the trust decision (local
verification) is enforced, while the parallel side (external-provider verification) is silently
omitted. The two must both hold for auto-linking to be safe.

## Impact

If an administrator enables email auto-linking for an external provider that permits sign-up with
an **unverified** email address, an attacker can:

1. Register an account at that permissive provider using a *victim's* email address (which the
   provider never verifies).
2. Log in to ZITADEL through that provider.
3. Because the email string matches, ZITADEL auto-links the attacker's federated identity to the
   victim's existing local account — with no interaction or confirmation from the victim.

The attacker then has authenticated access to the victim's account. The severity is Moderate
because the attack is configuration-dependent: it requires an administrator to have explicitly
enabled email auto-linking against a provider that does not enforce email verification.

## Affected logic

The flaw is in the external identity-provider link handler's auto-link-by-email path: the
verification gate considers the local account's `isEmailVerified` state but does not require the
external provider's asserted email to be verified upstream before proceeding with the automatic
link.

## Affected versions

- **4.x:** `4.0.0` through `4.15.2` (including RC versions)
- **3.x:** `3.0.0` through `3.4.12` (including RC versions)

## Fix

Fixed in [v4.15.3](https://github.com/zitadel/zitadel/releases/tag/v4.15.3): the external
provider's email-verification status is now explicitly validated before any automatic linking
logic proceeds.

**Workarounds** (if immediate upgrade is not possible):
- Disable email auto-linking (`AUTO_LINKING_OPTION_UNSPECIFIED`) on the affected identity providers.
- Restrict email auto-linking to trusted enterprise directories (e.g. corporate Okta / Entra ID)
  where upstream email verification is enforced by policy.

## Discovery

Identified through Pardes' authentication-flow analysis, which focuses on *auth-logic
inconsistencies* — cases where two code paths that must agree on a security decision diverge (here:
local vs. upstream email-verification checks in account linking). This is the same class as the
`request_uri` consent-path SSRF disclosed against Janssen jans-auth-server, where one request path
enforced a validation the parallel path omitted.

## Acknowledgment

Thanks to the [ZITADEL](https://github.com/zitadel) security team for a clean coordinated
disclosure and a prompt fix, and to [Android-Login-Analysis](https://github.com/Android-Login-Analysis)
for the collaborative report.
