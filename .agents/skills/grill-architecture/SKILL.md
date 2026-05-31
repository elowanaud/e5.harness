---
name: grill-architecture
description: Interview the user about code and file architecture until a request-specific architecture is explicit, dependency-ordered, and approved. Use when the user asks to grill, stress-test, review, or shape architecture, system design, module boundaries, file layout, refactor design, integration design, or technical planning before implementation. Do not use for implementation, general code review, or broad requirement discovery unless architecture decisions are the next needed step.
---

## purpose

Turn clarified requirements into a concrete, request-specific architecture before implementation begins. Guide the user from intent to an exact file-name architecture tree with operation markers, nested symbols, responsibilities, contracts, dependencies, tests, verification, risks, and unknowns.

## session loop

1. Review the conversation and available context.
2. Scan the codebase lightly before asking architecture questions.
3. Infer anything the context already answers.
4. Ask one dependency-unlocking question at a time.
5. Include a recommended answer with rationale.
6. Update the architecture sketch as answers arrive.
7. Repeat until the architecture tree and dependency order are clear.
8. Require explicit approval before treating the architecture as ready.

## before questioning

Review the available conversation and context first. Extract decisions, constraints, stated goals, rejected choices, and open gaps before asking anything.

Ask for a summary only when the available conversation is missing, contradictory, or too large to inspect with confidence. When asking for that summary, request only the missing context needed to continue.

Do a lightweight codebase scan before architecture questions. Look for existing modules, naming patterns, nearby tests, data flows, public interfaces, and conventions that can answer obvious questions without burdening the user.

Deepen the scan only for high-risk cases: cross-cutting state changes, authentication or authorization, persisted data, migrations, concurrency, public APIs, security-sensitive paths, performance-sensitive paths, generated code, or changes that affect multiple runtime surfaces.

## questioning rules

Ask exactly one question at a time. Prefer the next question that unlocks the most downstream architecture decisions.

Do not ask a question if the available context or codebase scan can answer it. State the answer you inferred and continue.

Include a recommended answer with rationale for every question. Make the recommendation concrete enough that the user can accept, reject, or edit it quickly.

Tie each question to an architecture dependency. Explain what later decision the answer will unblock.

## architecture synthesis

Synthesize continuously as answers arrive. Keep the architecture grounded in the user's request, environment, and codebase patterns found during the scan.

Separate decisions from assumptions. Mark unknowns that remain unresolved and explain whether they block architecture or can be handled during implementation.

Prefer small, composable boundaries. Assign responsibilities to files, modules, services, commands, routes, components, tests, or other units that already fit the project style.

## architecture output requirements

Produce a request-specific architecture tree when enough decisions are known.

The architecture tree must be rendered as a directory tree rooted at `.` and must use exact file and directory names from the proposed architecture. Do not describe the architecture only in prose. Do not repeat the same directory path in separate branches; merge shared parent directories into one coherent tree.

Use operation markers on every affected file or directory:

- `[new]` for files or directories to create
- `[edit]` for existing files to modify
- `[delete]` for files or directories to remove
- `[test]` for test files to add or modify
- no marker for unchanged context nodes

When a file contains meaningful internal units, nest those units under the file using their exact names when known: functions, classes, methods, routes, handlers, components, schemas, commands, or exported symbols.

Use this shape:

```text
.
└── src/
    ├── controllers/
    │   ├── [edit] users.controller.ts
    │   └── [new] todos.controller.ts
    │       ├── list()
    │       ├── view()
    │       ├── create()
    │       └── update()
    └── services/
        ├── [delete] users.service.ts
        └── [new] todos.service.ts
            ├── listTodos()
            ├── getTodo()
            ├── createTodo()
            └── updateTodo()
```

After the tree, include concise notes for each meaningful affected node:

- responsibility
- public contract or interface
- dependencies
- tests
- verification

Keep those notes tied to the exact file names from the tree. If an exact file name is unknown, ask one dependency-unlocking question before finalizing the architecture.

Call out the main data flow, control flow, public boundary, and error boundary when they matter to the request.

## dependency order

Present a short architecture dependency order, not an implementation execution plan. Put prerequisites before dependents, tests near the behavior they prove, and verification after the surface it exercises exists.

Identify decisions that must be made before other decisions. Continue asking one question at a time until the dependency order is clear enough to act on.

## alternatives

Name each rejected alternative that materially shaped the design. Give the reason it was rejected, such as poorer fit with existing contracts, higher risk, extra migration cost, weaker tests, or unnecessary complexity.

When the best choice is not obvious, compare the top alternatives briefly and recommend one answer with rationale.

## risk handling

List risks and unknowns that could change the architecture. Separate blockers from watch items.

For each risk, name the likely impact and the verification that would expose it early. Use available tools and the environment to reduce uncertainty before asking the user when practical.

Do not treat the architecture as approved while blocker risks or unknowns remain unresolved.

## approval gate

Require explicit approval before treating the architecture as approved or ready for implementation. Accept clear approval phrases such as `approved`, `yes, use this architecture`, `this architecture is approved`, and `ship this design`.

Treat vague positivity such as `looks good` as not enough. Ask one confirmation question that requests explicit approval before closing the architecture session.

## scope boundary

Inspect, analyze, and propose only: do not implement code, do not edit source files, do not scaffold feature files, and do not generate a full implementation work plan during the architecture session.
