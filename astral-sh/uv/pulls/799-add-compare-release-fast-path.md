```yaml
number: 799
title: Add compare_release fast path
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/add-compare_release-fast-path
created_at: 2024-01-05T18:21:37Z
updated_at: 2024-01-05T20:14:12Z
url: https://github.com/astral-sh/uv/pull/799
synced_at: 2026-01-12T16:04:12Z
```

# Add compare_release fast path

---

_@konstin_

Looking at the profile for tf-models-nightly after #789, `compare_release` is the single biggest item. Adding a fast path, we avoid paying the cost for padding releases with 0s when they are the same length, resulting in a 16% for this pathological case. Note that this mainly happens because tf-models-nightly is almost all large dev releases that hit the slow path.

**Before**

![image](https://github.com/astral-sh/puffin/assets/6826232/0d2b4553-da69-4cdb-966b-0894a6dd5d94)

**After**

![image](https://github.com/astral-sh/puffin/assets/6826232/6d484808-9d16-408d-823e-a12d321802a5)

```
$ hyperfine --warmup 1 --runs 3 "target/profiling/main pip-compile -q scripts/requirements/tf-models-nightly.txt"
 "target/profiling/puffin pip-compile -q scripts/requirements/tf-models-nightly.txt"
Benchmark 1: target/profiling/main pip-compile -q scripts/requirements/tf-models-nightly.txt
  Time (mean ± σ):     11.963 s ±  0.225 s    [User: 11.478 s, System: 0.451 s]
  Range (min … max):   11.747 s … 12.196 s    3 runs

Benchmark 2: target/profiling/puffin pip-compile -q scripts/requirements/tf-models-nightly.txt
  Time (mean ± σ):     10.317 s ±  0.720 s    [User: 9.885 s, System: 0.404 s]
  Range (min … max):    9.501 s … 10.860 s    3 runs

Summary
  target/profiling/puffin pip-compile -q scripts/requirements/tf-models-nightly.txt ran
    1.16 ± 0.08 times faster than target/profiling/main pip-compile -q scripts/requirements/tf-models-nightly.txt
```

---

_@charliermarsh approved on 2024-01-05 18:23_

Nice!

---

_@zanieb approved on 2024-01-05 18:52_

---

_@BurntSushi approved on 2024-01-05 19:06_

w00t

---

_Merged by @charliermarsh on 2024-01-05 20:14_

---

_Closed by @charliermarsh on 2024-01-05 20:14_

---

_Branch deleted on 2024-01-05 20:14_

---
