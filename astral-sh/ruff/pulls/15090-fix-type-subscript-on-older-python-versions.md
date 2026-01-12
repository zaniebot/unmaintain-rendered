```yaml
number: 15090
title: Fix type subscript on older python versions
type: pull_request
state: merged
author: aaron-skydio
labels:
  - bug
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-12-21T02:43:33Z
updated_at: 2024-12-22T19:38:04Z
url: https://github.com/astral-sh/ruff/pull/15090
synced_at: 2026-01-12T15:55:50Z
```

# Fix type subscript on older python versions

---

_@aaron-skydio_

## Summary

Python versions before Python 3.9 don't have subscriptable containers like `list`, which is being used in the annotation of `get_last_three_path_parts` here.  I believe Ruff supports Python 3.7 and Python 3.8?  In that case this needs to either use the container from the `typing` module, or use deferred annotations with `from __future__ import annotations` like I'm proposing here

Introduced by https://github.com/astral-sh/ruff/commit/60a2dc53e7b4407c3c0c14093fd8542879997ca8

## Test Plan

Currently the code path in `find_ruff_bin` that defines `get_last_three_path_parts` doesn't work on Python 3.8 or 3.9, it throws this:

```
>>> ruff.__main__.find_ruff_bin()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/aaron/miniconda3/envs/symforce/lib/python3.8/site-packages/ruff/__main__.py", line 77, in find_ruff_bin
    raise FileNotFoundError(scripts_path)
FileNotFoundError: /home/aaron/miniconda3/envs/symforce/bin/ruff
```

With this change it does work

---

_Comment by @github-actions[bot] on 2024-12-21 02:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2024-12-21 14:22_

Thanks for PRing this fix

@charliermarsh would you mind taking a look at this PR? I normally would ask Alex but he's out and I'm probably not the best person to review this change :)

---

_Label `bug` added by @MichaReiser on 2024-12-21 14:22_

---

_Comment by @MichaReiser on 2024-12-21 14:27_

I guess that's actually fine. Even I understand why this is needed with a summary as good as this one. Thank you

---

_Merged by @MichaReiser on 2024-12-21 14:28_

---

_Closed by @MichaReiser on 2024-12-21 14:28_

---

_Comment by @charliermarsh on 2024-12-21 14:39_

Yeah this looks correct to me.

---

_Branch deleted on 2024-12-22 19:38_

---
