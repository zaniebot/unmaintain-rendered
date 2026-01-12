```yaml
number: 7427
title: "Prefetch all packages in `uv lock --upgrade`"
type: pull_request
state: closed
author: konstin
labels:
  - enhancement
  - performance
assignees: []
draft: true
base: main
head: konsti/prefetch-packages-in-upgrade
created_at: 2024-09-16T13:52:15Z
updated_at: 2025-01-10T13:54:06Z
url: https://github.com/astral-sh/uv/pull/7427
synced_at: 2026-01-12T16:07:49Z
```

# Prefetch all packages in `uv lock --upgrade`

---

_@konstin_

When we upgrade the lockfile, we know that we have to fetch all packages. We can speed this up by queueing all packages from the lockfile eagerly.

The disadvantage is that we block the queue until all of them are done. I tried adding a priority and a prefetch lock, but that regressed performance.

I get the impression we could speed up uv esp. for the cache-evicted case by adding priorities to the requests.

## Benchmark

Small data science project:

```toml
[project]
name = "data-science"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
  "beautifulsoup4>=4.12.3,<5",
  "httpx>=0.27.0,<0.28",
  "matplotlib>=3.9.1,<4",
  "numpy>=2.0.1,<3",
  "pandas>=2.2.2,<3",
  "pydantic>=2.8.2",
  "scikit-learn>=1.5.1,<2",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
dev-dependencies = [
  "ruff<=0.6.2,<0.7",
  "jupyter>=1.0.0,<2.0.0",
]
```

```
$ hyperfine --prepare "sleep 5" --runs 20 "./uv lock --upgrade" "uv lock --upgrade"
Benchmark 1: ./uv lock --upgrade
  Time (mean ± σ):     239.3 ms ±  68.3 ms    [User: 76.0 ms, System: 127.9 ms]
  Range (min … max):   193.0 ms … 493.5 ms    20 runs

Benchmark 2: uv lock --upgrade
  Time (mean ± σ):     320.0 ms ±  31.1 ms    [User: 66.5 ms, System: 90.9 ms]
  Range (min … max):   291.5 ms … 382.0 ms    20 runs

Summary
  ./uv lock --upgrade ran
    1.34 ± 0.40 times faster than uv lock --upgrade
```

transformers:

```
$ hyperfine --prepare "sleep 10" --runs 100 "~/projects/uv/target/release/uv lock --upgrade" "uv lock --upgrade"
Benchmark 1: ~/projects/uv/target/release/uv lock --upgrade
  Time (mean ± σ):     572.3 ms ± 128.1 ms    [User: 322.8 ms, System: 264.6 ms]
  Range (min … max):   439.7 ms … 1007.1 ms    100 runs
 
Benchmark 2: uv lock --upgrade
  Time (mean ± σ):     729.7 ms ± 124.4 ms    [User: 324.7 ms, System: 239.4 ms]
  Range (min … max):   570.7 ms … 1131.7 ms    100 runs
 
Summary
  ~/projects/uv/target/release/uv lock --upgrade ran
    1.27 ± 0.36 times faster than uv lock --upgrade
```

Fixes #1206

---

_Label `enhancement` added by @konstin on 2024-09-16 13:52_

---

_Label `performance` added by @konstin on 2024-09-16 13:52_

---

_Comment by @charliermarsh on 2024-09-23 23:20_

@konstin -- Is this intended to be reviewed, or blocked on priority queueing?

---

_Converted to draft by @konstin on 2024-09-24 09:09_

---

_Comment by @konstin on 2024-09-24 09:10_

I'll try if i can turn this into a better abstraction

---

_Comment by @konstin on 2025-01-10 13:54_

I can't get a measurable effect anymore.

---

_Closed by @konstin on 2025-01-10 13:54_

---
