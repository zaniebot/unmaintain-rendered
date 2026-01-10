```yaml
number: 800
title: Add a script to benchmark against other tools
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bench
created_at: 2024-01-05T20:06:43Z
updated_at: 2024-01-05T21:08:21Z
url: https://github.com/astral-sh/uv/pull/800
synced_at: 2026-01-10T15:44:44Z
```

# Add a script to benchmark against other tools

---

_Pull request opened by @charliermarsh on 2024-01-05 20:06_

## Summary

Enables us to benchmark Puffin against `pip-tools` on a variety of tasks. In subsequent PRs, I'll add support for Poetry and perhaps other tools too.

Example usage:

```
❯ python scripts/bench.py -f requirements.in
2024-01-05 15:05:39 INFO Benchmarks: resolve-cold, resolve-warm
2024-01-05 15:05:39 INFO Tools: pip-tools, puffin
2024-01-05 15:05:39 INFO Reading requirements from: /Users/crmarsh/workspace/puffin/requirements.in
2024-01-05 15:05:39 INFO ```
2024-01-05 15:05:39 INFO black
2024-01-05 15:05:39 INFO ```
Benchmark 1: pip-tools (resolve-cold)
  Time (mean ± σ):     758.4 ms ±  15.1 ms    [User: 317.8 ms, System: 68.4 ms]
  Range (min … max):   738.1 ms … 786.7 ms    10 runs

Benchmark 1: puffin (resolve-cold)
  Time (mean ± σ):     213.5 ms ±  25.6 ms    [User: 34.6 ms, System: 27.9 ms]
  Range (min … max):   184.6 ms … 270.6 ms    12 runs

Benchmark 1: pip-tools (resolve-warm)
  Time (mean ± σ):     384.3 ms ±   6.7 ms    [User: 259.2 ms, System: 47.0 ms]
  Range (min … max):   376.0 ms … 399.6 ms    10 runs

Benchmark 1: puffin (resolve-warm)
  Time (mean ± σ):       8.0 ms ±   0.4 ms    [User: 4.4 ms, System: 5.0 ms]
  Range (min … max):     7.4 ms …  10.8 ms    209 runs
```


---

_Marked ready for review by @charliermarsh on 2024-01-05 20:06_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-05 20:06_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-05 20:06_

---

_Review requested from @konstin by @charliermarsh on 2024-01-05 20:06_

---

_@zanieb reviewed on 2024-01-05 20:27_

---

_Review comment by @zanieb on `scripts/bench.py`:313 on 2024-01-05 20:27_

Is it worth context managing these temporary directories?

---

_@zanieb approved on 2024-01-05 20:28_

Looks nice! Just gave it a quick read.

---

_Merged by @charliermarsh on 2024-01-05 21:08_

---

_Closed by @charliermarsh on 2024-01-05 21:08_

---

_Branch deleted on 2024-01-05 21:08_

---
