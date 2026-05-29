# Weave Documentation

Weave is a programming language and interop toolchain for building software across existing language ecosystems.

The goal is not to replace Java, Python, C++, or other major languages. The goal is to let teams use those ecosystems together through typed, generated, runtime-aware bridges.

In a Weave project, users should be able to:

- drop existing Java and Python projects into a conventional project layout
- let Weave detect how those projects are built and run
- expose selected Java classes, Spring beans, Python modules, or Python functions
- call Java from Python, Python from Java, or both from `.wv` files
- write `.wv` only when they want language-neutral orchestration logic

## Core Idea

Weave has three roles:

- **Language:** `.wv` files provide a small, typed language for interop and orchestration.
- **Bridge generator:** Weave scans existing projects and generates Java/Python client code.
- **Runtime bridge:** Weave moves calls, values, errors, and logs between language runtimes.

## Typical Workflow

```bash
weave doctor
weave scan
weave bind
weave run
```

The intended flow is:

1. Weave detects projects.
2. The user selects entrypoints.
3. Weave generates bridge clients.
4. Java, Python, and optional `.wv` code can call across runtimes.

## Docs

- [Vision](vision.md)
- [Project Layout](project-layout.md)
- [Interop Model](interop-model.md)
- [V1 Scope](v1-scope.md)
