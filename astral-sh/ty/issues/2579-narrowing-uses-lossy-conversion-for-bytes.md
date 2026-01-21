```yaml
number: 2579
title: "Narrowing uses lossy conversion for `bytes`"
type: issue
state: open
author: dscorbett
labels: []
assignees: []
created_at: 2026-01-21T16:05:07Z
updated_at: 2026-01-21T16:05:07Z
url: https://github.com/astral-sh/ty/issues/2579
synced_at: 2026-01-21T17:03:34Z
```

# Narrowing uses lossy conversion for `bytes`

---

_@dscorbett_

### Summary

Narrowing a non-UTF-8 `bytes` subscript also narrows a different `bytes` subscript if [`String::from_utf8_lossy`](https://doc.rust-lang.org/std/string/struct.String.html#method.from_utf8_lossy) converts them to the same string. Narrowing should not use lossy conversion. [Example](https://play.ty.dev/f4d1c149-3442-467b-a548-585a46d717c3):
```console
$ cat >bug.py <<'# EOF'
from typing import reveal_type
def _(x: dict[bytes, int | None]):
    if x[b"\xaa"] is not None:
        reveal_type(x[b"\xff"])
# EOF

$ ty check --output-format concise bug.py
bug.py:4:21: info[revealed-type] Revealed type: `int`
Found 1 diagnostic
```

### Version

ty 0.0.13 (fc1478bd9 2026-01-21)

---
