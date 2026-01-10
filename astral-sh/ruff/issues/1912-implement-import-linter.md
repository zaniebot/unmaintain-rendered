---
number: 1912
title: Implement import-linter
type: issue
state: open
author: jankatins
labels:
  - plugin
assignees: []
created_at: 2023-01-16T10:55:54Z
updated_at: 2025-10-17T15:47:54Z
url: https://github.com/astral-sh/ruff/issues/1912
synced_at: 2026-01-10T01:22:40Z
---

# Implement import-linter

---

_Issue opened by @jankatins on 2023-01-16 10:55_

> Import Linter allows you to define and enforce rules for the internal and external imports within your Python project.

From the readme at https://github.com/seddonym/import-linter:

> Decide on the dependency flows you wish to check. In this example, we have decided to make sure that myproject.foo has dependencies on neither myproject.bar nor myproject.baz, so we will use the forbidden contract type.

> Create an .importlinter file in the root of your project to define your contract(s). In this case:

```
[importlinter]
root_package = myproject

[importlinter:contract:1]
name=Foo doesn't import bar or baz
type=forbidden
source_modules=
    myproject.foo
forbidden_modules=
    myproject.bar
    myproject.baz
```

Some more background on this kind of linter in this blog post: https://sourcery.ai/blog/dependency-rules/

---

_Comment by @charliermarsh on 2023-01-16 17:19_

If helpful in the interim, we do support some subset of this via `flake8-tidy-imports`, e.g.:

```toml
[tool.ruff.flake8-tidy-imports.banned-api]
"cgi".msg = "The cgi module is deprecated, see https://peps.python.org/pep-0594/#cgi."
"typing.TypedDict".msg = "Use typing_extensions.TypedDict instead."
```

---

_Label `plugin` added by @charliermarsh on 2023-01-16 17:19_

---

_Referenced in [astral-sh/ruff#10300](../../astral-sh/ruff/issues/10300.md) on 2024-03-09 19:59_

---

_Comment by @ashrub-holvi on 2024-08-09 07:30_

Looks like there is an alternative to import-linter written in Rust - https://github.com/gauge-sh/tach/

---

_Comment by @ashrub-holvi on 2025-10-17 15:47_

> alternative to import-linter written in Rust - https://github.com/gauge-sh/tach/

looks like already died:
> ⚠️ UNMAINTAINED: This repository is no longer actively maintained. Issues and pull requests may not receive a response.

---
