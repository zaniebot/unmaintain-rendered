```yaml
number: 966
title: Normalize base python in venv creation
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/normalize-base-python
created_at: 2024-01-18T15:30:51Z
updated_at: 2024-01-18T15:32:31Z
url: https://github.com/astral-sh/uv/pull/966
synced_at: 2026-01-10T15:39:03Z
```

# Normalize base python in venv creation

---

_Pull request opened by @konstin on 2024-01-18 15:30_

Fixes #965

We have to canonicalize the interpreter path, otherwise the home is set to the venv dir instead of the real root. This would make python-build-standalone fail with the encodings module not being found because its home is wrong.

---

_Merged by @konstin on 2024-01-18 15:32_

---

_Closed by @konstin on 2024-01-18 15:32_

---

_Branch deleted on 2024-01-18 15:32_

---
