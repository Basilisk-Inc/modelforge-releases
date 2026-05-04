# Security Policy

## Reporting a vulnerability

**Please do not open public GitHub issues for security vulnerabilities.** Public disclosure before a fix is available puts users at risk. Use the private channels below instead.

### Preferred channels

1. **GitHub Security Advisories** — go to the [Security tab](https://github.com/TODO_GITHUB_ORG/modelforge-releases/security/advisories/new) and click "Report a vulnerability". This is private and gives us the most context up front.

2. **Email** — TODO_SECURITY_EMAIL with subject `[SECURITY] <short description>`.

   You may encrypt with our PGP key:
   ```
   TODO_PGP_KEY_FINGERPRINT
   ```
   Public key available at TODO_PGP_KEY_URL.

If you can't use either, contact a maintainer privately on Discord / Slack / Telegram and we'll move the conversation to a secure channel.

## What to include

A useful report has, at minimum:

- ModelForge version (e.g. `v0.0.1`)
- Operating system and architecture
- Steps to reproduce the issue
- Impact — what can an attacker do? What's the worst case?
- Whether you've shared this with anyone else, and if so, whom

If you've written a working proof-of-concept, please share it. If you haven't, no problem — a clear written description is plenty.

## What to expect

- **Acknowledgment within 3 business days.** If you don't hear back, assume the message got lost and try a different channel.
- **Initial assessment within 7 days.** We'll tell you whether we agree it's a vulnerability, what severity we're treating it as, and a target timeline.
- **Fix and disclosure**:
    - **Critical** (RCE, license bypass enabling unauthorized GPU use, secret extraction): aim to ship a patched release within 14 days.
    - **High** (privilege escalation, data exposure across user boundaries): within 30 days.
    - **Medium / Low**: rolled into the next regular release; specific timeline negotiated with reporter.

We'll coordinate disclosure with you. If you want public credit you'll get it; if you prefer to stay anonymous that's fine too.

## Scope

The following are **in scope**:

- The ModelForge binary as distributed through this release repository
- The bundled Python runtime as we package it (vulnerabilities in upstream packages we ship)
- The web UI as served by ModelForge
- The license-enforcement mechanism
- The OpenAI-compatible API surface
- Installation and update flows on Linux, Windows, and macOS

The following are **out of scope** (please don't report these as vulnerabilities — they're known classes of behavior):

- Issues in third-party tooling we don't control (e.g. PyTorch CUDA bugs, Hugging Face Hub outages). Please report those upstream.
- Issues that require physical access to the Machine ModelForge runs on. ModelForge is not designed to be tamper-resistant against its own administrator — license enforcement is intended to deter casual oversharing, not to thwart hardware-level attackers.
- Issues that require a malicious license file you've crafted yourself. The license signature check is the only defense; that's the design.
- Issues exploitable only via training data you supply yourself. ModelForge processes whatever JSONL you feed it; if you train a model to leak its own weights, that's working as configured.
- DOS via massive datasets or oversized model downloads — the platform doesn't impose hard limits on these by design.

## What we don't ask of researchers

- We don't require an NDA before you can report.
- We don't impose embargo periods longer than required for a fix.
- We don't restrict your right to publish your findings after the fix is public.
- We don't run a paid bug bounty (yet — if this changes it'll be announced here).

## Recognition

Reporters of confirmed vulnerabilities are credited in the release notes for the fix, unless they prefer otherwise. A "Security Researchers" page may be added to this repo once we have a few names to list.

Thanks for helping keep ModelForge users safe.