# V1 Scope

V1 should prove that Weave can reliably bridge Java and Python projects without requiring source edits.

The first useful milestone:

```text
Spring Boot service -> generated Weave bridge -> Python analytics function
```

The second milestone:

```text
Python code -> generated Weave bridge -> Spring Boot service
```

The third milestone:

```text
.wv orchestration -> Java service and Python function
```

## V1 Capabilities

V1 should support:

- conventional `/java`, `/python`, and `/weave` project folders
- optional `weave.toml` config
- Java project detection for Maven, Gradle, and Spring Boot
- Python project detection for common package and environment files
- selected Java/Spring entrypoints
- selected Python module/function entrypoints
- generated Java clients for Python calls
- generated Python clients for Java calls
- optional `.wv` orchestration files
- primitive values, strings, lists, maps, and simple object records
- clear error messages across runtime boundaries
- a `weave doctor` command

## V1 Constraints

V1 should not attempt:

- C++ interop
- arbitrary Java-to-Python object sharing
- automatic exposure of entire projects
- cross-language inheritance
- full Python type inference
- direct in-process JVM/CPython embedding
- high-performance dataframe transport as the default

## Type Strategy

Java has strong type metadata, so Weave can infer many Java signatures from compiled classes, reflection, and Spring metadata.

Python is dynamic, so V1 should prefer:

- type hints
- `.pyi` stubs
- external Weave binding declarations
- fallback `Any` types when no reliable type information exists

Untyped Python should still work, but typed Python should produce better generated clients and better diagnostics.

## Success Criteria

V1 is successful when a user can:

1. Place an existing Spring Boot app under `/java`.
2. Place an existing Python analytics package under `/python`.
3. Run `weave doctor` and see both projects detected.
4. Select a Java service and Python function to expose.
5. Generate bindings.
6. Call Python from Java.
7. Call Java from Python.
8. Optionally write `.wv` code that orchestrates both.
