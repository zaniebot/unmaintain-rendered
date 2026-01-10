```yaml
number: 10835
title: "Be more explicit about inability to resolve workspace `requires-python`"
type: issue
state: closed
author: alex-debrecht-kcftech
labels:
  - error messages
assignees: []
created_at: 2025-01-22T01:06:19Z
updated_at: 2025-01-22T17:22:54Z
url: https://github.com/astral-sh/uv/issues/10835
synced_at: 2026-01-10T04:27:58Z
```

# Be more explicit about inability to resolve workspace `requires-python`

---

_Issue opened by @alex-debrecht-kcftech on 2025-01-22 01:06_

### Summary

When attempting to resolve dependencies in a workspace where the `requires-python` is set with a lower bound in the root and in each project, but the versions are incompatible, something like the following is printed:
```
warning: The workspace `requires-python` value (``) does not contain a lower bound. Add a lower bound to indicate the minimum compatible Python version (e.g., `>=3.12`).
⠼ Resolving dependencies...                                                                                                                                                                                                                  
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version () does not satisfy Python>=3.12 and your workspace requires some-package:dev, we can conclude that your workspace's requirements are unsatisfiable.
```

This is misleading because the workspace `requires-python` is non-empty and does include a lower bound. Perhaps more appropriate behavior would be to error earlier on an empty resolved `requires-python`?

Note the example structure instead prints:
```
warning: The workspace `requires-python` value (``) does not contain a lower bound. Add a lower bound to indicate the minimum compatible Python version (e.g., `>=3.12`).
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version () does not satisfy Python>=3.10 and albatross depends on Python>=3.10, we can conclude that albatross's requirements are unsatisfiable.
      And because your workspace requires albatross, we can conclude that your workspace's requirements are unsatisfiable.

      hint: The `requires-python` value () includes Python versions that are not supported by your dependencies (e.g., albatross==0.1.0 only supports >=3.10). Consider using a more restrictive `requires-python` value (like >=3.10).
```
I'm not easily able to reproduce the exact behavior in the first example.

### Example

Using the repo structure from the docs:
```
albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   └── seeds
│       ├── pyproject.toml
├── pyproject.toml
```

pyproject.toml:
```
[project]
name = "albatross"
version = "0.1.0"
requires-python = ">=3.10"

[tool.uv.workspace]
members = ["packages/*"]
```

packages/bird-feeder/pyproject.toml:
```
[project]
name = "bird-feeder"
version = "0.1.0"
requires-python = "==3.10"
```

packages/seeds/pyproject.toml:
```
[project]
name = "seeds"
version = "0.1.0"
requires-python = ">=3.12"
```

---

_Label `enhancement` added by @alex-debrecht-kcftech on 2025-01-22 01:06_

---

_Comment by @charliermarsh on 2025-01-22 01:07_

In case it's helpful, your Python requirements are disjoint. There is no version that can possibly satisfy both `==3.10` and `>=3.12`.

---

_Label `enhancement` removed by @charliermarsh on 2025-01-22 01:07_

---

_Label `error messages` added by @charliermarsh on 2025-01-22 01:07_

---

_Comment by @alex-debrecht-kcftech on 2025-01-22 01:13_

Yep, my suggestion was just to be more explicit about that fact in the output.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-22 02:35_

---

_Closed by @charliermarsh on 2025-01-22 17:22_

---

_Closed by @charliermarsh on 2025-01-22 17:22_

---
