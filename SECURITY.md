# Security Policy

## Supported Versions

Security fixes are applied to the latest version of each plugin. We do not backport fixes to older versions.

| Version | Supported |
|---------|-----------|
| latest  | ✅ Yes     |
| older   | ❌ No      |

---

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

If you discover a security issue in a plugin or in how Conductor loads plugins, please report it privately:

**Email:** security@thealxlabs.ca  
**Subject:** `[conductor-plugins] <brief description>`

Include:
- Which plugin is affected (e.g. `gmail`, `github`)
- A description of the vulnerability
- Steps to reproduce, if applicable
- Potential impact (what an attacker could do)
- Whether you have a suggested fix

We will acknowledge your report within **48 hours** and aim to release a fix within **7 days** for critical issues.

---

## What Counts as a Security Issue

Please report:

- A plugin leaking credentials or sending them to third parties
- Code in this repo that could be used to compromise a user's system or data
- A way for a malicious plugin to bypass Conductor's credential storage
- Dependency vulnerabilities in plugin `package.json` files that have a realistic exploit path
- Supply chain issues (e.g. a compromised dependency)

Not a security issue (use a regular bug report):

- A plugin not working correctly
- A missing feature
- An API rate limit being hit

---

## Plugin Security Model

Conductor plugins run in the same Node.js process as Conductor itself. They are **not sandboxed**. This means:

- Plugins have access to the filesystem, network, and environment variables
- Plugins can read (but not write) the Conductor keychain through the provided API
- Plugins cannot access credentials for other plugins directly

All plugins in this registry are **manually reviewed** before being added. However, you should still only install plugins you trust, especially community-contributed ones.

---

## Responsible Disclosure

We follow [coordinated disclosure](https://en.wikipedia.org/wiki/Coordinated_vulnerability_disclosure). We ask that you:

- Give us reasonable time to fix the issue before publishing details publicly
- Not exploit the vulnerability beyond what is needed to demonstrate it
- Not access, modify, or delete other users' data

In return, we will:

- Acknowledge your report promptly
- Keep you informed of our progress
- Credit you in the fix (if you'd like)
- Not take legal action against you for good-faith research

---

## Hall of Fame

Researchers who have responsibly disclosed vulnerabilities:

*(none yet — be the first)*
