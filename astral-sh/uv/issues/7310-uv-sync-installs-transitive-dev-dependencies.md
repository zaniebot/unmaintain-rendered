```yaml
number: 7310
title: "`uv sync` installs transitive dev dependencies"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - breaking
assignees: []
created_at: 2024-09-11T20:40:32Z
updated_at: 2024-09-12T13:20:44Z
url: https://github.com/astral-sh/uv/issues/7310
synced_at: 2026-01-12T15:59:12Z
```

# `uv sync` installs transitive dev dependencies

---

_@charliermarsh_

If you have package A that depends on workspace member B, and you run `uv sync --package A --dev`, we'll pull in the dev dependencies for both A and B. That seems wrong -- we shouldn't pull in transitive development dependencies.

---

_Label `bug` added by @charliermarsh on 2024-09-11 20:40_

---

_Label `breaking` added by @charliermarsh on 2024-09-11 20:40_

---

_Closed by @charliermarsh on 2024-09-12 13:20_

---

_Closed by @charliermarsh on 2024-09-12 13:20_

---
