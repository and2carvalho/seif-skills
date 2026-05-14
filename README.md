# seif-skills

Skills that sign AI-generated code and produce public, tamper-detectable verify URLs.

Part of the [SEIF Protocol](https://seifprotocol.com) provenance plane — give every diff a shareable, cryptographically signed evidence page that anyone can audit.

## Skills

| Skill | Description | Status |
|---|---|---|
| [`seif-provenance`](skills/seif-provenance/) | Sign a code diff, get a public verify URL | v0.1 |

## Quick start

Install via the Vercel skills CLI (works across Claude Code, Cursor, Codex CLI, Gemini CLI, and 18+ other agents — `SKILL.md` is cross-agent):

```bash
npx skills add and2carvalho/seif-skills
```

To pin a specific skill from this repo:

```bash
npx skills add and2carvalho/seif-skills --skill seif-provenance
```

> Name-only resolution (`npx skills add seif-provenance`) requires publication to the [skills.sh](https://skills.sh) directory — planned for v0.1.1. Until then, use the `owner/repo` form above; it installs directly from this GitHub repo, no registry needed.

Then set your API key:

```bash
export SEIF_API_KEY=sk_seif_...   # get one at https://seifprotocol.com
```

Invoke the skill on any diff your AI assistant produced — you'll get back a shareable URL like `https://verify.seifprotocol.com/v1/evidence/<evidence_id>` that anyone can open to confirm the signature is intact.

## How it works

1. Skill captures the current diff (staged or working tree).
2. POSTs `{diff, model, session_id}` to `api.seifprotocol.com/v1/sign`.
3. Engine returns `{evidence_id, signature, public_key, signed_at, signed_url, integrity_hash}` (HTTP 201).
4. Anyone with the URL can open it — tamper with one byte and the badge turns red.

The signature is Ed25519 over the canonical JSON of the payload. The Ed25519 public key is returned inline in every `/v1/sign` response so consumers can verify offline.

## License

MIT. See [LICENSE](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
