```yaml
number: 1323
title: "Support shell expansion in `extend` paths"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/env-vars
created_at: 2022-12-22T01:45:05Z
updated_at: 2022-12-22T01:46:39Z
url: https://github.com/astral-sh/ruff/pull/1323
synced_at: 2026-01-12T15:55:06Z
```

# Support shell expansion in `extend` paths

---

_@charliermarsh_

This allows things like:

```toml
[tool.ruff]
extend = "$FOO/pyproject.toml"
```

```toml
[tool.ruff]
extend = "~/pyproject.toml"
```

Raises an error in the event that an environment variable is undefined.

Resolves #1316.


---

_Merged by @charliermarsh on 2022-12-22 01:46_

---

_Closed by @charliermarsh on 2022-12-22 01:46_

---

_Branch deleted on 2022-12-22 01:46_

---
