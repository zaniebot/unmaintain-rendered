```yaml
number: 9315
title: Improve responsiveness when invoked via Python
type: pull_request
state: merged
author: ofek
labels:
  - cli
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-12-29T20:39:22Z
updated_at: 2023-12-31T16:36:46Z
url: https://github.com/astral-sh/ruff/pull/9315
synced_at: 2026-01-10T23:07:18Z
```

# Improve responsiveness when invoked via Python

---

_Pull request opened by @ofek on 2023-12-29 20:39_

This saves a handful of milliseconds on Windows and even more on other platforms when running `python -m ruff`. On non-Windows systems the process is replaced directly (impossible on Windows unfortunately).

```
❯ docker run --rm python:3.11 bash -c "for i in {1..15}; do python -m timeit -n 1 -r 1 'from pathlib import Path'; done"
1 loop, best of 1: 25.7 msec per loop
1 loop, best of 1: 3.07 msec per loop
1 loop, best of 1: 3.16 msec per loop
1 loop, best of 1: 3.06 msec per loop
1 loop, best of 1: 3.32 msec per loop
1 loop, best of 1: 3.93 msec per loop
1 loop, best of 1: 3.26 msec per loop
1 loop, best of 1: 3.73 msec per loop
1 loop, best of 1: 3.1 msec per loop
1 loop, best of 1: 3.29 msec per loop
1 loop, best of 1: 3.12 msec per loop
1 loop, best of 1: 3.05 msec per loop
1 loop, best of 1: 3.04 msec per loop
1 loop, best of 1: 3.19 msec per loop
1 loop, best of 1: 3.04 msec per loop

❯ docker run --rm python:3.11 bash -c "for i in {1..15}; do python -m timeit -n 1 -r 1 'import subprocess'; done"
1 loop, best of 1: 31.2 msec per loop
1 loop, best of 1: 3.75 msec per loop
1 loop, best of 1: 4.71 msec per loop
1 loop, best of 1: 3.88 msec per loop
1 loop, best of 1: 4.08 msec per loop
1 loop, best of 1: 4.35 msec per loop
1 loop, best of 1: 3.94 msec per loop
1 loop, best of 1: 4.06 msec per loop
1 loop, best of 1: 3.88 msec per loop
1 loop, best of 1: 3.85 msec per loop
1 loop, best of 1: 3.84 msec per loop
1 loop, best of 1: 4.01 msec per loop
1 loop, best of 1: 4.21 msec per loop
1 loop, best of 1: 4.07 msec per loop
1 loop, best of 1: 4.11 msec per loop

❯ python -m timeit -n 1 -r 1 "from pathlib import Path"
1 loop, best of 1: 5.25 msec per loop

❯ python -m timeit -n 1 -r 1 "import subprocess"
1 loop, best of 1: 7.61 msec per loop
```

---

_@ofek reviewed on 2023-12-29 20:41_

---

_Review comment by @ofek on `python/ruff/__main__.py`:4 on 2023-12-29 20:41_

```suggestion


```

---

_Comment by @zanieb on 2023-12-29 20:42_

Hey :)

Can you show the performance changes for invoking Ruff itself rather than microbenchmarks of the import? It's hard to tell if these changes are worthwhile in our use-case. I generally prefer the readability of undeferred imports and use of `Path` when it's not critical.

---

_Comment by @ofek on 2023-12-29 20:47_

```
❯ hyperfine -m 10 --warmup 1 "python -m ruff --help"
Benchmark 1: python -m ruff --help
  Time (mean ± σ):      78.3 ms ±   1.8 ms    [User: 0.0 ms, System: 0.0 ms]
  Range (min … max):    75.7 ms …  83.3 ms    34 runs

❯ hyperfine -m 10 --warmup 1 "python -m ruff --help"
Benchmark 1: python -m ruff --help
  Time (mean ± σ):      73.4 ms ±   1.0 ms    [User: 0.0 ms, System: 1.2 ms]
  Range (min … max):    71.7 ms …  75.7 ms    36 runs
```

I can't easily test right now in Docker, these are on my Windows machine.

---

_Comment by @github-actions[bot] on 2023-12-29 20:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2023-12-29 22:21_

This seems reasonable to me.

---

_Label `cli` added by @charliermarsh on 2023-12-29 22:22_

---

_Merged by @charliermarsh on 2023-12-31 12:11_

---

_Closed by @charliermarsh on 2023-12-31 12:11_

---

_Branch deleted on 2023-12-31 16:36_

---
