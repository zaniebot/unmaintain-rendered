```yaml
number: 112
title: switch default Python version to latest
type: issue
state: closed
author: carljm
labels: []
assignees: []
created_at: 2025-04-25T17:55:56Z
updated_at: 2025-05-08T14:58:42Z
url: https://github.com/astral-sh/ty/issues/112
synced_at: 2026-01-12T15:54:22Z
```

# switch default Python version to latest

---

_@carljm_

If we don't find another Python version to use, our last-resort fallback should be the latest supported, not earliest supported, Python version. This minimizes first-experience false positives; if users need to support older Python versions, they can configure that to opt in to stricter checking.

(We can also of course add new version sources that take priority over this default, such as looking at the venv Python version; that's independent of this issue.)

Holding off on this change while we figure out Ruff's python-version handling, in case it influences this choice in some way, but filing this issue into the Alpha milestone so we don't miss it before the alpha.

---

_Renamed from "[red-knot] switch default Python version to latest" to "switch default Python version to latest" by @MichaReiser on 2025-05-07 15:25_

---

_Added to milestone `alpha` by @MichaReiser on 2025-05-07 15:56_

---

_Comment by @MichaReiser on 2025-05-08 07:39_

This is not quiet as simple as changing our initializer to `PythonVersion::latest` because we need to respect any `requires-python` upper bound.

---

_Assigned to @MichaReiser by @MichaReiser on 2025-05-08 07:40_

---

_Closed by @MichaReiser on 2025-05-08 14:58_

---
