---
name: seif-provenance
version: 0.1.0
description: Sign a code diff and produce a public, tamper-detectable verify URL.
homepage: https://seifprotocol.com
license: MIT
keywords:
  - provenance
  - signing
  - evidence
  - audit
  - ai-generated-code
---

# seif-provenance

Sign code emitted by Claude Code, Cursor, or any AI assistant. Returns a public shareable URL with tamper detection.

## Usage

After installing the skill, invoke it on any diff. The skill will:

1. Read the current diff (staged + working tree, or a specified file).
2. POST it to `https://api.seifprotocol.com/v1/sign` with the required headers (see [Headers](#headers)).
3. Return a workspace-namespaced shareable URL like `https://verify.seifprotocol.com/{user}/{workspace}/v/{evidence_id}` (per protocol-spec-v0 amendment-002 / §7). The short form `https://verify.seifprotocol.com/v/{evidence_id}` (without user/workspace) also works for social-card sharing.

Open the URL — anyone (no auth) sees a verify page with a green badge. Tamper with the underlying diff and the badge turns red on next load.

## What it does

POSTs `{diff, model, session_id, metadata?}` to `api.seifprotocol.com/v1/sign` and surfaces the returned `signed_url` in the chat. The signature is Ed25519 over the canonical JSON of the payload. The Ed25519 public key is returned inline in the response (`public_key` field) so consumers can verify offline without a separate roundtrip.

The returned `signed_url` follows the namespaced verify URL form `https://verify.seifprotocol.com/{user}/{workspace}/v/{evidence_id}` (protocol-spec-v0 §4.3 + amendment-002). The unnamespaced short form `/v/{evidence_id}` is accepted by the verify suite for sharing.

Use it when you want to:

- Prove a diff came from a specific AI session at a specific timestamp.
- Share evidence with a reviewer who has no SEIF account.
- Embed a verify badge in a PR description.

## Auth

```bash
export SEIF_API_KEY=sk_seif_...
```

During v0.1 (design-partner phase), API keys are invite-only. Join the waitlist at https://seifprotocol.com/waitlist.

## Headers

Per protocol-spec-v0 §2.3, every authenticated request emitted by this skill MUST carry the following HTTP headers:

| Header | Required | Example | Purpose |
|---|---|---|---|
| `Authorization` | yes (every request) | `Bearer sk_seif_...` | Auth context (api key, oauth session, or magic-link session) per §2.3. |
| `X-Seif-Client` | yes (every POST/GET) | `seif-skill/0.1.0` | Client identifier so the engine can attribute requests and emit per-client observability per §2.3. Format: `seif-skill/<version>` (use this skill's `version` from the front-matter). |
| `X-Seif-Idempotency-Key` | yes on `POST /v1/sign` | `<uuid-v4>` | Client-generated UUID enabling safe retry per §2.5. Reuse the same key when retrying after a network error to avoid duplicate signs. |

Example minimal `POST /v1/sign` request the skill should emit:

```http
POST /v1/sign HTTP/1.1
Host: api.seifprotocol.com
Authorization: Bearer sk_seif_...
X-Seif-Client: seif-skill/0.1.0
X-Seif-Idempotency-Key: 550e8400-e29b-41d4-a716-446655440000
Content-Type: application/json

{ "artifact": { ... }, "classification": "INTERNAL" }
```

## Configuration

| Variable | Required | Default | Purpose |
|---|---|---|---|
| `SEIF_API_KEY` | yes | — | Authenticates the POST to `/v1/sign` (sent as `Authorization: Bearer <key>`). |
| `SEIF_API_URL` | no | `https://api.seifprotocol.com` | Override for self-hosted engines. |

## Output format

`POST /v1/sign` returns `201 Created` with:

```json
{
  "evidence_id": "ev_<16-hex>",
  "signature": "<base64-ed25519>",
  "public_key": "<base64-ed25519-pubkey>",
  "signed_at": "2026-05-14T20:00:00Z",
  "signed_url": "https://verify.seifprotocol.com/{user}/{workspace}/v/ev_<16-hex>",
  "integrity_hash": "<sha256-hex>"
}
```

The `evidence_id` is derived from the first 16 hex chars of the SHA-256 over the canonical payload — same diff produces the same id.

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| `401 Unauthorized` | `SEIF_API_KEY` missing or revoked. |
| `429 Rate limited` | Too many signs in short window — engine enforces a sliding cap. Retry after ~30s. |
| Verify page shows red badge | The diff content on the server no longer matches the signature. Either someone edited it, or the signing payload differed from the rendered payload — re-sign. |

## License

MIT.
