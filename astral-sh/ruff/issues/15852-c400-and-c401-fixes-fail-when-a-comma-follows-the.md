```yaml
number: 15852
title: C400 and C401 fixes fail when a comma follows the generator
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-31T13:43:42Z
updated_at: 2025-02-05T13:38:04Z
url: https://github.com/astral-sh/ruff/issues/15852
synced_at: 2026-01-10T11:09:57Z
```

# C400 and C401 fixes fail when a comma follows the generator

---

_Issue opened by @dscorbett on 2025-01-31 13:43_

### Description

The fixes for [`unnecessary-generator-list` (C400)](https://docs.astral.sh/ruff/rules/unnecessary-generator-list/) and [`unnecessary-generator-set` (C401)](https://docs.astral.sh/ruff/rules/unnecessary-generator-set/) introduce syntax errors in Ruff 0.9.4 when a parenthesized generator comprehension is followed by a comma and the generator comprehension cannot be simplified to just the iterable.
```console
$ cat >c40.py <<'# EOF'
list((0 for _ in []),)
set((0 for _ in []),)
# EOF

$ ruff --isolated check --preview --select C400 c40.py --unsafe-fixes --diff                        

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `c40.py`, the rule codes C400, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


$ ruff --isolated check --preview --select C401 c40.py --unsafe-fixes --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `c40.py`, the rule codes C401, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

The syntactically incorrect output is presumably:
```python
[0 for _ in [],]
{0 for _ in [],}
```
(Is there a way to see this output instead of having Ruff revert the changes?)

---

_Label `bug` added by @AlexWaygood on 2025-01-31 16:50_

---

_Label `fixes` added by @AlexWaygood on 2025-01-31 16:50_

---

_Comment by @dylwil3 on 2025-01-31 17:42_

> (Is there a way to see this output instead of having Ruff revert the changes?)

If you apply the quick fix in the Playground it'll make the fix even though it's a syntax error.

---

_Assigned to @dylwil3 by @dylwil3 on 2025-02-03 16:51_

---

_Closed by @dylwil3 on 2025-02-05 13:38_

---
