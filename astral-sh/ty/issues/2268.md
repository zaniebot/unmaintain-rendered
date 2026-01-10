---
number: 2268
title: can not infer return type of polars scan_parquet
type: issue
state: closed
author: Liyixin95
labels: []
assignees: []
created_at: 2025-12-30T01:07:51Z
updated_at: 2025-12-30T01:47:49Z
url: https://github.com/astral-sh/ty/issues/2268
synced_at: 2026-01-10T01:51:14Z
---

# can not infer return type of polars scan_parquet

---

_Issue opened by @Liyixin95 on 2025-12-30 01:07_

### Summary
reproduce code:

```
import polars as pl

lf = pl.scan_parquet("")
```

### Version

ty 0.0.8

---

_Comment by @carljm on 2025-12-30 01:47_

Thanks for reporting! This is #1136 (due to the `deprecate_renamed_parameter` decorator).

---

_Closed by @carljm on 2025-12-30 01:47_

---
