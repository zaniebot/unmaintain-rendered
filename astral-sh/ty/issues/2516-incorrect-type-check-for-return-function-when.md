```yaml
number: 2516
title: Incorrect type check for return function, when using polars concat function on a list of LazyFrames
type: issue
state: open
author: danielpopescu
labels: []
assignees: []
created_at: 2026-01-15T12:56:15Z
updated_at: 2026-01-15T12:59:04Z
url: https://github.com/astral-sh/ty/issues/2516
synced_at: 2026-01-15T13:51:48Z
```

# Incorrect type check for return function, when using polars concat function on a list of LazyFrames

---

_@danielpopescu_

### Summary

```py
import polars as pl

def foo() -> pl.LazyFrame:
    paths = ["/tmp/some_path1", "/tmp/some_path2"]
    return pl.concat([pl.scan_csv(p) for p in paths])

```

ty thinks the return type should be pl.DataFrame | pl.LazyFrame as opposed to just pl.LazyFrame.

### Version

_No response_

---

_Renamed from "Incorrect type check for return function" to "Incorrect type check for return function, when using polars concat function on a list of LazyFrames" by @danielpopescu on 2026-01-15 12:59_

---
