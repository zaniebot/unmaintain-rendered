```yaml
number: 21274
title: Annotations block the FURB101 fix
type: issue
state: closed
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-11-04T15:09:04Z
updated_at: 2025-11-08T00:04:46Z
url: https://github.com/astral-sh/ruff/issues/21274
synced_at: 2026-01-10T11:10:00Z
```

# Annotations block the FURB101 fix

---

_Issue opened by @dscorbett on 2025-11-04 15:09_

### Summary

Annotations block the fix for [`read-whole-file` (FURB101)](https://docs.astral.sh/ruff/rules/read-whole-file/) but shouldn’t. [Example](https://play.ruff.rs/8ab9fa5b-48fc-474c-ae0e-b71b5112dcb3):
```console
$ cat >furb101.py <<'# EOF'
with open("file.txt", encoding="utf-8") as f:
    contents: str = f.read()
# EOF

$ ruff --isolated check furb101.py --select FURB101 --preview --fix --output-format concise
furb101.py:1:6: FURB101 `open` and `read` should be replaced by `Path("file.txt").read_text(encoding="utf-8")`
Found 1 error.
```

The fix should produce:
```python
import pathlib
contents: str = pathlib.Path("file.txt").read_text(encoding="utf-8")
```

### Version

ruff 0.14.3 (8737a2d5f 2025-10-30)

---

_Label `fixes` added by @ntBre on 2025-11-04 15:37_

---

_Closed by @ntBre on 2025-11-08 00:04_

---
