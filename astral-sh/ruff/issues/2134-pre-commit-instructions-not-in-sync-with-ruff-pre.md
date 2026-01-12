```yaml
number: 2134
title: "pre-commit instructions not in sync with `ruff-pre-commit` repo"
type: issue
state: closed
author: goretkin
labels: []
assignees: []
created_at: 2023-01-24T16:22:48Z
updated_at: 2023-01-25T04:47:22Z
url: https://github.com/astral-sh/ruff/issues/2134
synced_at: 2026-01-12T15:54:42Z
```

# pre-commit instructions not in sync with `ruff-pre-commit` repo

---

_@goretkin_

https://github.com/charliermarsh/ruff/blob/605416922d193d2261bc8b5ed0de8741691a018d/README.md#usage has

```
- repo: https://github.com/charliermarsh/ruff-pre-commit
  # Ruff version.
  rev: 'v0.0.233'
  hooks:
    - id: ruff
```

but the latest tag is https://github.com/charliermarsh/ruff-pre-commit/tree/v0.0.231 (there is no v0.0.233)

---

_Comment by @charliermarsh on 2023-01-24 17:36_

Thank you. We update the README when we make the release commit, but the release itself takes a bit. (This release also failed due to us finally hitting a PyPI limit.) I'll back this out for now until 0.0.233 is out.

---

_Closed by @charliermarsh on 2023-01-25 04:47_

---

_Comment by @charliermarsh on 2023-01-25 04:47_

Fixed!

---
