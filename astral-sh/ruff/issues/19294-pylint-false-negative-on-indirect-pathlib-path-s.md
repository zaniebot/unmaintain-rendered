```yaml
number: 19294
title: "[`pylint`] False negative on indirect `pathlib.Path`s (`PLW1514`)"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - rule
assignees: []
created_at: 2025-07-11T21:45:46Z
updated_at: 2025-07-16T16:57:33Z
url: https://github.com/astral-sh/ruff/issues/19294
synced_at: 2026-01-12T15:54:56Z
```

# [`pylint`] False negative on indirect `pathlib.Path`s (`PLW1514`)

---

_@MeGaGiGaGon_

### Summary

[unspecified-encoding (PLW1514)](https://docs.astral.sh/ruff/rules/unspecified-encoding/#unspecified-encoding-plw1514) has false negatives if a `pathlib.Path` is opened through a variable: https://play.ruff.rs/9a30291c-642c-413f-be9a-1b82db550daf
```py
from pathlib import Path

file = Path().open()  # True positive

path = Path()
file = path.open()  # False negative
```
This mainly applies to functions, since I ran into this case that would have been nice to be warned about:
```py
def format_file(file: Path):
    with file.open() as f:
        contents = f.read()
```

### Version

ruff 0.12.3 (5bc81f26c 2025-07-11) + playground

---

_Label `rule` added by @MichaReiser on 2025-07-14 09:18_

---

_Closed by @ntBre on 2025-07-16 16:57_

---
