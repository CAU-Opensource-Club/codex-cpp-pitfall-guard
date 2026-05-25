---
name: codex-cpp-pitfall-guard
description: "Use when Codex reads, analyzes, compares, reviews, modifies, generates, designs, or verifies code, especially C++/CMake. Apply strict guardrails for premise verification, comparable-context analysis, Modern C++ implementation, SSOT, performance-sensitive ownership, minimal code generation, and avoiding unnecessary project-wide compilation. Do not use for unrelated tasks."
---

# Code Reading and Writing Rules

## Scope

Use this skill only when the current task involves reading code, analyzing code, comparing implementations, modifying code, generating code, or designing a software module. Do not mention or apply this skill when the task is unrelated to code reading or code writing.

Assume the user has a professional but incomplete understanding of the project. Avoid explaining generic programming basics unless the user asks for them or they are necessary to explain a project-specific point.

## Code Reading

Inspect the code before accepting the user's framing. Treat questions and assumptions as hypotheses, not facts.

- If the user asks how project A overcomes weakness B, first verify whether weakness B exists in the relevant context, whether A addresses it, and whether the implementation supports that claim.
- If the inspected code does not implement the claimed behavior, state directly: "This is not implemented in the inspected code."
- If the user asks to compare similar features across projects, first verify whether their usage scenarios, semantics, constraints, and performance goals are comparable.
- State what the code actually implements.
- State plainly when something is not implemented.
- Do not convert missing functionality into a supposed advantage.
- Do not invent benefits; only credit advantages that follow from the code.
- Do not treat absence of implementation as an intentional optimization unless code or docs support that.
- Keep analysis serious, specific, and grounded in file paths, functions, types, constraints, and observable behavior.
- When analyzing implementation pros and cons, be strict: unsupported benefits are not benefits, and missing functionality remains missing functionality.
- Distinguish implemented behavior, inferred intent, and speculation.

Use precise labels when helpful:

- `Implemented: ...`
- `Not implemented: ...`
- `Likely intention: ...`
- `Risk: ...`
- `Unclear from current code: ...`

## Analysis Documents

Format analysis documents for VS Code's default Markdown preview:

- Use clear headings, short sections, and flat bullets.
- Put conclusions before supporting detail when it improves scanning.
- Prefer compact tables when comparing designs.
- Use code references and concrete names instead of broad prose.
- Avoid overly long nested lists.
- Avoid common-knowledge filler.

## C++ Implementation

Prefer clean, compact Modern C++ that follows the surrounding codebase style.

When Clean Code and Effective Modern C++ conflict, prioritize:

1. performance;
2. Effective Modern C++;
3. Clean Code aesthetics.

- Keep code minimal; do not add unused extension points, helper functions, abstractions, or "complete" scaffolding without an immediate need.
- Preserve SSOT: avoid duplicating facts, configuration, state ownership, protocol constants, or derived values.
- Avoid expensive ownership models such as `std::shared_ptr` unless shared ownership is truly required by the design.
- Avoid directly introducing old-style singleton or inheritance-heavy designs. If equivalent capability is needed, first look for composition, dependency injection, free functions, templates, or small value types.
- Keep interfaces narrow and explicit.
- Let performance-sensitive code stay simple and predictable.

Prefer these ownership forms when they fit:

- value ownership;
- references for non-owning access;
- raw pointers only for explicit non-owning nullable references;
- `std::unique_ptr` for exclusive heap ownership;
- RAII and local ownership;
- atomic raw pointer plus explicit lifetime or epoch scheme when appropriate.

Use `std::shared_ptr` only when shared ownership is actually required.

If duplication is needed for caching or performance, make the source of truth explicit and define when the cached copy is refreshed or invalidated.

Before editing:

- Read nearby code and tests to learn naming, error handling, ownership, threading, and allocation patterns.
- Make the smallest change that satisfies the request.
- Work with any existing user changes in the tree; do not revert unrelated edits.

## Minimal Code Generation

When implementing a module or function:

- Generate only what is immediately required.
- Do not add speculative extension points or framework-like abstractions.
- Do not add unused helper functions.
- Do not create "complete-looking" abstractions without current use.
- Prefer a small correct implementation over a broad generic one.

## Validation

Do not compile the whole project unless the user explicitly asks for it.

Treat these as explicit permission:

- "compile the whole project";
- "build and fix all errors";
- "run the full build";
- "please compile and repair compile errors."

If the user only asks to implement or modify a module, inspect the relevant files, check local correctness of the changed module, and run only narrow tests or targeted checks when appropriate. If no focused validation exists, use static inspection and explain what was not run.

When tests or builds fail, report the concrete failure and whether it is related to the change.

## Response Style

Be concise and focus on concrete changes, evidence, uncertainty, and remaining risks. Distinguish evidence from inference. Avoid filler explanations and reassurance that is not backed by code.

For code-reading results, lead with findings and uncertainty boundaries. For code-writing results, summarize the changed files and focused validation. When code is produced, include only the necessary code unless the user asks for a fuller scaffold.
