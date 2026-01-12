```yaml
number: 3223
title: "Create dedicated `Body` nodes in the formatter CST"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/body
created_at: 2023-02-25T04:29:17Z
updated_at: 2023-02-27T22:55:07Z
url: https://github.com/astral-sh/ruff/pull/3223
synced_at: 2026-01-12T04:39:44Z
```

# Create dedicated `Body` nodes in the formatter CST

---

_Pull request opened by @charliermarsh on 2023-02-25 04:29_

We need dedicated body nodes so that we can properly attach comments across disparate code blocks within the same AST node. For example, if we have own-line comments in an `if` and `else` block, right now, we have no way to ensure those are attached to nodes within the `if` or `else` block.


---

_Label `autoformatter` added by @charliermarsh on 2023-02-25 04:29_

---

_@charliermarsh reviewed on 2023-02-25 04:29_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/core/helpers.rs`:44 on 2023-02-25 04:29_

Ideally, in the future, this could be handled by a parser, rather than with this separate conversion pass.

---

_@charliermarsh reviewed on 2023-02-25 04:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/format/builders.rs`:21 on 2023-02-25 04:30_

Also own-line comments, like `finally:  # comment`.

---

_Marked ready for review by @charliermarsh on 2023-02-27 22:32_

---

_Merged by @charliermarsh on 2023-02-27 22:55_

---

_Closed by @charliermarsh on 2023-02-27 22:55_

---

_Branch deleted on 2023-02-27 22:55_

---
