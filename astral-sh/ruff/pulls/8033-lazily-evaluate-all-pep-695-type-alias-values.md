```yaml
number: 8033
title: Lazily evaluate all PEP 695 type alias values
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/PEP
created_at: 2023-10-18T01:23:52Z
updated_at: 2023-10-18T01:50:27Z
url: https://github.com/astral-sh/ruff/pull/8033
synced_at: 2026-01-12T15:55:25Z
```

# Lazily evaluate all PEP 695 type alias values

---

_@charliermarsh_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In https://github.com/astral-sh/ruff/pull/7968, I introduced a regression whereby we started to treat imports used _only_ in type annotation bounds (with `__future__` annotations) as unused.

The root of the issue is that I started using `visit_annotation` for these bounds. So we'd queue up the bound in the list of deferred type parameters, then when visiting, we'd further queue it up in the list of deferred type annotations... Which we'd then never visit, since deferred type annotations are visited _before_ deferred type parameters.

Anyway, the better solution here is to use a dedicated flag for these, since they have slightly different behavior than type annotations.

I've also fixed what I _think_ is a bug whereby we previously failed to resolve `Callable` in:

```python
type RecordCallback[R: Record] = Callable[[R], None]

from collections.abc import Callable
```

IIUC, the values in type aliases should be evaluated lazily, like type parameters.

Closes https://github.com/astral-sh/ruff/issues/8017.

## Test Plan

`cargo test`


---

_Review requested from @zanieb by @charliermarsh on 2023-10-18 01:23_

---

_Label `bug` added by @charliermarsh on 2023-10-18 01:23_

---

_@charliermarsh reviewed on 2023-10-18 01:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_20.py`:3 on 2023-10-18 01:26_

`Record` should be undefined, but `Callable` should resolve.

---

_@charliermarsh reviewed on 2023-10-18 01:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_19.py`:11 on 2023-10-18 01:26_

Both of these should be considered "used".

---

_Comment by @github-actions[bot] on 2023-10-18 01:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-10-18 01:45_

Thanks for fixing this! I was going to look into it but ya beat me to it (which is good because I would have been confused)

---

_Merged by @charliermarsh on 2023-10-18 01:50_

---

_Closed by @charliermarsh on 2023-10-18 01:50_

---

_Branch deleted on 2023-10-18 01:50_

---
