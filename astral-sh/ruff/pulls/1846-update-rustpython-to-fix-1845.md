```yaml
number: 1846
title: "Update RustPython to fix #1845"
type: pull_request
state: closed
author: messense
labels: []
assignees: []
base: main
head: update-rustpython
created_at: 2023-01-13T13:16:57Z
updated_at: 2023-01-13T14:48:03Z
url: https://github.com/astral-sh/ruff/pull/1846
synced_at: 2026-01-12T15:55:07Z
```

# Update RustPython to fix #1845

---

_@messense_

See https://github.com/RustPython/RustPython/pull/4444

Resolves #1845

---

_Comment by @messense on 2023-01-13 13:32_

Not sure what's going on with the failed test, here's the changes between these commits: https://github.com/RustPython/RustPython/compare/d532160333ffeb6dbeca2c2728c2391cd1e53b7f..68d413e923bb632e179b7ff7ef170c174298fd78

---

_Comment by @charliermarsh on 2023-01-13 14:11_

Is it related to the tokenizer change?

https://github.com/charliermarsh/ruff/pull/1836

---

_Comment by @messense on 2023-01-13 14:47_

I guess so, closing in favor of a single `RustPython` update in #1836 

---

_Closed by @messense on 2023-01-13 14:47_

---

_Branch deleted on 2023-01-13 14:48_

---
