```yaml
number: 2056
title: Failing type inference after sorted
type: issue
state: closed
author: bvolkmer
labels: []
assignees: []
created_at: 2025-12-18T09:42:15Z
updated_at: 2025-12-18T09:46:22Z
url: https://github.com/astral-sh/ty/issues/2056
synced_at: 2026-01-10T01:53:59Z
```

# Failing type inference after sorted

---

_Issue opened by @bvolkmer on 2025-12-18 09:42_

### Summary

Minimal example:
```python
from pathlib import Path
from typing import reveal_type

foo: list[Path] = [Path("a"), Path("b"), Path("c")]
bar = sorted(foo, key=str)
reveal_type(foo)
reveal_type(sorted)
reveal_type(bar) 
```
For some reason, bar is revealed as `list[Path | Buffer]` where it should be `list[Path]`. It does not happen if `key=str` is omitted, even though also in this case the function generic of the return type depends on the input iterable: `(iterable: Iterable[_T@sorted], *, /, key: (_T@sorted, /) -> SupportsDunderLT[Any] | SupportsDunderGT[Any], reverse: bool = False) -> list[_T@sorted]]`

### Version

ty 0.0.3

---

_Comment by @sharkdp on 2025-12-18 09:46_

Thank you for reporting this. I think this is related to #1714, which we hope to address soon.

---

_Closed by @sharkdp on 2025-12-18 09:46_

---
