---
number: 16487
title: "outdated-version-block (UP036): Flag assignments (or any outdated `sys.version_info`)"
type: issue
state: open
author: Avasam
labels:
  - rule
  - needs-design
assignees: []
created_at: 2025-03-04T00:23:51Z
updated_at: 2025-03-04T11:37:02Z
url: https://github.com/astral-sh/ruff/issues/16487
synced_at: 2026-01-07T13:12:16-06:00
---

# outdated-version-block (UP036): Flag assignments (or any outdated `sys.version_info`)

---

_Issue opened by @Avasam on 2025-03-04 00:23_

### Summary

I found the following code snippet in `pypa/distutils`:

```py
def _pypy_hack(name):
    PY37 = sys.version_info < (3, 8)
    old_pypy = hasattr(sys, 'pypy_version_info') and PY37
    prefix = not name.endswith(('_user', '_home'))
    pypy_name = 'pypy' + '_nt' * (os.name == 'nt')
    return pypy_name if old_pypy and prefix else name
```

And I'd like [outdated-version-block (UP036)](https://docs.astral.sh/ruff/rules/outdated-version-block/#outdated-version-block-up036) (or a similar rule) to flag usage of `sys.version_info < (3, 8)`.
When targetting python 3.9+, in any context this is the same as just writing `False`. Whilst Ruff can't necessarily autofix it *correctly*, it should be flagged for a human to update / remove the legacy code.

---

_Referenced in [astral-sh/ruff#12093](../../astral-sh/ruff/issues/12093.md) on 2025-03-04 10:19_

---

_Label `rule` added by @MichaReiser on 2025-03-04 10:39_

---

_Label `needs-design` added by @MichaReiser on 2025-03-04 10:39_

---

_Comment by @MichaReiser on 2025-03-04 10:41_

This does make sense to me, although I'm not sure if it makes sense to include it into UP036 or if we have to rename and rescope the rule to `unnecessary-version-check` because it is now no longer about just flagging blocks. 


Related to: https://github.com/astral-sh/ruff/issues/12093

---

_Renamed from "outdated-version-block (UP036): Flag assignments (or any outdated `sys.version`)" to "outdated-version-block (UP036): Flag assignments (or any outdated `sys.version_info`)" by @AlexWaygood on 2025-03-04 11:37_

---

_Referenced in [pypa/distutils#348](../../pypa/distutils/pulls/348.md) on 2025-03-19 23:56_

---

_Referenced in [astral-sh/ruff#18375](../../astral-sh/ruff/issues/18375.md) on 2025-05-29 16:47_

---
