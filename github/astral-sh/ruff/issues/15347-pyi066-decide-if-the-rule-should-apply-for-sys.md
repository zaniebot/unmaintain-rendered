---
number: 15347
title: "`PYI066`: Decide if the rule should apply for `sys.version_info` checks in boolean expressions"
type: issue
state: open
author: MichaReiser
labels:
  - rule
  - preview
assignees: []
created_at: 2025-01-08T12:23:11Z
updated_at: 2025-01-08T12:55:28Z
url: https://github.com/astral-sh/ruff/issues/15347
synced_at: 2026-01-07T13:12:16-06:00
---

# `PYI066`: Decide if the rule should apply for `sys.version_info` checks in boolean expressions

---

_Issue opened by @MichaReiser on 2025-01-08 12:23_

We considered stabilizing the behavior change that enables `PYI066` for nono stub files (see https://github.com/astral-sh/ruff/pull/14059) as part of the 0.9 release (https://github.com/astral-sh/ruff/pull/15340) but ultimately decided against it because we found the following two reports in the ecosystem results:

```python
def _stringify_exception(self, exc: BaseException) -> str:
    try:
        notes = getattr(exc, "__notes__", [])
    except KeyError:
        # Workaround for https://github.com/python/cpython/issues/98778 on
        # Python <= 3.9, and some 3.10 and 3.11 patch versions.
        HTTPError = getattr(sys.modules.get("urllib.error", None), "HTTPError", ())
        if sys.version_info < (3, 12) and isinstance(exc, HTTPError):
            notes = []
        else:
            raise
```

```python
    tags = WheelTag.compute_best(["x86_64"], py_api="cp39")
    if sys.version_info < (3, 9) or sys.implementation.name != "cpython":
        assert "macosx_10_10_x86_64" in str(tags)
        assert "abi3" not in str(tags)
        assert "cp39" not in str(tags)
    else:
        assert str(tags) == "cp39-abi3-macosx_10_10_x86_64"
```

While both cases rightfully fall into the scope of the rule, it is fairly opinionated to enforce `PYI066`. We have to decide if the rule should apply in those cases and, if so, if it should apply both for stub files and regular files.

---

_Label `preview` added by @MichaReiser on 2025-01-08 12:23_

---

_Label `rule` added by @MichaReiser on 2025-01-08 12:23_

---

_Referenced in [astral-sh/ruff#15340](../../astral-sh/ruff/pulls/15340.md) on 2025-01-08 12:23_

---

_Renamed from "`PYI066`: Decide if the rule should apply if used in a more complex boolean expression" to "`PYI066`: Decide if the rule should apply for `sys.version` checks in boolean expressions" by @MichaReiser on 2025-01-08 12:24_

---

_Renamed from "`PYI066`: Decide if the rule should apply for `sys.version` checks in boolean expressions" to "`PYI066`: Decide if the rule should apply for `sys.version_info` checks in boolean expressions" by @AlexWaygood on 2025-01-08 12:55_

---
