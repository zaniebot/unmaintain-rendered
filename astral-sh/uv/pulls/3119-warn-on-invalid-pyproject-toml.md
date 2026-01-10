```yaml
number: 3119
title: Warn on invalid pyproject.toml
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
draft: true
base: main
head: konsti/warn-invalid-pyproject.toml
created_at: 2024-04-18T11:17:04Z
updated_at: 2024-04-25T10:35:35Z
url: https://github.com/astral-sh/uv/pull/3119
synced_at: 2026-01-10T14:37:54Z
```

# Warn on invalid pyproject.toml

---

_Pull request opened by @konstin on 2024-04-18 11:17_

When we try to compile a pyproject.toml, we currently silently fall back to PEP 517 when we can't parse it (according to PEP 621). This PR adds a warning. It would be even better if we could be strict and require a valid pyproject.toml.

This is relevant for `tool.uv.sources`, which doesn't get forwarded through PEP 517, so we'd silently lose that information.


---

_Closed by @konstin on 2024-04-25 10:35_

---
