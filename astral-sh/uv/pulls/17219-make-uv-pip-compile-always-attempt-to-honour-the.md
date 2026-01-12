```yaml
number: 17219
title: "Make `uv pip compile` always attempt to honour the `--python` argument"
type: pull_request
state: open
author: EliteTK
labels:
  - bug
assignees: []
draft: true
base: tk/pip-compile-missing-py-2
head: tk/pip-compile-missing-py-3
created_at: 2025-12-22T19:44:06Z
updated_at: 2025-12-29T13:16:34Z
url: https://github.com/astral-sh/uv/pull/17219
synced_at: 2026-01-12T16:12:39Z
```

# Make `uv pip compile` always attempt to honour the `--python` argument

---

_@EliteTK_

## Summary

Addresses #16709. 

Previous when `uv pip compile --python <ver>` was used and `<ver>` was a "simple Python version request" (i.e. `PythonVersion::from_str` succeeded) then `uv pip compile` would act as if the version was passed using `--python-version` instead (with no `--python`) which in turn meant that it turned into a loose requirement which could be satisfied with `find_best`.

This PR makes `uv pip compile --python <simple-ver>` act like `uv pip compile --python <simple-ver> --python-version <simple-ver>` which I believe is more accurate.

## Test Plan

Existing tests were adjusted since they already covered this case.

---

_Review requested from @zanieb by @EliteTK on 2025-12-22 19:44_

---

_Label `bug` added by @EliteTK on 2025-12-22 19:44_

---

_Converted to draft by @EliteTK on 2025-12-29 13:16_

---
