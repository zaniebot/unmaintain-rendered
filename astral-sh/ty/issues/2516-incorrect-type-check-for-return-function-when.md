```yaml
number: 2516
title: Incorrect type check for return function, when using polars concat function on a list of LazyFrames
type: issue
state: closed
author: danielpopescu
labels: []
assignees: []
created_at: 2026-01-15T12:56:15Z
updated_at: 2026-01-17T02:11:46Z
url: https://github.com/astral-sh/ty/issues/2516
synced_at: 2026-01-17T03:12:55Z
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

_Comment by @carljm on 2026-01-17 02:11_

The root cause of this is #1136 -- the `scan_csv` function is decorated with `@deprecate_renamed_parameter`, which needs #1136 to work correctly.

---

_Closed by @carljm on 2026-01-17 02:11_

---
