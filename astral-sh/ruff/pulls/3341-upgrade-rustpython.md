```yaml
number: 3341
title: Upgrade RustPython
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rustpython
created_at: 2023-03-04T17:49:29Z
updated_at: 2023-03-04T19:01:05Z
url: https://github.com/astral-sh/ruff/pull/3341
synced_at: 2026-01-12T15:55:12Z
```

# Upgrade RustPython

---

_@charliermarsh_

## Summary

Upgrades RustPython to pull in https://github.com/RustPython/RustPython/pull/4623, which in turn closes  https://github.com/charliermarsh/ruff/issues/3337.

The linked PR ensures that we treat `match` as an identifier, and not a keyword, when assigning a lambda, like `match = lambda: 1 + 1`.


---

_@MichaReiser approved on 2023-03-04 17:55_

Can you add some context to the PR about the features/bugfixes that we get with the bump (why are we interested in pulling in the new version)? 

---

_Comment by @charliermarsh on 2023-03-04 17:59_

Done, thanks for the nudge :)

---

_Merged by @charliermarsh on 2023-03-04 19:01_

---

_Closed by @charliermarsh on 2023-03-04 19:01_

---

_Branch deleted on 2023-03-04 19:01_

---
