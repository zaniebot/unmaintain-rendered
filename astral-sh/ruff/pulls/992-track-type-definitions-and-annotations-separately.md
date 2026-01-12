```yaml
number: 992
title: Track type definitions and annotations separately
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/runtime-typing
created_at: 2022-12-02T03:02:15Z
updated_at: 2022-12-02T03:31:21Z
url: https://github.com/astral-sh/ruff/pull/992
synced_at: 2026-01-12T05:48:46Z
```

# Track type definitions and annotations separately

---

_Pull request opened by @charliermarsh on 2022-12-02 03:02_

Previously, we used `self.in_annotation` to detect that we're in a type definition (which implies that, for example, strings need to be treated as forward annotations). We _also_ used this to enable rewriting type annotations when `from __future__ import annotations` was imported pre-Python 3.9 or 3.10 (which can only be safely rewritten when used as annotations, and not as runtime constructs).

This PR splits that tracking into two separate pieces of state: one to track whether we're in an _actual_ annotation (as defined by the AST), and another to track whether we're in a type definition.

Resolves #979.


---

_Merged by @charliermarsh on 2022-12-02 03:31_

---

_Closed by @charliermarsh on 2022-12-02 03:31_

---

_Branch deleted on 2022-12-02 03:31_

---
