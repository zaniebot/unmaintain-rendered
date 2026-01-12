```yaml
number: 9444
title: Use a trie-based router to resolve per-file settings
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
draft: true
base: main
head: charlie/matchit
created_at: 2024-01-09T05:27:39Z
updated_at: 2024-01-09T06:09:39Z
url: https://github.com/astral-sh/ruff/pull/9444
synced_at: 2026-01-12T15:55:28Z
```

# Use a trie-based router to resolve per-file settings

---

_@charliermarsh_

## Summary

This is a small change that I put together a while ago but forgot to clean up. When the formatter is fully cached, it turns out we actually spend meaningful time mapping from file to `Settings` (since we use a hierarchical approach to settings). Using `matchit` rather than `BTreeMap` improves fully-cached performance by anywhere from 2-5% depending on the project, and since these are all implementation details of `Resolver`, it's minimally invasive.

We don't need all the behavior of `matchit`, but I tried other libraries like `radix_trie` and didn't see any speedup vs. `main`, whereas `matchit` shows a consistent improvement.

## Test Plan

```
❯ hyperfine --warmup 20 -i "./target/release/main format ../airflow" "./target/release/ruff format ../airflow"
Benchmark 1: ./target/release/main format ../airflow
  Time (mean ± σ):      73.4 ms ±   1.2 ms    [User: 48.8 ms, System: 76.4 ms]
  Range (min … max):    68.4 ms …  78.2 ms    39 runs

Benchmark 2: ./target/release/ruff format ../airflow
  Time (mean ± σ):      69.7 ms ±   0.9 ms    [User: 41.8 ms, System: 75.7 ms]
  Range (min … max):    68.5 ms …  73.5 ms    42 runs

Summary
  './target/release/ruff format ../airflow' ran
    1.05 ± 0.02 times faster than './target/release/main format ../airflow'
```

---

_Label `performance` added by @charliermarsh on 2024-01-09 05:27_

---

_Converted to draft by @charliermarsh on 2024-01-09 05:35_

---

_Comment by @charliermarsh on 2024-01-09 06:05_

Okay, need to fix Windows -- draft for now.

---

_Comment by @charliermarsh on 2024-01-09 06:09_

I'm realizing that this likely won't work for paths that contain characters that can be confused for routing characters in matchit (like colons).

---

_Closed by @charliermarsh on 2024-01-09 06:09_

---
