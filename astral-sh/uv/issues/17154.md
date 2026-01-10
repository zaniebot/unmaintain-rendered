```yaml
number: 17154
title: Add support for local venvs in workspace repositories
type: issue
state: closed
author: danijar
labels:
  - enhancement
assignees: []
created_at: 2025-12-16T22:14:18Z
updated_at: 2025-12-28T20:23:11Z
url: https://github.com/astral-sh/uv/issues/17154
synced_at: 2026-01-10T03:11:35Z
```

# Add support for local venvs in workspace repositories

---

_Issue opened by @danijar on 2025-12-16 22:14_

### Summary

We have a large monorepo with multiple projects. We'd like a global uv.lock to enforce consistent dependencies, are provided by the workspace feature.

However, the current workspace feature is prone to "dependency ghosting", where `uv run` for one package will install its dependencies into the workspace venv. A following `uv run` for a different package will then also have access to those dependencies, even if they are not specified within the second package's pyproject.toml.

It would be ideal to have a global uv.lock for the whole workspace (to enforce version consistency) but separate venvs for each package within the workspace (to avoid dependency ghosting).

### Example

_No response_

---

_Label `enhancement` added by @danijar on 2025-12-16 22:14_

---

_Comment by @zanieb on 2025-12-16 22:50_

It sounds like you might just want to use `uv run --exact`?

We're considering multiple virtual environments though, there are various problems it makes easier.

---

_Comment by @danijar on 2025-12-28 20:23_

Thanks! I realized that we actually need not just separate venvs but also separate lock files for each package, so I'm closing this issue here.

---

_Closed by @danijar on 2025-12-28 20:23_

---
