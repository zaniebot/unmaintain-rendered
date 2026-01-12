```yaml
number: 2691
title: Add documentation for mccabe, isort, and flake8-annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs
created_at: 2023-02-09T16:53:05Z
updated_at: 2023-02-10T22:59:38Z
url: https://github.com/astral-sh/ruff/pull/2691
synced_at: 2026-01-12T04:52:00Z
```

# Add documentation for mccabe, isort, and flake8-annotations

---

_Pull request opened by @charliermarsh on 2023-02-09 16:53_

See: #2646.

---

_Label `documentation` added by @charliermarsh on 2023-02-09 16:53_

---

_Merged by @charliermarsh on 2023-02-09 16:56_

---

_Closed by @charliermarsh on 2023-02-09 16:56_

---

_Branch deleted on 2023-02-09 16:56_

---

_@not-my-profile reviewed on 2023-02-10 22:43_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_annotations/rules.rs`:382 on 2023-02-10 22:43_

>[Any] is the default type for unannotated expressions.

This depends on the type checker. While that's true for mypy (and probably also pytype?) it's not generally true, e.g. Pyright uses [Unknown](https://github.com/microsoft/pyright/blob/main/docs/internals.md#type-checking-concepts) instead.

---

_@charliermarsh reviewed on 2023-02-10 22:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/rules.rs`:382 on 2023-02-10 22:48_

Wow TIL

---

_@charliermarsh reviewed on 2023-02-10 22:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/rules.rs`:382 on 2023-02-10 22:49_

(Did you mean "probably also pyre"? Or "pytype"? I see pyright twice in that sentence.)

---

_@not-my-profile reviewed on 2023-02-10 22:51_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_annotations/rules.rs`:382 on 2023-02-10 22:51_

(ah yes I meant pytype ...)

---

_@not-my-profile reviewed on 2023-02-10 22:58_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_annotations/rules.rs`:382 on 2023-02-10 22:58_

I should add that Pyright has very good type inference so for most expressions it can actually infer the right type without any annotations ... but when a type cannot be inferred, it infers `Unknown` instead of `Any` (and `Unknown` is a Pyright internal type).

---
