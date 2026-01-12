```yaml
number: 4905
title: Run shadowed-variable analyses in deferred handlers
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/move-bindings
created_at: 2023-06-06T18:34:54Z
updated_at: 2023-06-12T20:11:09Z
url: https://github.com/astral-sh/ruff/pull/4905
synced_at: 2026-01-12T03:43:29Z
```

# Run shadowed-variable analyses in deferred handlers

---

_Pull request opened by @charliermarsh on 2023-06-06 18:34_

## Summary

More to come.

This doesn't properly handle deletions, but I want to create the draft PR so that I don't lose the benchmarks.

On main:

```
Benchmark 1: ./target/release/ruff crates/ruff/resources/test/cpython --silent -e --no-cache --select F402,F811
  Time (mean ± σ):     208.5 ms ±   7.3 ms    [User: 1724.4 ms, System: 64.6 ms]
  Range (min … max):   199.2 ms … 241.5 ms    100 runs

Benchmark 1: ./target/release/ruff crates/ruff/resources/test/cpython --silent -e --no-cache --select F402,F811
  Time (mean ± σ):     213.2 ms ±   8.2 ms    [User: 1764.5 ms, System: 63.8 ms]
  Range (min … max):   202.5 ms … 243.4 ms    100 runs
```

On `charlie/move-bindings`:

```
Benchmark 1: ./target/release/ruff crates/ruff/resources/test/cpython --silent -e --no-cache --select F402,F811
  Time (mean ± σ):     208.3 ms ±   6.0 ms    [User: 1734.5 ms, System: 63.2 ms]
  Range (min … max):   200.6 ms … 240.2 ms    100 runs

Benchmark 1: ./target/release/ruff crates/ruff/resources/test/cpython --silent -e --no-cache --select F402,F811
  Time (mean ± σ):     212.8 ms ±   7.5 ms    [User: 1770.8 ms, System: 63.3 ms]
  Range (min … max):   203.9 ms … 248.9 ms    100 runs
```

`cargo benchmark` showed no significant changes.


---

_Comment by @charliermarsh on 2023-06-12 20:11_

Closing for now, but I'm going to reopen this once we handle deletions properly.

---

_Closed by @charliermarsh on 2023-06-12 20:11_

---
