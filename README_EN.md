# SoloSpec

[English](README_EN.md) | [中文](README.md)

**Lightweight AI project conventions for solo developers: preserve decisions and essential knowledge while keeping everyday development simple.**

SoloSpec is a collection of Markdown instructions and templates. It does not orchestrate agents, manage processes, maintain a task database, or require a runtime. Its workflow has four steps: read relevant constraints → record when needed → implement and verify → deliver and preserve useful knowledge.

## Getting started

For an existing project, use the integration prompt below to let your agent fetch the template and merge the conventions. For manual adoption, merge the needed content from `AGENTS.md`, `docs/`, and `templates/`; the optional ADR skill lives in `.agents/skills/adr-workflow/`. Read existing files before merging and preserve the project's content.

Once integrated, describe the work you want done. The agent follows the project conventions to read relevant guidelines, implement, and verify. It creates a task record only when work must continue across sessions, involves multiple dependent stages, or you explicitly request a record.

Codex reads `AGENTS.md`; Claude Code imports the same rules through `@AGENTS.md` in the root `CLAUDE.md`. For manual Claude Code adoption, merge `CLAUDE.md` too; see the switching section for the skill entry point. Other tools need a reference from their project instruction entry point. The template defines conventions rather than enforcing tool behavior.

The instructions and document templates default to Chinese. During adoption, translate them into the target project's documentation language as needed, keeping paths, code, status values, and references consistent.

## Existing projects: let your agent integrate SoloSpec

Open an agent with network-reading or Git-cloning access in the target project and send the prompt below. No manual download is needed. The SoloSpec repository is the source; the currently open project is the destination.

```text
Integrate SoloSpec into the current project.

Template source: https://github.com/yutaoshao/SoloSpec
Destination: the current project root.

Read the template files from the repository's default branch. You may download or clone them into a new temporary directory outside the target project as a read-only source. Do not replace the current project with the template repository or copy its .git directory. If the source is inaccessible, report the specific cause and request an accessible URL or local template path instead of inventing template content.

Read the destination's existing AGENTS.md, CLAUDE.md, relevant project instructions, and documentation structure before making changes:
1. Merge the template's four-step workflow, task-record conventions, and ADR conventions into the project instructions. Preserve existing rules and remove duplication. Existing project conventions take precedence in substantive conflicts; explain and clarify unresolved conflicts instead of silently overwriting them.
2. Add missing ADR, spec, and task entry points and the task template. Reuse existing ADR or spec directories where possible; update directory conventions and references throughout the adopted instructions, templates, and optional skill instead of creating a second source of truth. Keep legacy task records in their existing locations and formats, and document their entry points.
3. Follow the target project's documentation language, translating template guidance as needed while preserving paths, code, and status values. Register guidelines and check commands only from actual code and existing documentation. Leave unknown information explicitly empty; do not invent a stack, decisions, or test results.
4. Keep the shared rules in AGENTS.md. For Claude Code, add a missing @AGENTS.md import to the root CLAUDE.md, preserving existing content and avoiding duplicate or circular imports. Keep the optional ADR skill in .agents/skills/adr-workflow/ as its single source; use a relative symlink from .claude/skills/adr-workflow to ../../.agents/skills/adr-workflow. Inspect and merge any existing file, directory, or link instead of overwriting it. If symlinks are unavailable, add an instruction to CLAUDE.md to read the source SKILL.md for lasting architectural decisions; do not require native skill discovery or install a runtime.
5. Preserve the destination's README, LICENSE, Git history, and unrelated files. Do not overwrite them with SoloSpec's project description or license; retain the required MIT notice for copied template content.
6. Do not introduce hooks, a task database, phase approvals, or automatic commits. Do not bulk-migrate historical documents just to adopt this template.

Check relative links, the CLAUDE.md import, and the optional skill link to ensure they resolve to the same source files inside the target project. Verify that simple tasks need no record, complex tasks have a defined location, a new session can identify work by its objective, and lasting decisions go into a single ADR directory.
Report added and changed files, reused directories, verification results, and unresolved issues. Do not commit or push to Git.
```

This prompt authorizes changes to integration files in the current project. Confirm that you have opened the intended destination. For your own fork, replace the source URL; for offline adoption, replace it with the absolute path to an existing local template. For a read-only assessment, replace the first sentence with: “Assess how SoloSpec would fit this project and propose file changes without modifying files.”

## Switching between Codex and Claude Code

Code, ADRs, specs, and task records are shared project files. Each tool uses its own entry point to read the same conventions:

| Content | Codex | Claude Code |
| --- | --- | --- |
| Project rules | `AGENTS.md` | `CLAUDE.md` imports `@AGENTS.md` |
| Optional ADR skill | `.agents/skills/adr-workflow/` | `.claude/skills/adr-workflow` links to the same source directory |
| Tasks and project knowledge | `docs/` | The same `docs/` |

`CLAUDE.md` does not duplicate the rules. The optional skill entry point is a relative symlink, so edits to the source apply to both tools. Preserve it during adoption or recreate the correct relative link in the destination. ZIP downloads, file copying, or system settings may not preserve symlinks; check that the link resolves instead of becoming a regular file containing path text.

If symlinks are unavailable, add this instruction to the existing `CLAUDE.md`. It provides on-demand document access, not a guarantee of native skill menus or automatic discovery:

```text
For architectural decisions that affect long-term maintenance, read .agents/skills/adr-workflow/SKILL.md and follow its ADR conventions.
```

To switch during unfinished work:

1. Ask the original agent to update the existing `task.md` with progress, verification, blockers, and next steps. If no record exists but a cross-session handoff is needed, create one using the project conventions.
2. Open the other tool in the same project directory. Across machines or checkouts, first synchronize the actual changes and task record, including uncommitted files.
3. Give the actual task path, for example: “Continue docs/tasks/2026-09-08-example/task.md. Check the current code against the record before proceeding with unfinished work.” Replace this example path with a real one.

SoloSpec hands off work through files. It does not transfer chat history, running processes, permissions, or external tool configuration; the new agent must check the current state. The entry points follow Claude Code's [file import](https://code.claude.com/docs/en/memory) and [skill directory](https://code.claude.com/docs/en/skills) conventions.

## Three kinds of documents

| Content | Location | When to write it |
| --- | --- | --- |
| Decisions and trade-offs | `docs/adr/` | A choice affects long-term maintenance |
| Stable constraints | `docs/spec/` | Future implementations must keep following a rule |
| Tasks and handoffs | `docs/tasks/YYYY-MM-DD-<topic>/task.md` | Work spans sessions or dependent stages, or a recorded plan is requested |

ADRs explain why, specs describe constraints, and task records track current progress. Connect them with links rather than duplicating content.

## Usage scenarios

- **Fix wording or a local bug:** read the relevant constraints, make the change, and verify it without creating a task record.
- **Replace storage in stages:** create a task record when needed, consult relevant ADRs, and document the lasting decision and necessary constraints once the direction is clear.
- **Resume unfinished work:** prefer an explicit path; otherwise match records by objective and next steps. Clarify ambiguity instead of guessing the “latest task.”
- **Record a project-wide decision:** create an ADR directly without manufacturing an accompanying task or spec.

## Repository contents

```text
AGENTS.md                         Shared project rules
CLAUDE.md                         Claude Code import entry point
LICENSE                           MIT license
README.md                         Chinese guide (default)
README_EN.md                      English guide
docs/adr/index.md                 Decision index
docs/adr/template.md              Decision template
docs/spec/index.md                Project guideline entry point
docs/tasks/README.md              Task locations and legacy adoption
templates/task.md                 Task template
.agents/skills/adr-workflow/       Optional ADR skill source
.claude/skills/adr-workflow        Relative symlink to the skill source
```

Documents live in the visible `docs/` directory so people and different tools can find them. There is no hidden runtime state. If you stop using SoloSpec, the documents remain part of your project.

## Scope and optional tools

There is no mandatory brainstorming, phase approval, fixed multi-agent sequence, session bookkeeping, or automatic commit. Choose requirement interviews, conversation-history search, and code review tools when useful; the core template does not depend on them.

SoloSpec is intended for solo development and maintenance. You can add ownership and review conventions when collaboration becomes necessary, without building a process for hypothetical future needs.

## Publishing your own copy

You can fork or download this repository to publish your own template. Replace the source URL in both README integration prompts with your own repository URL. The decision and spec entry points start empty and are filled with real project information by adopters; keep product-project history out of the reusable template.

## Origins and license

SoloSpec grew out of a review of documentation practices for solo projects, with inspiration from Trellis's approach to persisting guidelines and tasks. The instructions, templates, and skill text in this repository were written independently. No Trellis code, skill text, template text, or runtime components are bundled, and Trellis is not a dependency.

This repository uses the [MIT License](LICENSE). It does not change the existing licenses of other code, data, or documentation in your project.
