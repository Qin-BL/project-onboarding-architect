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
- When the repository has tests or the user wants stronger validation, include a testing rule that requires complete positive and negative coverage for each added or changed test module.
- When the repository's tests are present but incomplete, include a rule that future agents must extend the test module until the changed code paths and meaningful branches are covered end to end.
- When the repository uses message queues, pub/sub, or background workers, include concrete rules for idempotent job handling, safe retries, and re-enqueue behavior for failures or missing responses when the delivery model requires it.
- When the repository has CI/CD, include a rule that future agents must validate against the closest local equivalent of the real CI/CD stages when feasible instead of stopping at narrow unit tests.
- When the repository uses schema migrations, include a rule that future agents must validate migration graph or plan health after merge-sensitive changes so deploy-only failures are caught before handoff.
- When the repository syncs data from external systems or derives downstream state from sync results, include rules that future agents must validate the full sync chain: FK rebinding, event emission, attachment or relation binding, derived records, and async follow-up work, not only the primary upsert.
- When the repository relies on created-event hooks or asynchronous initialization, include rules that future agents should prefer idempotent, eligibility-based initialization over one-time creation timing when upstream data may arrive late.
- When the repository has meaningful auth, session, admin, or operator surfaces, include rules that future agents must review reachable deprecated endpoints, token revocation behavior, and whether internal operational tools are exposed publicly.

Read [references/rule-modules.md](references/rule-modules.md) when choosing which sections to include or exclude from `AGENTS.md`.

## Confirmation Before Writing

Before generating the final `AGENTS.md`, provide a concise confirmation summary with:

1. detected project type
2. detected stack and package manager
3. detected tests, linting, and CI/CD
4. whether the repository has public or external contracts
5. detected architecture style or notable lack of structure
6. detected async job or message-queue patterns, including retry, timeout, acknowledgement, and re-enqueue risks when relevant
7. detected test gaps, including whether current tests miss positive cases, negative cases, or meaningful branches
8. rule modules proposed for inclusion
9. rule modules proposed for exclusion
10. decisions that still need confirmation

Do not generate the final `AGENTS.md` until material policy questions are resolved.

Material policy questions often include:

- public API or SDK backward compatibility
- whether to enforce architecture boundaries
- whether to create or update bridge files
- whether to replace or revise an existing `AGENTS.md`
- whether HA, concurrency, migration, or operational rules should be strong or optional
- whether queue-consumer retry, re-enqueue, timeout, and idempotency rules should be strong or optional

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

If the repository uses tests, make the testing expectations concrete. Prefer rules such as:

- add both positive and negative tests when creating or expanding a test module
- treat incomplete test modules as unfinished work and extend them before closing the task
- cover changed code paths and meaningful branches, not only the happy path
- do not claim completion while obvious uncovered logic remains

If the repository uses message queues, pub/sub, or background workers, make the operational expectations concrete. Prefer rules such as:

- make queue-driven tasks safe to run more than once by requiring idempotent handlers
- define what happens on task failure, timeout, or no response before acknowledging the work as complete
- re-enqueue or retry transient failures safely when the queue semantics and product requirements call for it
- avoid duplicate side effects on redelivery by using deduplication keys, idempotency keys, or state checks when appropriate
- treat silent skips, empty-result branches, and early returns as high-risk paths when they can accidentally convert retryable work into a completed terminal state

If the repository has CI/CD or deployment automation, prefer rules such as:

- run the closest local equivalent of the real CI commands when feasible instead of assuming focused tests are sufficient
- name the exact lint, format, typecheck, packaging, migration, or deploy-preflight commands that mirror CI/CD gates
- treat CI/CD as the final source of truth for integration safety and deployment safety when it exists
- if a fix is applied on an integration branch because CI/CD failed, check whether the same fix must be backported to source branches or open PR branches

If the repository uses schema migrations, prefer rules such as:

- check migration graph or migration plan health after rebases, branch merges, or concurrent schema work
- catch deployment-only failures such as multiple migration leaf nodes before declaring the work complete
- prefer an explicit merge migration or tool-specific equivalent when already-shared branches create parallel migration leaves
- distinguish between private branch conflicts and already-shared or already-deployed migration paths; a linear rewrite is safer before sharing, while deployed paths need an explicit environment transition plan
- when an integration branch auto-deploys to test or staging, validate migration-history changes against both fresh databases and databases that already applied the old path
- when replacing a deployed migration path, include rollout compatibility steps such as migration-record cleanup, fake migration transitions, or equivalent deploy-time guardrails

If the repository syncs external data or fans sync results into downstream workflows, prefer rules such as:

- validate the full sync chain instead of only the main table write: foreign keys, attachments, events, derived objects, and downstream async jobs
- prefer idempotent, eligibility-based initialization when related upstream data can arrive after the first create event
- make sidecar refresh jobs, backfills, and incremental tasks perform the same post-processing as the full sync path when they touch the same entities
- avoid fragile gating on filename strings or similar incidental values when stronger business identifiers or actual processing capability exist

If the repository has auth-sensitive or operator-only surfaces, prefer rules such as:

- treat deprecated but reachable endpoints as live attack surface until they are removed or protected
- require refresh-token rotation and server-side revocation guidance when session-bearing tokens are used
- keep internal dashboards, worker control planes, and maintenance tools off the public internet by default; prefer private network access plus front-door authentication

## Output Contract

Produce work in this order unless the user asks for something else:

1. discovery summary
2. open questions or assumptions
3. proposed rule modules to include and exclude
4. testing-policy recommendation, including whether positive cases, negative cases, and missing coverage must be added
5. async-processing recommendation, including whether retry, re-enqueue, timeout, and idempotency rules must be added
6. `AGENTS.md` draft or revision plan
7. bridge-file recommendation only if relevant

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
