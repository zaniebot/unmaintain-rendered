---
number: 497
title: Investigate slow resolutions
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2023-11-24T10:44:21Z
updated_at: 2024-01-09T09:40:43Z
url: https://github.com/astral-sh/uv/issues/497
synced_at: 2026-01-07T13:12:16-06:00
---

# Investigate slow resolutions

---

_Issue opened by @konstin on 2023-11-24 10:44_

Looking at the top 8k pypi projects with

```
cargo run --profile profiling --bin puffin-dev -- resolve-many --cache-dir cache-no-build --no-build scripts/resolve/pypi_top_8k_flat.txt
```

we find 

```
x-transformers 5659ms
ultralytics 4748ms
azureml-dataprep 4233ms
dbnd-spark 2009ms
autogluon-tabular 859ms
```

Note that the whole process takes <11s for 5.8k projects that don't have source dists at 32 cores

---

_Label `performance` added by @konstin on 2023-11-24 10:44_

---

_Comment by @charliermarsh on 2024-01-09 02:34_

@konstin - Do you want to keep this open?

---

_Closed by @konstin on 2024-01-09 09:39_

---

_Comment by @konstin on 2024-01-09 09:40_

We've broken those down (and partially fixed) into pubgrub improvements and prefetching.

---
