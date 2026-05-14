# seif-provenance

Skill that signs an AI-generated code diff and returns a public verify URL.

See [SKILL.md](SKILL.md) for the canonical spec (frontmatter + usage).

## Why

When an AI assistant writes code, there's no trail tying that specific diff to that specific session at that specific time. `seif-provenance` adds one: sign the diff with the engine's Ed25519 key, get back a shareable evidence page anyone can audit.

## End-to-end flow

```
┌────────────────┐    diff    ┌──────────────────────┐
│  Claude Code   │──────────▶│  api.seifprotocol... │
│   / Cursor     │            │       /v1/sign       │
└────────────────┘            └──────────┬───────────┘
                                         │ {evidence_id, signature}
                                         ▼
                              ┌──────────────────────┐
                              │ verify.seifprotocol  │
                              │ .com/v1/evidence/<id>│
                              │ → green/red badge    │
                              └──────────────────────┘
```

## Install

### Claude Code

```bash
npx skills add seif-provenance
```

### Cursor

Settings → Skills → Install `seif-provenance`.

## First sign

```bash
export SEIF_API_KEY=sk_seif_...
# in your AI assistant chat:
> sign this diff
```

The assistant invokes the skill, posts the diff, and replies with a URL. Open the URL in any browser.

## Demo

A 60-second walkthrough GIF is planned for v0.1.1.

## Requirements

- An AI assistant that can run skills (Claude Code, Cursor).
- A `SEIF_API_KEY` from https://seifprotocol.com.
- Outbound HTTPS to `api.seifprotocol.com`.

## License

MIT.
