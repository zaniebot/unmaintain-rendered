```yaml
number: 1809
title: Use async unzip for local source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/block
created_at: 2024-02-21T14:00:12Z
updated_at: 2024-02-21T14:11:38Z
url: https://github.com/astral-sh/uv/pull/1809
synced_at: 2026-01-10T15:33:24Z
```

# Use async unzip for local source distributions

---

_Pull request opened by @charliermarsh on 2024-02-21 14:00_

## Summary

We currently maintain separate untar methods for sync and async, but we only use the sync version when the user provides a local source distribution. (Otherwise, we untar as we download the distribution.) In my testing, this is actually slower anyway:

```
❯ python -m scripts.bench \
        --uv-path ./target/release/main \
        --uv-path ./target/release/uv \
        ./requirements.in --benchmark resolve-cold --min-runs 50
Benchmark 1: ./target/release/main (resolve-cold)
  Time (mean ± σ):     835.2 ms ± 107.4 ms    [User: 346.0 ms, System: 151.3 ms]
  Range (min … max):   639.2 ms … 1051.0 ms    50 runs

Benchmark 2: ./target/release/uv (resolve-cold)
  Time (mean ± σ):     750.7 ms ±  91.9 ms    [User: 345.7 ms, System: 149.4 ms]
  Range (min … max):   637.9 ms … 905.7 ms    50 runs

Summary
  './target/release/uv (resolve-cold)' ran
    1.11 ± 0.20 times faster than './target/release/main (resolve-cold)'
```


---

_Label `internal` added by @charliermarsh on 2024-02-21 14:00_

---

_@konstin approved on 2024-02-21 14:08_

:rocket:

---

_Merged by @charliermarsh on 2024-02-21 14:11_

---

_Closed by @charliermarsh on 2024-02-21 14:11_

---

_Branch deleted on 2024-02-21 14:11_

---
