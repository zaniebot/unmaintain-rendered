```yaml
number: 2779
title: Avoid treating deferred string annotations as required-at-runtime
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/defer
created_at: 2023-02-11T19:56:24Z
updated_at: 2023-02-11T20:00:09Z
url: https://github.com/astral-sh/ruff/pull/2779
synced_at: 2026-01-12T04:52:01Z
```

# Avoid treating deferred string annotations as required-at-runtime

---

_Pull request opened by @charliermarsh on 2023-02-11 19:56_

See #2761.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:3905 on 2023-02-11 19:58_

We still want to omit `self.in_deferred_type_definition`, since if that's true but `self.in_annotation` is `False`, then we're in a runtime-required future type.

---

_@charliermarsh reviewed on 2023-02-11 19:58_

---

_@charliermarsh reviewed on 2023-02-11 19:59_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_type_checking/TCH002.py`:47 on 2023-02-11 19:59_

This _should_ error because it's _not_ required at runtime, despite being on the RHS.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_type_checking/snapshots/ruff__rules__flake8_type_checking__tests__runtime-import-in-type-checking-block_TCH004_8.py.snap`:5 on 2023-02-11 19:59_

This _shouldn't_ error because it's _not_ required at runtime, despite being on the RHS.

---

_@charliermarsh reviewed on 2023-02-11 19:59_

---

_Merged by @charliermarsh on 2023-02-11 20:00_

---

_Closed by @charliermarsh on 2023-02-11 20:00_

---

_Branch deleted on 2023-02-11 20:00_

---
