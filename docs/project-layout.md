# Project Layout

A Weave project can use this conventional structure:

```text
project/
  java/
    backend-spring-app/
  python/
    analytics-package/
  weave/
    optional .wv files
  weave.toml
```

The `java`, `python`, and `weave` folders are conventions. Users can also point Weave at external paths when their repositories already live elsewhere.

## Minimal Config

In simple projects, `weave.toml` should be small:

```toml
[project]
name = "sales-platform"
```

Weave should automatically scan the conventional folders.

## Optional Path Config

When projects are not in the default layout, users can provide paths:

```toml
[java.backend]
path = "./services/backend"

[python.analytics]
path = "../analytics"
```

## Detection

Weave should infer build tools and frameworks by reading project files.

For Java, Weave should detect:

- `pom.xml` as Maven
- `build.gradle` or `build.gradle.kts` as Gradle
- Spring Boot dependencies
- `@SpringBootApplication`
- existing jars in `target/` or `build/libs/`

For Python, Weave should detect:

- `pyproject.toml`
- `requirements.txt`
- `setup.py` or `setup.cfg`
- `poetry.lock`
- `environment.yml`
- `.venv` or `venv`

Users should only need explicit `build`, `framework`, or environment config when detection is wrong or ambiguous.

## Doctor Command

Weave should include a diagnostic command:

```bash
weave doctor
```

It should report what Weave detected, which projects are available, and what still needs configuration.
