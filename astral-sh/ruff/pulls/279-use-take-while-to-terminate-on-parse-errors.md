```yaml
number: 279
title: Use take-while to terminate on parse errors
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/take-while
created_at: 2022-09-29T01:15:23Z
updated_at: 2022-09-29T02:06:36Z
url: https://github.com/astral-sh/ruff/pull/279
synced_at: 2026-01-12T15:55:04Z
```

# Use take-while to terminate on parse errors

---

_@charliermarsh_

Resolves #277.

---

_@andersk reviewed on 2022-09-29 01:36_

---

_Review comment by @andersk on `src/linter.rs`:87 on 2022-09-29 01:36_

This discards any `Err` completely, which doesn’t seem like it should be right?

---

_@charliermarsh reviewed on 2022-09-29 01:42_

---

_Review comment by @charliermarsh on `src/linter.rs`:87 on 2022-09-29 01:42_

We still get errors from the parser (e.g., `foo.py:1:7: E999 SyntaxError: Got unexpected EOF` when parsing `print(`), though you're probably right. Lemme iterate on it for a sec.

---

_Merged by @charliermarsh on 2022-09-29 02:06_

---

_Closed by @charliermarsh on 2022-09-29 02:06_

---

_Branch deleted on 2022-09-29 02:06_

---
