---
name: divergent-security
description: Rin's security review playbook for Team Divergent. Use for any code review, security audit, or diff analysis work. Encodes threat-based review order, severity calibration, and reporting standards. Complements the built-in /security-review scan.
---

# Divergent Security — Rin's Playbook

Works WITH /security-review: the scan is the automated first pass; this playbook is Rin's human-grade methodology on top.

## Review order (attacker's priorities, not file order)
1. **Auth surfaces first**: new/changed endpoints, middleware, session handling, password/token flows. Separate authN (are you logged in?) from authZ (may THIS user do THIS to THIS object?) — most real breaches are broken authZ: IDOR, horizontal access, missing ownership checks on mutations, and privilege escalation.
2. **Input paths second**: everywhere external data enters — request bodies, params, headers, file uploads, webhooks. Trace each to its sink (query, shell, template, outbound URL) — a server-side fetch built from user input is SSRF until proven otherwise.
3. **Data exits third**: responses, logs, error messages, exports — what leaks?
4. **New dependencies fourth**: what was added, why, how popular/maintained, what does it pull in transitively, any known-vulnerable version?
5. Everything else last

## The three questions per finding
For every suspicious pattern ask: (1) Can an attacker REACH this? (2) What do they GAIN? (3) How HARD is it? Reachable + valuable + easy = Critical. Unreachable defense-in-depth gaps are still findings — just calibrated honestly as Minor.

## Severity calibration (be consistent, not dramatic)
- **Critical**: exploitable now — injection, auth bypass, IDOR on real data, committed secret, RCE-shaped anything
- **Major**: exploitable with conditions, or systemic weakness — missing rate limits on auth, verbose errors leaking internals, weak crypto choices
- **Minor**: hardening opportunities, style-level security hygiene
Inflating severity burns trust; deflating it burns users. Calibrate like your report will be audited — it will.

## Review honesty rules
- Cite the exact file and code for every finding — no vibes-based flagging
- "I did not review X" is a valid and REQUIRED statement when scope-limited; silent gaps are worse than admitted ones
- Verify the scanner's findings yourself before repeating them; false positives in your report train the team to ignore you
- Re-reviews check two things only: prior findings resolved, and no new issues introduced BY the fixes

## Secrets protocol
A committed secret is compromised the moment it's committed — deletion is not remediation. Flag as Critical, require rotation, and check git history depth of exposure.

## Anti-patterns (never do)
- Approving with unverified assumptions ("Abnegation probably validated this upstream")
- Generic advice ("improve input validation") instead of file-specific findings
- Softening a verdict because the team is under deadline pressure
