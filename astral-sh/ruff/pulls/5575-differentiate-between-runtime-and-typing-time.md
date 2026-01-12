```yaml
number: 5575
title: Differentiate between runtime and typing-time annotations
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/runtime-annotation
created_at: 2023-07-07T02:01:14Z
updated_at: 2023-07-07T04:21:46Z
url: https://github.com/astral-sh/ruff/pull/5575
synced_at: 2026-01-12T03:36:55Z
```

# Differentiate between runtime and typing-time annotations

---

_Pull request opened by @charliermarsh on 2023-07-07 02:01_

## Summary

In Python, the annotations on `x` and `y` here have very different treatment:

```python
def foo(x: int):
  y: int
```

The `int` in `x: int` is a runtime-required annotation, because `x` gets added to the function's `__annotations__`. You'll notice, for example, that this fails:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
  from foo import Bar

def f(x: Bar):
  ...
```

Because `Bar` is required to be available at runtime, not just at typing time. Meanwhile, this succeeds:

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
  from foo import Bar

def f():
  x: Bar = 1

f()
```

(Both cases are fine if you use `from __future__ import annotations`.)

Historically, we've tracked those annotations that are _not_ runtime-required via the semantic model's `ANNOTATION` flag. But annotations that _are_ runtime-required have been treated as "type definitions" that aren't annotations.

This causes problems for the flake8-future-annotations rules, which try to detect whether adding `from __future__ import annotations` would _allow_ you to rewrite a type annotation. We need to know whether we're in _any_ type annotation, runtime-required or not, since adding `from __future__ import annotations` will convert any runtime-required annotation to a typing-only annotation.

This PR adds separate state to track these runtime-required annotations. The changes in the test fixtures are correct -- these were false negatives before.

Closes https://github.com/astral-sh/ruff/issues/5574.


---

_Label `bug` added by @charliermarsh on 2023-07-07 02:01_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-07 02:20_

---

_Review requested from @zanieb by @charliermarsh on 2023-07-07 02:21_

---

_@zanieb approved on 2023-07-07 04:01_

---

_Merged by @charliermarsh on 2023-07-07 04:21_

---

_Closed by @charliermarsh on 2023-07-07 04:21_

---

_Branch deleted on 2023-07-07 04:21_

---
