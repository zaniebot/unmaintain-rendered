```yaml
number: 467
title: Refactor lenient parsing
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: lenient-refactoring
created_at: 2023-11-20T11:38:50Z
updated_at: 2023-11-20T15:35:40Z
url: https://github.com/astral-sh/uv/pull/467
synced_at: 2026-01-10T15:50:29Z
```

# Refactor lenient parsing

---

_Pull request opened by @konstin on 2023-11-20 11:38_

Deduplicate lenient parsing code between version specifiers and Requirement. Use `warn_once!` since the warnings did show up multiple times in my code. Fix the macro hygiene in `warn_once!`.

---

_@charliermarsh reviewed on 2023-11-20 11:44_

---

_Review comment by @charliermarsh on `crates/pypi-types/src/lenient_requirement.rs`:42 on 2023-11-20 11:44_

This one matches on `>=3.6,` but not `(>=3.6,)`, just as an aside.

---

_@charliermarsh approved on 2023-11-20 11:44_

---

_Merged by @konstin on 2023-11-20 15:35_

---

_Closed by @konstin on 2023-11-20 15:35_

---

_Branch deleted on 2023-11-20 15:35_

---
