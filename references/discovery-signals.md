# Discovery Signals

Use this file when classifying the repository, collecting facts, or deciding whether a question is important enough to ask.

## Classify The Engagement

Identify both the project state and the repository shape.

Project state candidates:

- greenfield
- active maintenance
- project handoff
- maintenance takeover
- legacy modernization

Repository shape candidates:

- mono-repo
- multi-package workspace
- backend service
- frontend application
- full-stack application
- CLI
- SDK or library
- data pipeline or batch job
- infrastructure or platform repository

## Inspect Repository Facts

Inspect these sources when present:

- repository structure
- package manifests and lockfiles
- dependency management files
- test configuration
- lint and format configuration
- CI/CD files
- deployment files
- environment and secrets templates
- container files
- infrastructure files
- queue, worker, scheduler, or pub/sub configuration
- architecture docs
- API docs
- migration setup
- scripts
- existing governance files

Useful signals include:

- `package.json`, `pnpm-lock.yaml`, `yarn.lock`, `package-lock.json`
- `pyproject.toml`, `requirements*.txt`, `poetry.lock`, `uv.lock`
- `go.mod`, `Cargo.toml`, `pom.xml`, `build.gradle*`
- `Dockerfile`, `docker-compose*`, `compose.yaml`
- `.github/workflows/*`, Azure Pipelines, GitLab CI, Jenkins, CircleCI
- `README*`, `docs/*`, `mkdocs.yml`, `docusaurus`, `storybook`
- test directories, fixtures, and coverage setup
- worker entrypoints, consumer handlers, broker clients, and retry or dead-letter configuration
- migration tools and schema folders
- public API definitions and SDK packaging clues
- CLI entrypoints and release scripts

## Detect Governance Files

Check for:

- `AGENTS.md`
- `CLAUDE.md`
- `.cursor/rules/*`
- `.cursor/rules/*.mdc`
- `.trae/rules/*`
- `CONVENTIONS.md`
- `.aider.conf.yml`
- `.aiderignore`

If any exist:

- list the paths
- explain likely purpose briefly
- note likely overlap or conflict with `AGENTS.md`
- ask whether bridge files should be created or updated
- do not overwrite without confirmation

If multiple `AGENTS.md` files exist, explain that they may have different scopes and should be reconciled deliberately.

## Ask Only Material Questions

Ask only when the repository cannot answer the issue and the answer materially affects governance:

1. What is the project path or repository root?
2. Is this a new project, existing-project maintenance, handoff, or legacy modernization?
3. What is the application scenario?
4. What is the target scale?
5. Are there required language, framework, database, cloud, or architecture constraints?
6. Are there special constraints such as HA, low latency, compliance, privacy, cost, team size, or delivery deadline?
7. Does the system use message queues, background workers, or pub/sub flows whose retry, timeout, re-enqueue, or idempotency policy is not obvious from the repository?
8. If `AGENTS.md` already exists, should it be modified, replaced, or rewritten as a new draft?

Do not ask questions the user already answered or that can be inferred safely from the repository.
