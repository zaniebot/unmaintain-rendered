```yaml
number: 1792
title: Stream zip archive when fetching non-range-request metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/async-meta
created_at: 2024-02-21T03:01:18Z
updated_at: 2024-02-21T03:12:22Z
url: https://github.com/astral-sh/uv/pull/1792
synced_at: 2026-01-12T16:04:44Z
```

# Stream zip archive when fetching non-range-request metadata

---

_@charliermarsh_

## Summary

If a registry doesn't support range requests, then today, we download the entire wheel to disk and then read the metadata from the downloaded archive. This PR instead modifies the registry client to stream the zipfile and stop as soon as it's seen the metadata, which should be more efficient.

Closes https://github.com/astral-sh/uv/issues/1596.

## Test Plan

Made this the _only_ path for downloading metadata; verified that the test suite passed.


---

_Label `enhancement` added by @charliermarsh on 2024-02-21 03:01_

---

_Comment by @charliermarsh on 2024-02-21 03:12_

Something like 10-20% faster:

```
puffin on  charlie/async-meta:main [$!⇡] is 📦 v0.1.6 via 🐍 v3.12.1 (.venv) via 🦀 v1.75.0
❯ python -m scripts.bench \
        --uv-path ./target/release/main \
        --uv-path ./target/release/uv \
        ./scripts/requirements/dtlssocket.in --benchmark resolve-cold --min-runs 50
Benchmark 1: ./target/release/main (resolve-cold)
  Time (mean ± σ):     896.5 ms ±  79.0 ms    [User: 404.9 ms, System: 202.6 ms]
  Range (min … max):   726.3 ms … 1075.0 ms    50 runs

Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):     802.7 ms ±  58.6 ms    [User: 420.7 ms, System: 179.8 ms]
  Range (min … max):   722.5 ms … 953.8 ms    50 runs

Summary
  './target/release/uv (resolve-cold)' ran
    1.12 ± 0.13 times faster than './target/release/main (resolve-cold)'
puffin on  charlie/async-meta:main [$!⇡] is 📦 v0.1.6 via 🐍 v3.12.1 (.venv) via 🦀 v1.75.0 took 1m34s
❯ python -m scripts.bench \
        --uv-path ./target/release/main \
        --uv-path ./target/release/uv \
        ./scripts/requirements/black.in --benchmark resolve-cold --min-runs 50
Benchmark 1: ./target/release/main (resolve-cold)
  Time (mean ± σ):     380.8 ms ±  83.5 ms    [User: 102.9 ms, System: 39.9 ms]
  Range (min … max):   281.8 ms … 550.7 ms    50 runs

Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):     325.3 ms ±  44.7 ms    [User: 113.8 ms, System: 26.9 ms]
  Range (min … max):   269.4 ms … 411.9 ms    50 runs

Summary
  './target/release/uv (resolve-cold)' ran
    1.17 ± 0.30 times faster than './target/release/main (resolve-cold)'
```

---

_Merged by @charliermarsh on 2024-02-21 03:12_

---

_Closed by @charliermarsh on 2024-02-21 03:12_

---

_Branch deleted on 2024-02-21 03:12_

---
