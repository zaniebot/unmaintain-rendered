```yaml
number: 1285
title: pd.read_parquet renders as an Unknown type.
type: issue
state: closed
author: ExSidius
labels: []
assignees: []
created_at: 2025-09-30T14:24:48Z
updated_at: 2025-09-30T17:19:23Z
url: https://github.com/astral-sh/ty/issues/1285
synced_at: 2026-01-12T15:54:24Z
```

# pd.read_parquet renders as an Unknown type.

---

_@ExSidius_

### Summary

<img width="397" height="135" alt="Image" src="https://github.com/user-attachments/assets/0d7f04e3-36e4-4d56-ba39-d6d0c6b11e29" />

```python
import pandas as pd

df = pd.read_parquet("test.parquet")
```

I'm using uv and have a conventional `pyproject.toml`.

Link to reproduce in the playground - https://play.ty.dev/083fc849-ccc3-4c74-80f4-5f41b3ad9867

### Version

VS Code 2025.43.12620731; ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @sharkdp on 2025-09-30 17:18_

Thank you for reporting this.

We reveal `Unknown` here due to [this `@doc` decorator](https://github.com/pandas-dev/pandas/blob/d8159471b1e6e9327274a1b98aca2b88c9b5e0b5/pandas/io/parquet.py#L503). This issue will be resolved with our new generics solver, which will allow us to properly specialize the generic `Callable[[F], F]` return type (see #623).

---

_Closed by @sharkdp on 2025-09-30 17:18_

---
