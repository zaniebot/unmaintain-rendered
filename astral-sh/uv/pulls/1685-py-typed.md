```yaml
number: 1685
title: Py.typed
type: pull_request
state: closed
author: sbrugman
labels:
  - enhancement
assignees: []
base: main
head: py-typed
created_at: 2024-02-19T11:29:03Z
updated_at: 2024-02-20T08:04:25Z
url: https://github.com/astral-sh/uv/pull/1685
synced_at: 2026-01-12T16:04:41Z
```

# Py.typed

---

_@sbrugman_

Suggested implementation for https://github.com/astral-sh/uv/issues/1677

## Summary

Add `py.typed` + cautionary docstring. 
Importing these functions is discouraged, but not prohibited: users can do so at their own risk.

Included some best practices in the Python part, we should set the right example !

Atomic commits, see commit message for details.

## Test Plan

<!-- How was it tested? -->


---

_@zanieb reviewed on 2024-02-19 15:51_

---

_Review comment by @zanieb on `python/uv/__main__.py`:4 on 2024-02-19 15:51_

We are explicitly not using `pathlib` because it's slower to import

- https://github.com/astral-sh/ruff/pull/9315

---

_Review comment by @zanieb on `python/uv/__init__.py`:6 on 2024-02-19 15:52_

Let's not import then from `__main__`, let's do it the other way around (define them here and import them there).

I think `detect_virtualenv` should remain private as well.

---

_@zanieb reviewed on 2024-02-19 15:52_

---

_@zanieb reviewed on 2024-02-19 15:52_

---

_Review comment by @zanieb on `python/uv/__main__.py`:4 on 2024-02-19 15:52_

Can you revert those changes?

---

_@zanieb reviewed on 2024-02-19 15:53_

---

_Review comment by @zanieb on `python/uv/__init__.py`:4 on 2024-02-19 15:53_

```suggestion
The Python API of `uv` is not guaranteed to be stable and may change at any time.
```

Fixed a typo and this seems sufficient. I was only strongly discouraging people from importing from `__main__`.

---

_Label `enhancement` added by @zanieb on 2024-02-19 15:53_

---

_Comment by @sbrugman on 2024-02-20 08:04_

Superseded by https://github.com/astral-sh/uv/pull/1728

---

_Closed by @sbrugman on 2024-02-20 08:04_

---
