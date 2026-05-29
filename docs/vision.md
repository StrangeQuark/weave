# Vision

Weave exists to make mixed-language software feel intentional instead of stitched together by ad hoc scripts, subprocess calls, and hand-written glue code.

The flagship use case is a project that wants to combine:

- Java and Spring Boot for application services
- Python for data science, analytics, machine learning, and automation
- `.wv` files for typed orchestration where a neutral interop layer is useful

Weave should also support projects that do not use `.wv` files at all. A team should be able to use Weave only to call Java from Python, or Python from Java.

## Principles

- Existing Java and Python code should not need source edits.
- `.wv` files should be optional, not mandatory.
- Weave should expose selected entrypoints, not entire projects by default.
- Projects should be detected by convention first, with config overrides when needed.
- Interop boundaries should be explicit, typed when possible, and easy to inspect.
- Weave should treat a Spring Boot app or Python package as a runtime unit, not as isolated files.

## Non-Goals

For early versions, Weave should not try to:

- compile arbitrary Java, Python, and C++ into one native binary
- automatically expose every class, method, or function in a project
- hide all runtime boundaries
- support every framework in every ecosystem
- guarantee zero-cost calls across languages

The first versions should prioritize reliable, understandable interop over magic.
