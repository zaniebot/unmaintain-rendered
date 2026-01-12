```yaml
number: 8496
title: "E701 is not listed on the \"Conflicting lint rules\" page but it conflicts with `ruff format` when `--preview` flag is used"
type: issue
state: closed
author: simon-liebehenschel
labels:
  - documentation
assignees: []
created_at: 2023-11-05T14:56:26Z
updated_at: 2023-11-06T00:42:36Z
url: https://github.com/astral-sh/ruff/issues/8496
synced_at: 2026-01-12T15:54:48Z
```

# E701 is not listed on the "Conflicting lint rules" page but it conflicts with `ruff format` when `--preview` flag is used

---

_@simon-liebehenschel_

## The original code

```py
from contextlib import nullcontext

ctx = nullcontext()
with ctx:
    ...
```

## Steps to reproduce

`ruff format --isolated --preview sample.py` moves ellipsis:

```py
from contextlib import nullcontext

ctx = nullcontext()
with ctx: ...
```

and now `ruff --isolated --fix --preview sample.py` will complain about `:` and ellipsis on a same line:

```
sample.py:4:9: E701 Multiple statements on one line (colon)
Found 1 error.
```

## Expected behavior 

No conflicts between `ruff check` and `ruff format`.


## Versions

- ruff 0.1.4
- Python 3.11.6


## Related

In the comment a developer assumed that `E701` is not conflicting https://github.com/astral-sh/ruff/pull/8088/files#r1366486452

---

_Comment by @charliermarsh on 2023-11-05 15:08_

Thanks, it should now be marked as conflicting as of that change to preview style.

---

_Label `documentation` added by @charliermarsh on 2023-11-05 15:08_

---

_Comment by @charliermarsh on 2023-11-05 20:10_

Actually, I think we should just allow this in `E701`.

---

_Comment by @charliermarsh on 2023-11-05 20:11_

Oh, we do allow it in some cases (https://github.com/astral-sh/ruff/pull/2837).

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-05 20:11_

---

_Closed by @charliermarsh on 2023-11-06 00:42_

---
