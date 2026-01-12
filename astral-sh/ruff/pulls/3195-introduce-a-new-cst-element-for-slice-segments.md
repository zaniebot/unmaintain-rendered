```yaml
number: 3195
title: Introduce a new CST element for slice segments
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-ii
created_at: 2023-02-24T00:40:31Z
updated_at: 2023-02-24T00:49:42Z
url: https://github.com/astral-sh/ruff/pull/3195
synced_at: 2026-01-12T04:39:44Z
```

# Introduce a new CST element for slice segments

---

_Pull request opened by @charliermarsh on 2023-02-24 00:40_

In Python, both of these slices have the same AST:

```py
x[::]
x[:]
```

We need a way to differentiate between them, _and_ we need a way to account for comments within empty segments, like:

```py
x[
  :
  # Comment here
  :
]
```

This PR attempts to solve both problems by introducing a new CST node kind, `SliceIndex`, which can either be an expression or empty. The final segment of a `Slice` is now optional; if it's present but empty, we know to include the trailing colon. Since `SliceIndex` is `Located`, we can also attach comments to it.

As an alternative, I also considered creating a new `ExprKind::Placeholder`, which would only be used here. That _might've_ been more succinct, but this felt like less of a dramatic change to the AST structure.


---

_Label `autoformatter` added by @charliermarsh on 2023-02-24 00:40_

---

_Merged by @charliermarsh on 2023-02-24 00:49_

---

_Closed by @charliermarsh on 2023-02-24 00:49_

---

_Branch deleted on 2023-02-24 00:49_

---
