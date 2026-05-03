# Bridge Files

Use this file only when the repository already contains tool-specific AI rule files or the user wants one shared governance source across multiple AI coding tools.

## Core Policy

Treat `AGENTS.md` as the source of truth.

Suggested mapping:

- `AGENTS.md`: main rule file, tool-agnostic, source of truth
- `CLAUDE.md`: optional Claude Code entrypoint
- `.cursor/rules/*`: optional Cursor bridge
- `.trae/rules/*`: optional Trae bridge
- `CONVENTIONS.md`: optional human-readable supplement

## Bridge-File Rules

Bridge files must:

- stay short
- point back to `AGENTS.md`
- avoid copying large sections of `AGENTS.md`
- contain only minimal tool-specific entry guidance
- avoid long-term rule divergence

Inspect existing bridge files before touching them. Do not overwrite them without confirmation.

## Minimal Bridge Guidance

When creating a bridge file:

- state that `AGENTS.md` is the source of truth
- instruct the tool or reader to read and follow `AGENTS.md`
- keep any extra tool-specific note minimal

Minimal example text:

`This repository uses AGENTS.md as the source of truth. Read and follow AGENTS.md before making changes.`

## Format Guidance

Keep `AGENTS.md` itself in plain, portable Markdown.

If diagram syntax matters:

- follow the repository's existing renderer if one is already established
- otherwise prefer Mermaid syntax that the target environment can render reliably
- avoid relying on one tool's private Markdown extensions inside the shared source of truth
