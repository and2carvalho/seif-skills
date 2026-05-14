# Contributing to seif-skills

Thanks for considering a contribution.

## Scope

This repo hosts skills for the SEIF provenance plane. Each skill lives under `skills/<name>/` and must include a `SKILL.md` with Vercel-compatible frontmatter (name, version, description, license).

## Adding a new skill

1. Open an issue describing the use case before writing code.
2. Branch from `main`.
3. Create `skills/<your-skill>/SKILL.md` following the format of [seif-provenance](skills/seif-provenance/SKILL.md).
4. Add a short README under the same directory.
5. Open a PR. The maintainer (`@and2carvalho`) reviews.

## Skill format

Frontmatter is required and must include at minimum:

```yaml
---
name: <skill-name>
version: <semver>
description: <one sentence>
license: MIT
---
```

Optional but recommended: `homepage`, `keywords`, `repository`.

## Reporting bugs

File an issue with:

- The skill name and version.
- The AI assistant (Claude Code / Cursor / other).
- A minimal repro.

## License

By contributing you agree your work is released under the MIT license.
