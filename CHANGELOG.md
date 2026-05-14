# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] — 2026-05-14

### Added

- Initial public release of `seif-skills`.
- `seif-provenance` skill (`skills/seif-provenance/SKILL.md`) — signs a code diff via `POST https://api.seifprotocol.com/v1/sign` and returns a shareable verify URL at `https://verify.seifprotocol.com/v1/evidence/<id>`.
- Vercel-compatible frontmatter (`name`, `version`, `description`, `homepage`, `license`, `keywords`).
- Ed25519 signing surface; public key returned inline in `/v1/sign` response.
- MIT license.
- Example payload + curl walkthrough under `examples/`.
- Contributor guide.

### Status

- API keys are invite-only during the design-partner phase. Join the waitlist at <https://seifprotocol.com/waitlist>.
- Cursor adapter wrapper is tracked for v0.1.1+ (see [SKILL.md](skills/seif-provenance/SKILL.md) — Cursor section in the install instructions assumes the SKILL.md spec is consumed directly).
