```yaml
number: 1944
title: "Avoid broken autofix for `SIM103` with `elif`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/SIM103
created_at: 2023-01-18T02:58:22Z
updated_at: 2023-01-18T03:43:52Z
url: https://github.com/astral-sh/ruff/pull/1944
synced_at: 2026-01-12T15:55:07Z
```

# Avoid broken autofix for `SIM103` with `elif`

---

_@charliermarsh_

Also adjusts the generator to avoid the extra parentheses (and skips commented `if` statements).

Closes #1943.

---

_Review comment by @charliermarsh on `src/rules/flake8_simplify/rules/ast_if.rs`:108 on 2023-01-18 02:59_

If you're in an `elif`, the location on the statement doesn't include the `else`.

---

_@charliermarsh reviewed on 2023-01-18 02:59_

---

_Merged by @charliermarsh on 2023-01-18 03:03_

---

_Closed by @charliermarsh on 2023-01-18 03:03_

---

_Branch deleted on 2023-01-18 03:03_

---

_@andersk reviewed on 2023-01-18 03:42_

---

_Review comment by @andersk on `src/rules/flake8_simplify/rules/ast_if.rs`:108 on 2023-01-18 03:42_

Yeah, that seems like a bug. Opened RustPython/RustPython#4459.

---

_@charliermarsh reviewed on 2023-01-18 03:43_

---

_Review comment by @charliermarsh on `src/rules/flake8_simplify/rules/ast_if.rs`:108 on 2023-01-18 03:43_

Thanks, much appreciated.

---
