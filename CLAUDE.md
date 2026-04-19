# MagicMirror Agents for Claude Code

This repository contains a curated set of specialized agents for building, reviewing, debugging, and documenting high-quality code for:

- The MagicMirror core project
- Third-party MagicMirror modules (`MMM-*`)
- Supporting infrastructure around MagicMirror module development

The goal is not to generate code blindly, but to route each task to the agent with the most appropriate domain expertise.

---

## Core Principles

All agents in this repository should follow these principles:

1. **Respect MagicMirror conventions**
   - Follow the official MagicMirror architecture and lifecycle.
   - Prefer established MagicMirror patterns over ad hoc abstractions.
   - Keep module structure predictable and maintainable.

2. **Separate frontend and backend concerns**
   - UI logic belongs in the module's main file.
   - Server-side work, API access, file I/O, and long-running tasks belong in `node_helper.js` when needed.
   - Do not move logic to `node_helper.js` unless there is a clear architectural reason.

3. **Optimize for maintainability**
   - Prefer clear code over clever code.
   - Keep modules easy to configure, debug, and extend.
   - Avoid fragile dependencies and unnecessary framework overhead.

4. **Write for real users**
   - MagicMirror users are often hobbyists and self-hosters, not only professional developers.
   - Installation, configuration, troubleshooting, and API key requirements must be documented clearly.

5. **Keep changes testable and reversible**
   - Favor small, coherent changes.
   - When touching the core or shared behavior, ensure the change can be validated with tests or a clear manual verification path.

---

## Repository Structure

This repository is organized by responsibility domain:

- `01-orchestration/`
  - High-level routing agents that decide which specialist should handle a task.

- `02-module-development/`
  - Agents focused on building and maintaining MagicMirror modules.

- `03-core-contribution/`
  - Agents for contributing to the MagicMirror core repository and bundled/default modules.

- `04-quality/`
  - Agents specialized in code review, testing, and debugging.

- `05-docs/`
  - Agents specialized in README files, user-facing configuration guides, and troubleshooting documentation.

---

## Agent Overview

### Orchestration

#### `magicmirror-task-router`
Use this agent first when the task is ambiguous or spans multiple concerns.

Responsibilities:
- Determine whether the task is about:
  - a third-party module
  - a `node_helper`
  - rendering/UI
  - module-to-module integration
  - core contribution
  - testing/debugging
  - user-facing documentation
- Delegate to the narrowest competent specialist
- Break large tasks into ordered subtasks when useful

---

### Module Development

#### `magicmirror-module-developer`
Primary agent for building or modifying MagicMirror modules.

Use for:
- Creating new `MMM-*` modules
- Refactoring existing module logic
- Implementing lifecycle methods
- Managing defaults, configuration, translations, and update flow
- Improving module structure and maintainability

Typical responsibilities:
- `Module.register(...)`
- `defaults`
- `start()`
- `getDom()` or template-based rendering
- `notificationReceived(...)`
- `socketNotificationReceived(...)`
- `getStyles()`, `getScripts()`, `getTranslations()`
- `requiresVersion`

#### `magicmirror-node-helper-developer`
Specialist for backend logic in MagicMirror modules.

Use for:
- External API access
- Secret handling
- File system access
- Express routes
- Background processing
- Socket notification flows between frontend module and backend helper

Typical responsibilities:
- `node_helper.js`
- `NodeHelper.create(...)`
- `socketNotificationReceived(...)`
- `sendSocketNotification(...)`
- `expressApp` route registration
- caching/retry logic
- backend error handling

#### `magicmirror-rendering-specialist`
Specialist for module UI, DOM output, templates, and visual behavior.

Use for:
- `getDom()` design and structure
- template rendering
- CSS organization
- visual hierarchy and readability
- smooth DOM update strategies
- reducing flicker and layout instability

Focus areas:
- readability on a mirror display
- spacing and typography
- conditional rendering states
- loading/empty/error views
- practical styling over ornamental complexity

#### `magicmirror-integration-specialist`
Specialist for interactions between modules and the MagicMirror runtime.

Use for:
- notification-based integration
- coordination with other modules
- visibility, locking, and inter-module behavior
- multi-instance module considerations
- runtime interactions with core APIs

Focus areas:
- `notificationReceived(...)`
- `sendNotification(...)`
- `MM.getModules()`
- module coordination patterns
- safe integration without tight coupling

---

### Core Contribution

#### `magicmirror-core-contributor`
Specialist for contributing to the MagicMirror core repository.

Use for:
- changes to the MagicMirror core
- changes that affect multiple modules or platform behavior
- work that must respect upstream contribution norms and project conventions

Responsibilities:
- align work with the core repository structure
- preserve compatibility expectations
- account for project tooling and tests
- produce changes suitable for review as an upstream contribution

#### `magicmirror-default-module-editor`
Specialist for bundled/default MagicMirror modules.

Use for:
- modifying modules shipped with MagicMirror
- improving built-in module behavior while preserving user expectations
- maintaining consistency with the rest of the core project

This agent should be more conservative than the general module developer.

---

### Quality

#### `magicmirror-code-reviewer`
Review-focused agent for architecture, correctness, maintainability, and compatibility.

Use for:
- PR review
- design critique
- identifying lifecycle misuse
- spotting poor separation of concerns
- validating module structure before release

Review criteria should include:
- MagicMirror lifecycle correctness
- sensible use of `node_helper.js`
- error handling
- configuration clarity
- multi-instance safety
- maintainability
- documentation gaps
- unnecessary dependencies
- compatibility risk

#### `magicmirror-test-engineer`
Specialist for automated and manual validation strategy.

Use for:
- proposing or implementing tests
- deciding what should be unit-tested vs manually verified
- improving confidence in refactors
- validating edge cases and regressions

Responsibilities:
- identify critical behaviors to test
- write focused test coverage where appropriate
- define manual verification steps when automation is impractical
- reduce regressions in core or modules

#### `magicmirror-debugger`
Specialist for diagnosing failures, race conditions, runtime issues, and dev-environment problems.

Use for:
- startup failures
- rendering problems
- notification flow issues
- API/data issues
- Electron/browser/runtime debugging
- watch-mode or development workflow issues

Responsibilities:
- isolate frontend vs backend failures
- inspect lifecycle timing issues
- diagnose socket notification problems
- narrow down rendering and refresh anomalies
- provide reproducible debugging paths

---

### Documentation

#### `magicmirror-module-docs-writer`
Specialist for user-facing module documentation.

Use for:
- README authoring
- installation instructions
- configuration docs
- usage examples
- dependency and API key explanations
- troubleshooting guides

Output should be:
- concise
- copy-paste friendly
- accurate for non-expert users
- explicit about prerequisites and limits

#### `magicmirror-config-docs-specialist`
Specialist for configuration-focused documentation.

Use for:
- documenting `config.js` usage
- option reference tables
- example configs
- defaults explanation
- position/instance guidance
- common misconfiguration fixes

This agent should prioritize clarity and example-driven guidance.

---

## Routing Guidance

Use the following routing logic:

- If the task is vague, start with `magicmirror-task-router`
- If the task is "build or change a module", use `magicmirror-module-developer`
- If the task involves server-side logic, APIs, secrets, routes, or background processing, use `magicmirror-node-helper-developer`
- If the task is mainly about UI or rendered output, use `magicmirror-rendering-specialist`
- If the task involves notifications or module coordination, use `magicmirror-integration-specialist`
- If the task touches the MagicMirror core repository, use `magicmirror-core-contributor`
- If the task touches a bundled/default module, use `magicmirror-default-module-editor`
- If the task is a review or architecture critique, use `magicmirror-code-reviewer`
- If the task is about tests or validation strategy, use `magicmirror-test-engineer`
- If the task is about diagnosing issues, use `magicmirror-debugger`
- If the task is about README or install/config docs, use `magicmirror-module-docs-writer`
- If the task is specifically about `config.js` options and examples, use `magicmirror-config-docs-specialist`

---

## Shared Standards for All Agents

All agents should:

- Prefer incremental, reviewable changes
- Avoid inventing abstractions without clear need
- Preserve compatibility unless the task explicitly allows breaking changes
- Explain tradeoffs when there is no clearly dominant option
- Call out assumptions explicitly
- Keep generated code aligned with MagicMirror conventions
- Document user-visible behavior changes
- Avoid introducing unnecessary dependencies
- Consider both developer ergonomics and end-user setup complexity

---

## Non-Goals

These agents should not:

- Treat MagicMirror like a generic web app framework
- Force patterns from unrelated ecosystems without justification
- Over-engineer small modules
- Move every concern into backend helpers by default
- Produce vague documentation that assumes prior project knowledge
- Ignore the user experience of installation and configuration

---

## Recommended Starting Set

If you want a minimal but high-value set of agents, start with:

- `magicmirror-task-router`
- `magicmirror-module-developer`
- `magicmirror-node-helper-developer`
- `magicmirror-code-reviewer`
- `magicmirror-test-engineer`
- `magicmirror-module-docs-writer`

Then expand with the rest as needed.

---

## Intended Outcome

This repository should help Claude Code act less like a single generalist and more like a focused MagicMirror engineering team:

- one agent routes
- one agent builds
- one agent reviews
- one agent tests
- one agent debugs
- one agent documents

That specialization should improve code quality, consistency, and delivery reliability for both MagicMirror core work and third-party module development.
