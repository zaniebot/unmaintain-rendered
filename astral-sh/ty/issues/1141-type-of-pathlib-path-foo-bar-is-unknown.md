```yaml
number: 1141
title: "Type of pathlib.Path(\"foo\") / \"bar\" is unknown"
type: issue
state: closed
author: ibrokemypie
labels: []
assignees: []
created_at: 2025-09-06T16:20:24Z
updated_at: 2025-09-06T17:53:30Z
url: https://github.com/astral-sh/ty/issues/1141
synced_at: 2026-01-12T15:54:24Z
```

# Type of pathlib.Path("foo") / "bar" is unknown

---

_@ibrokemypie_

### Summary

Appending to a pathlib path with `/` results in the new type being unknown.

Did a bit of searching and wasn't able to find this reported already.

```python
from pathlib import Path

path_one: Path = Path("foo")
path_two: Unknown = path_one / "bar"
```

[Playground link](https://play.ty.dev/6f5adeeb-898e-4ed4-9da8-5589d83e7eb6)

### Version

ty 0.0.1-alpha.20   

---

_Renamed from "Type of pathlib.Path("foo") / "bar" is unkown" to "Type of pathlib.Path("foo") / "bar" is unknown" by @ibrokemypie on 2025-09-06 16:22_

---

_Comment by @sharkdp on 2025-09-06 17:53_

Thank you for reporting this. The problem is that [`PurePath.__truediv__` is annotated with a return-type of `Self`](https://github.com/python/typeshed/blob/6795f430a4fa029feda5b2bcf4500fbdb1eb3884/stdlib/pathlib/__init__.pyi#L89-L90), which we currently don't understand yet. We hope to solve that problem soon (see #159 and https://github.com/astral-sh/ruff/pull/18007).

---

_Closed by @sharkdp on 2025-09-06 17:53_

---
