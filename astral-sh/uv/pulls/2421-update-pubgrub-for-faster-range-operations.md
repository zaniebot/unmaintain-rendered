```yaml
number: 2421
title: Update pubgrub for faster range operations
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/pubgrub
created_at: 2024-03-13T18:57:06Z
updated_at: 2024-03-13T22:48:25Z
url: https://github.com/astral-sh/uv/pull/2421
synced_at: 2026-01-10T14:49:08Z
```

# Update pubgrub for faster range operations

---

_Pull request opened by @konstin on 2024-03-13 18:57_

This update pulls in https://github.com/pubgrub-rs/pubgrub/pull/177, optimizing common range operations in pubgrub. Please refer to this PR for a more extensive description and discussion of the changes.

The changes optimize that last remaining pathological case, `bio_embeddings[all]` on python 3.12, which has to try 100k versions, from 12s to 3s in the cached case. It should also enable smarter prefetching in batches (https://github.com/astral-sh/uv/issues/170), even though a naive attempt didn't show better network usage.

**before** 12s

![image](https://github.com/pubgrub-rs/pubgrub/assets/6826232/80ffdc49-1159-453d-a3ea-0dd431df6d3b)

**after** 3s

![image](https://github.com/pubgrub-rs/pubgrub/assets/6826232/69508c29-73ab-4593-a588-d8c722242513)

```
$  taskset -c 0 hyperfine --warmup 1 "../uv/target/profiling/main-uv pip compile ../uv/scripts/requirements/bio_embeddings.in"  "../uv/target/profiling/branch-uv pip compile ../uv/scripts/requirements/bio_embeddings.in"
Benchmark 1: ../uv/target/profiling/main-uv pip compile ../uv/scripts/requirements/bio_embeddings.in
  Time (mean ± σ):     12.321 s ±  0.064 s    [User: 12.014 s, System: 0.300 s]
  Range (min … max):   12.224 s … 12.406 s    10 runs

Benchmark 2: ../uv/target/profiling/branch-uv pip compile ../uv/scripts/requirements/bio_embeddings.in
  Time (mean ± σ):      3.109 s ±  0.004 s    [User: 2.782 s, System: 0.321 s]
  Range (min … max):    3.103 s …  3.116 s    10 runs

Summary
  ../uv/target/profiling/branch-uv pip compile ../uv/scripts/requirements/bio_embeddings.in ran
    3.96 ± 0.02 times faster than ../uv/target/profiling/main-uv pip compile ../uv/scripts/requirements/bio_embeddings.in
```

It also adds `bio_embeddings[all]` as a requirements test case.

---

_Label `performance` added by @konstin on 2024-03-13 18:57_

---

_@zanieb approved on 2024-03-13 18:58_

---

_Comment by @charliermarsh on 2024-03-13 18:59_

> The changes optimize that last remaining pathological case, bio_embeddings[all] on python 3.12, which has to try 100k versions, from 12s to 3s in the cached case.

Lol

---

_Merged by @zanieb on 2024-03-13 22:48_

---

_Closed by @zanieb on 2024-03-13 22:48_

---

_Branch deleted on 2024-03-13 22:48_

---
