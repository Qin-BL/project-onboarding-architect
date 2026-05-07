# Rule Modules

Use this file when deciding which sections to include in `AGENTS.md`. Include only modules that match the detected repository type, scale, and confirmed constraints.

## Lightweight Default Modules

These usually belong in a concise `AGENTS.md` when they reflect the actual repository:

- repository scope
- stack and package-manager facts
- change policy
- docs, comments, and final response expectations

Keep these sections short and tied to real project conventions.

## Conditional Modules

### Architecture And Boundaries

Include when the repository already has meaningful architectural boundaries, or when the project is large enough that the user wants explicit structure.

Do not force heavy layering into every repository. If the project lacks clear structure but would benefit from stronger boundaries, explain the recommendation and ask before enforcing it.

### Dependency Policy

Include when the repository has an established package manager, vendoring rule, security policy, or strong preference against new dependencies.

Keep it practical: prefer "avoid unnecessary new dependencies" over a long procurement policy.

### Testing Policy

Include when tests already exist or the user wants a stronger validation standard.

Common expectations:

- align with the existing test framework and fixtures
- add or update tests for behavior changes
- add complete positive and negative cases when creating or expanding a test module
- add regression coverage for bug fixes when practical
- treat a partially covered test module as incomplete work and extend it until changed code paths and meaningful branches are covered
- do not stop at the happy path when validation, error handling, permissions, edge cases, or fallback logic materially affect behavior
- do not weaken or skip tests just to pass

If the repository has no testing framework, describe the current state and ask before imposing a heavy new policy.

### CI/CD And Validation Policy

Include when the repository has CI workflows, deployment gates, or standard validation commands.

Usually state that local validation is expected when feasible and that CI/CD is the final source of truth when it exists.

Common expectations:

- run the closest local equivalent of the real CI stages when feasible instead of stopping at narrow unit tests
- name the actual lint, format, typecheck, packaging, migration, or deploy-preflight commands that mirror CI/CD gates
- explain that passing focused tests is not enough when CI/CD also enforces formatting, hooks, schema checks, or artifact/build validation
- if an integration-branch fix is required to satisfy CI/CD, check whether source branches or open PR branches need the same fix backported

### API, SDK, CLI, Or Compatibility Policy

Include only when the repository exposes public APIs, SDKs, CLI contracts, shared UI components, integration contracts, or other external consumer surfaces.

Backward compatibility is often the production default, but it should not be enforced blindly. Explain the benefit and cost, then ask before writing it as a strong rule.

### Migration And Data Safety Policy

Include when the repository owns schema changes, data migrations, state transitions, or replay-sensitive jobs.

Focus on rollout safety, reversibility, data integrity, and operational caution rather than generic database advice.

Common expectations:

- validate migration graph or migration plan health after rebases, branch merges, or concurrent schema work
- catch deploy-only failures such as conflicting migration leaves before declaring the work complete
- prefer explicit merge migrations or the migration tool's equivalent when already-shared branches create parallel leaves

### Message Queue And Async Job Safety

Include when the repository uses message queues, pub/sub, background workers, scheduled async jobs, or other delivery models where work may be retried or redelivered.

Common expectations:

- make message and job handlers idempotent so retries or redelivery do not create duplicate side effects
- define acknowledgement, timeout, retry, and failure behavior explicitly instead of assuming the default broker behavior is safe
- re-enqueue or retry transient failures and no-response cases when the delivery semantics and business workflow require another attempt
- separate transient failures from permanent failures so poison messages are not retried forever
- use deduplication keys, idempotency keys, optimistic state checks, or equivalent guards when the side effects are externally visible or expensive
- describe dead-letter, poison-message, or operator-escalation handling when it materially affects safety

Do not add queue-specific rules to repositories that do not actually use async job delivery or message brokers.

### Operational Safety And Observability

Include when the repository runs services, jobs, or infrastructure where auth, authorization, logging, retry behavior, metrics, alerting, rollback, or failure handling matter.

Do not add service-ops policy to a simple local-only tool unless it truly applies.

### Documentation And Comment Policy

Include lightly in most repositories.

Prefer rules such as:

- update docs when behavior, config, deployment, or developer workflow changes
- add comments for non-obvious logic
- prefer `why` comments over `what` comments
- avoid noise comments

## Repo-Type Guidance

Use these examples as selection hints, not as mandatory templates:

- backend service: API, auth, validation, migration, observability, deployment, rollback
- frontend app: UI boundaries, state/data fetching patterns, accessibility, build and test workflow
- full-stack app: split frontend and backend rules only when both surfaces matter materially
- CLI: command contract stability, config files, output compatibility, release discipline
- SDK or library: public API compatibility, versioning, examples, release discipline, test matrix
- data job: idempotency, replay, checkpoints, data quality, recovery, resource control
- queue-driven worker or service: idempotency, retry policy, re-enqueue behavior, timeout handling, poison-message recovery
- simple internal tool: keep rules light and avoid over-architecture
- high-traffic production service: strengthen compatibility, observability, rollback, and concurrency guidance

## Exclusion Rule

Do not include backend-only, frontend-only, database-only, HA-only, or public-API-only sections unless they actually apply to the repository.

When unsure, either ask the user or mark the rule as optional instead of enforcing it.
