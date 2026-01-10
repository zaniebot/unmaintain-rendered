```yaml
number: 7161
title: "Use path file instead of `sitecustomize.py`"
type: pull_request
state: merged
author: bluss
labels:
  - bug
assignees: []
merged: true
base: main
head: sitecustomize-to-path
created_at: 2024-09-07T09:38:27Z
updated_at: 2024-10-08T12:37:31Z
url: https://github.com/astral-sh/uv/pull/7161
synced_at: 2026-01-10T12:53:41Z
```

# Use path file instead of `sitecustomize.py`

---

_Pull request opened by @bluss on 2024-09-07 09:38_

## Summary

Use a path file (`.pth`) instead of `sitecustomize.py` for configuring path in emphemeral virtualenvs, overlaying the ephemeral venv on top of the base `.venv`.

`sitecustomize.py` is a module in the python installation and as such a unique resource - homebrew pythons on macos already install such a file and thus uv's `sitecustomize.py`, placed in the ephemeral env, did not have any effect.

I don't find any documentation explicitly saying that addsitedir is valid in `.pth` files but from trial it seems to be - and there is the precedent of the existing _virtualenv.pth _virtualenv.py pair that do nontrivial operations.

## Test Plan

- Testing on ephemeral venv, resolving to base venv including editable install in base: done (py3.7, 3.12)
- Testing on homebrew python/macos: done (py3.11)
- tests: run_editable

Fixes #7152

---

_Review requested from @charliermarsh by @zanieb on 2024-09-07 16:08_

---

_@carljm approved on 2024-09-08 22:12_

This is a good idea, and it looks good to me. Thanks for the fix!

---

_Merged by @charliermarsh on 2024-09-09 13:19_

---

_Closed by @charliermarsh on 2024-09-09 13:19_

---

_Label `bug` added by @charliermarsh on 2024-09-09 13:19_

---

_Branch deleted on 2024-09-09 18:21_

---

_Comment by @zanieb on 2024-10-08 12:37_

Cross-linking in case this becomes a problem 

- https://github.com/python/cpython/issues/77102

---
