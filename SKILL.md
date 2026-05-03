---
name: project-onboarding-architect
description: Audit a repository at project start, project handoff, maintenance takeover, or legacy modernization time, then generate or update a concise project-specific `AGENTS.md` for future AI coding agents across tools such as Codex, Claude Code, Cursor, Trae, Aider, and similar agents. Use when the goal is project onboarding, repository governance, AI agent alignment, or creating one tool-agnostic source of truth before substantial implementation work begins.
---

# Project Onboarding Architect

Audit the repository before major implementation work. Discover the real project shape, identify material risks, and generate or update a concise, repository-specific, tool-agnostic `AGENTS.md`.

Use this skill for project initialization, project handoff, governance, and `AGENTS.md` design. Do not use it as a general feature-delivery skill.

## Core Goal

Produce a repository-specific operating guide for future AI coding agents.

Keep the generated `AGENTS.md`:

- concise
- clear
- actionable
- tool-agnostic
- repository-specific

Prefer short, high-signal rules over generic best-practice dumps. Mark unknown facts as `Unknown` or `Assumed`; do not invent them.

## Working Style

- Read the repository first.
- Infer what can be inferred before asking questions.
- Ask only for information that materially affects architecture, compatibility, security, dependency policy, CI/CD, scale, or governance.
- Treat best practices as defaults, not automatic mandates.
- Preserve working project conventions unless the user wants deliberate change.
- Explain "works today but not ideal" findings before recommending tighter governance.
- Do not silently upgrade architecture, compatibility, HA, migration, or policy strictness.
- Write `AGENTS.md` in plain Markdown and keep the core rules tool-agnostic.

Use language such as:

- `AI coding agents must...`
- `Future coding agents must...`
- `When using AI coding tools...`

Avoid tool-bound rules such as:

- `Codex must...`
- `Claude must...`
- `Cursor must...`
- `Trae must...`

## Discovery Workflow

Follow this order:

1. Classify the repository and the current engagement.
2. Inspect repository facts before proposing rules.
3. Detect existing governance and tool-specific rule files.
4. Summarize findings, unknowns, and decisions that need confirmation.
5. Draft or update `AGENTS.md` only after material policy questions are resolved.

Read [references/discovery-signals.md](references/discovery-signals.md) when you need exact files, heuristics, or low-noise question prompts.

## Existing Governance Files

Check for existing `AGENTS.md` and tool-specific rule files before writing anything.

If one or more `AGENTS.md` files exist:

- show the paths
- explain likely scope when possible
- summarize the current content briefly
- ask whether to modify, replace, or create a new draft
- never overwrite without explicit confirmation

If other rule files exist, list them, explain likely overlap, and ask whether bridge files should be created or updated.

Read [references/bridge-files.md](references/bridge-files.md) only when the repository already uses multiple AI coding tools or the user asks for bridge files.

## Rule Selection

Include only rule modules that fit the detected repository type, maturity, and user-confirmed constraints.

- Keep rules light for simple internal tools and prototypes.
- Strengthen safety, compatibility, observability, migration, or rollback guidance only when the repository actually needs it.
- Preserve the existing architecture and style unless the user wants a deliberate shift.
- Ask before enforcing strong compatibility or architecture rules.
- Ask before adding heavy process to a repository that currently succeeds with a lighter workflow.

Read [references/rule-modules.md](references/rule-modules.md) when choosing which sections to include or exclude from `AGENTS.md`.

## Confirmation Before Writing

Before generating the final `AGENTS.md`, provide a concise confirmation summary with:

1. detected project type
2. detected stack and package manager
3. detected tests, linting, and CI/CD
4. whether the repository has public or external contracts
5. detected architecture style or notable lack of structure
6. rule modules proposed for inclusion
7. rule modules proposed for exclusion
8. decisions that still need confirmation

Do not generate the final `AGENTS.md` until material policy questions are resolved.

Material policy questions often include:

- public API or SDK backward compatibility
- whether to enforce architecture boundaries
- whether to create or update bridge files
- whether to replace or revise an existing `AGENTS.md`
- whether HA, concurrency, migration, or operational rules should be strong or optional

## Drafting `AGENTS.md`

When writing `AGENTS.md`:

- keep it short enough to be read before work starts
- include only repository-relevant rules and defaults
- prefer concrete rules tied to the detected stack, workflow, and risk profile
- preserve existing conventions unless the user approves change
- state assumptions explicitly
- keep the main file tool-agnostic even if bridge files exist
- avoid copying large policy blocks into multiple files

Default structure:

1. repository scope
2. stack and package-manager facts
3. architecture and change boundaries
4. validation and testing expectations
5. docs, comments, and response expectations
6. optional project-specific sections that truly apply

## Output Contract

Produce work in this order unless the user asks for something else:

1. discovery summary
2. open questions or assumptions
3. proposed rule modules to include and exclude
4. `AGENTS.md` draft or revision plan
5. bridge-file recommendation only if relevant

If asked to edit files directly and material policy decisions are still open, stop at a draft plus confirmation request instead of overwriting governance files.

## Reference Map

Read extra files only when needed:

- [references/discovery-signals.md](references/discovery-signals.md): repository classification, files to inspect, and low-noise question prompts
- [references/rule-modules.md](references/rule-modules.md): candidate `AGENTS.md` sections, inclusion rules, and repo-type guidance
- [references/bridge-files.md](references/bridge-files.md): multi-tool bridge-file policy and minimal bridge guidance

## Example Triggers

- "Audit this repo and generate a project-specific `AGENTS.md`."
- "We are taking over this legacy system; create governance rules for future AI coding agents."
- "Initialize a new project with a tool-agnostic `AGENTS.md`."
- "Review this repo before implementation starts and update the AI agent rules."
- "Create one shared rules file for Codex, Claude Code, Cursor, Trae, and Aider."
