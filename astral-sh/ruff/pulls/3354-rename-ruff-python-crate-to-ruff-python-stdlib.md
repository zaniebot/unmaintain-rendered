```yaml
number: 3354
title: "Rename `ruff_python` crate to `ruff_python_stdlib`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/python
created_at: 2023-03-05T21:50:38Z
updated_at: 2023-03-06T13:43:24Z
url: https://github.com/astral-sh/ruff/pull/3354
synced_at: 2026-01-12T04:39:44Z
```

# Rename `ruff_python` crate to `ruff_python_stdlib`

---

_Pull request opened by @charliermarsh on 2023-03-05 21:50_

In hindsight, `ruff_python` is too general. A good giveaway is that it's actually a prefix of some other crates. The intent of this crate is to reimplement pieces of the Python standard library and CPython itself, so `ruff_python_stdlib` feels appropriate.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-05 21:50_

---

_Review requested from @konstin by @charliermarsh on 2023-03-05 21:50_

---

_@konstin approved on 2023-03-06 10:11_

---

_Merged by @charliermarsh on 2023-03-06 13:43_

---

_Closed by @charliermarsh on 2023-03-06 13:43_

---

_Branch deleted on 2023-03-06 13:43_

---
