# MagicMirror Claude Code Agents

A curated set of specialized Claude Code agents for building, reviewing, debugging, and documenting high-quality code for:

- The [MagicMirror core project](https://github.com/MagicMirrorOrg/MagicMirror)
- Third-party MagicMirror modules (`MMM-*`)
- Supporting tooling and documentation around MagicMirror development

This repository is designed to make Claude Code behave less like a single generalist and more like a focused MagicMirror development team with clearly separated responsibilities.

---

## Why this repository exists

MagicMirror development has a few recurring architectural boundaries:

- **Frontend module code** vs **backend `node_helper.js`**
- **Rendering/UI work** vs **data fetching/integration logic**
- **Core contributions** vs **third-party module development**
- **Developer-oriented implementation** vs **end-user documentation**

A single general-purpose agent can handle many of these tasks, but quality improves when the task is routed to a specialist that already understands the relevant patterns and tradeoffs.

This repository provides that specialization.

---

## Goals

The agents in this repository are intended to help with:

- Building new MagicMirror modules
- Refactoring existing `MMM-*` modules
- Implementing or improving `node_helper.js`
- Improving module rendering and user experience
- Reviewing code for architectural correctness and maintainability
- Writing tests and validation plans
- Debugging runtime, lifecycle, and integration issues
- Writing user-facing READMEs and configuration docs
- Contributing safely to the MagicMirror core project

---

## Non-goals

This repository is **not** intended to:

- Turn MagicMirror development into an over-engineered framework
- Force patterns from unrelated ecosystems without justification
- Replace official MagicMirror documentation
- Encourage unnecessary abstractions or dependencies
- Blur the separation between frontend module code and backend helpers
- Produce vague documentation that assumes advanced prior knowledge

---

## Repository structure

```text
.
├── CLAUDE.md
├── README.md
├── 01-orchestration/
│   └── task-router.md
├── 02-module-development/
│   ├── module-developer.md
│   ├── node-helper-developer.md
│   ├── rendering-specialist.md
│   └── integration-specialist.md
├── 03-core-contribution/
│   ├── core-contributor.md
│   └── default-module-editor.md
├── 04-quality/
│   ├── code-reviewer.md
│   ├── test-engineer.md
│   └── debugger.md
└── 05-docs/
    ├── module-docs-writer.md
    └── config-docs-specialist.md
```

---

## Agent catalog

## 01. Orchestration

### `magicmirror-task-router`
Use this agent when the task is ambiguous, broad, or likely spans multiple concerns.

It is responsible for:
- identifying the real task type
- delegating to the correct specialist
- breaking large work into smaller coherent subtasks
- preventing the wrong specialist from owning the entire problem

Typical use cases:
- “Build a new module with an API backend and a polished README”
- “This module is failing, and I am not sure whether the issue is rendering, config, or helper logic”
- “I want to contribute a fix, but I don’t know if it belongs in the core or a module”

---

## 02. Module development

### `magicmirror-module-developer`
Primary implementation agent for MagicMirror modules.

Use this agent for:
- creating new `MMM-*` modules
- modifying lifecycle logic
- handling defaults, translations, notifications, and update flow
- improving module organization and maintainability

This agent should understand:
- `Module.register(...)`
- `defaults`
- `start()`
- `getDom()`
- `getTemplate()`
- `getTemplateData()`
- `getStyles()`
- `getScripts()`
- `getTranslations()`
- `notificationReceived(...)`
- `socketNotificationReceived(...)`
- `requiresVersion`

### `magicmirror-node-helper-developer`
Backend specialist for MagicMirror modules.

Use this agent for:
- API integrations
- file I/O
- Express routes
- secret handling
- data caching
- background work
- socket notification flow between module and helper

This agent should own:
- `node_helper.js`
- `NodeHelper.create(...)`
- `socketNotificationReceived(...)`
- `sendSocketNotification(...)`
- route registration
- backend retries, failures, and diagnostics

### `magicmirror-rendering-specialist`
UI and rendering specialist for MagicMirror modules.

Use this agent for:
- designing `getDom()` output
- template-driven rendering
- CSS styling
- readability on mirror displays
- stable update behavior
- loading, empty, and error states
- reducing flicker and visual clutter

### `magicmirror-integration-specialist`
Specialist for module-to-module and module-to-core runtime integration.

Use this agent for:
- inter-module notifications
- runtime coordination patterns
- multi-instance behavior
- visibility or locking interactions
- safe coupling with other modules

This agent should focus on:
- `sendNotification(...)`
- `notificationReceived(...)`
- `MM.getModules()`
- defensive integration design

---

## 03. Core contribution

### `magicmirror-core-contributor`
Specialist for working on the MagicMirror core repository itself.

Use this agent for:
- changes to core behavior
- changes with wide compatibility implications
- upstream-ready contributions
- work that must align with repository standards, tooling, and review expectations

This agent should be conservative, compatibility-aware, and test-oriented.

### `magicmirror-default-module-editor`
Specialist for bundled/default modules included with MagicMirror.

Use this agent for:
- improving built-in module behavior
- keeping changes aligned with the rest of the core ecosystem
- making conservative improvements with low surprise for users

This agent is narrower and more cautious than the generic module developer.

---

## 04. Quality

### `magicmirror-code-reviewer`
Architecture and maintainability reviewer.

Use this agent for:
- reviewing pull requests
- validating lifecycle correctness
- catching separation-of-concerns violations
- identifying maintainability risks
- spotting documentation and configuration gaps

Typical review areas:
- proper module/helper separation
- compatibility risk
- unnecessary dependencies
- unclear defaults or config contracts
- weak error handling
- brittle integration logic
- multi-instance safety

### `magicmirror-test-engineer`
Testing and validation specialist.

Use this agent for:
- designing a testing strategy
- writing or improving tests
- deciding when manual verification is more realistic than automation
- reducing regressions during refactors or core changes

### `magicmirror-debugger`
Runtime debugging specialist.

Use this agent for:
- startup failures
- rendering anomalies
- stale or missing data
- broken notification flow
- helper communication problems
- race conditions or lifecycle timing issues
- local development workflow issues

---

## 05. Documentation

### `magicmirror-module-docs-writer`
User-facing documentation specialist.

Use this agent for:
- README creation
- installation steps
- configuration guides
- API key setup notes
- screenshots section drafting
- troubleshooting guidance
- upgrade notes and behavior changes

The writing style should be:
- practical
- explicit
- copy-paste friendly
- accessible to non-expert users

### `magicmirror-config-docs-specialist`
Configuration documentation specialist.

Use this agent for:
- documenting `config.js` options
- writing defaults tables
- creating working configuration examples
- clarifying positions, instances, and common setup mistakes
- documenting configuration migrations

---

## Recommended starting set

If you want the smallest useful version of this repository, start with:

- `magicmirror-task-router`
- `magicmirror-module-developer`
- `magicmirror-node-helper-developer`
- `magicmirror-code-reviewer`
- `magicmirror-test-engineer`
- `magicmirror-module-docs-writer`

That set covers the majority of real module-development workflows.

---

## How to use these agents

A practical routing pattern:

1. Start with `magicmirror-task-router` if the task is unclear or mixes concerns.
2. Delegate implementation to `magicmirror-module-developer` or `magicmirror-node-helper-developer`.
3. Use `magicmirror-rendering-specialist` when the problem is mostly UI or DOM structure.
4. Use `magicmirror-integration-specialist` when notifications or coordination with other modules matter.
5. Send the result through `magicmirror-code-reviewer`.
6. Validate changes with `magicmirror-test-engineer`.
7. Finish with `magicmirror-module-docs-writer` or `magicmirror-config-docs-specialist`.

This produces a cleaner workflow than asking one agent to do everything at once.

---

## Design philosophy

These agents should consistently prefer:

- clear code over clever code
- maintainability over novelty
- predictable module structure over abstraction for its own sake
- explicit documentation over implicit assumptions
- conservative compatibility decisions
- small, verifiable changes

The objective is not just to generate code that works once, but code that remains understandable and supportable.

---

## Suggested use cases

Examples of tasks that fit this repository well:

- “Create a new `MMM-*` weather dashboard module with a `node_helper.js` for API calls.”
- “Refactor this module to stop mixing backend HTTP calls into the frontend file.”
- “Review this module and tell me whether the lifecycle is being used correctly.”
- “Improve the README so non-technical users can install and configure it.”
- “Diagnose why this module updates once but never refreshes again.”
- “Prepare a core-compatible patch for MagicMirror itself.”
- “Document every config option in a copy-paste-friendly format.”

---

## Intended outcome

This repository should help Claude Code produce better MagicMirror results by assigning tasks to agents that already understand the constraints of the platform.

The intended result is a workflow where:
- one agent routes
- one agent builds
- one agent reviews
- one agent tests
- one agent debugs
- one agent documents

That specialization should improve quality, consistency, and delivery reliability for both MagicMirror core work and third-party module development.
