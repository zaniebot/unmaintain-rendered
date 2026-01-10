```yaml
number: 19004
title: Fix semantic model symbol resolution in classes
type: pull_request
state: closed
author: LaBatata101
labels: []
assignees: []
draft: true
base: main
head: fix-binding
created_at: 2025-06-27T22:25:59Z
updated_at: 2025-12-10T17:43:25Z
url: https://github.com/astral-sh/ruff/pull/19004
synced_at: 2026-01-10T16:42:11Z
```

# Fix semantic model symbol resolution in classes

---

_Pull request opened by @LaBatata101 on 2025-06-27 22:25_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
The semantic model was not correctly resolving symbols inside class scopes. It didn't implement this part of the [docs](https://docs.python.org/3/reference/executionmodel.html#naming).
>  A class definition is an executable statement that may use and define names. These references follow the normal rules for name resolution with an exception that unbound local variables are looked up in the global namespace. The namespace of the class definition becomes the attribute dictionary of the class. The scope of names defined in a class block is limited to the class block; it does not extend to the code blocks of methods. This includes comprehensions and generator expressions, but it does not include [annotation scopes](https://docs.python.org/3/reference/executionmodel.html#annotation-scopes), which have access to their enclosing class scopes.

This was causing some false positives/negatives in some rules. The rules affectted were:
- `PYI019` -- false negative in this code:
```python
def shadowed_type():
    type = 1
    class A:
        @classmethod
        def m[S](cls: type[S]) -> S: ... 
```
- `F401` -- false positive in this code:
```python
from sys import version
def f():
    version = 0
    class Nested:
        print(version)
        version = 1
        print(version)
    print(version)
```
- `B905` -- false negative in this code:
```python
def f():
    zip = "outer"
    class Nested:
        zip("A", "B")
        zip = "inner"
``` 

Fixes https://github.com/astral-sh/ruff/issues/18429
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression tests.
<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @LaBatata101 on 2025-06-27 22:26_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-06-28 15:13_

---

_Converted to draft by @LaBatata101 on 2025-06-30 21:50_

---

_Comment by @MichaReiser on 2025-12-10 17:43_

Thanks for your contribution. I'll close this PR because it has become stale. Please don't hesitate to resubmit the PR and reference this PR if you plan to keep working on this feature.

---

_Closed by @MichaReiser on 2025-12-10 17:43_

---
