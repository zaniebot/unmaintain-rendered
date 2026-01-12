```yaml
number: 10269
title: Actually use jemalloc
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/use-jemalloc
created_at: 2025-01-02T10:00:53Z
updated_at: 2025-01-02T15:03:17Z
url: https://github.com/astral-sh/uv/pull/10269
synced_at: 2026-01-12T16:09:12Z
```

# Actually use jemalloc

---

_@konstin_

The uv-performance-memory-allocator is currently optimized out at least on musl due to the crate being otherwise unused (https://github.com/rust-lang/rust/issues/64402), causing musl to not use jemalloc and being slow.

Command:
```
cargo build --target x86_64-unknown-linux-musl --profile profiling
hyperfine --warmup 1 --runs 10 --prepare "uv venv -p 3.12" "target/x86_64-unknown-linux-musl/profiling/uv pip compile scripts/requirements/airflow.in"
```

Before:
```
Time (mean ± σ):      1.149 s ±  0.013 s    [User: 1.498 s, System: 0.433 s]
Range (min … max):    1.131 s …  1.173 s    10 runs
```

After:
```
Time (mean ± σ):     552.6 ms ±   4.7 ms    [User: 771.7 ms, System: 197.5 ms]
Range (min … max):   546.4 ms … 561.6 ms    10 runs

---

_Label `performance` added by @konstin on 2025-01-02 10:00_

---

_Renamed from "Konsti/use jemalloc" to "Actually use jemalloc" by @konstin on 2025-01-02 10:01_

---

_@charliermarsh approved on 2025-01-02 14:35_

Do we need to do the same for the zip feature?

---

_Comment by @charliermarsh on 2025-01-02 14:41_

I'm running a dry-run release: https://github.com/astral-sh/uv/actions/runs/12584094167

---

_Merged by @charliermarsh on 2025-01-02 15:03_

---

_Closed by @charliermarsh on 2025-01-02 15:03_

---

_Branch deleted on 2025-01-02 15:03_

---
