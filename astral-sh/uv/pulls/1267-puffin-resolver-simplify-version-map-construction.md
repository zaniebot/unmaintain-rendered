```yaml
number: 1267
title: "puffin-resolver: simplify version map construction"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/version-map-simplify
created_at: 2024-02-08T20:27:36Z
updated_at: 2024-02-08T20:33:34Z
url: https://github.com/astral-sh/uv/pull/1267
synced_at: 2026-01-12T16:04:32Z
```

# puffin-resolver: simplify version map construction

---

_@BurntSushi_

In the process of making VersionMap construction lazy, I realized this
refactoring would be useful to me. It also simplifies a fair bit of case
analysis and does fewer BTreeMap lookups during construction. With that
said, this doesn't seem to matter for perf:

```
$ hyperfine -w10 --runs 50 \
    "puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" \
    "puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null"
Benchmark 1: puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     146.8 ms ±   4.1 ms    [User: 350.1 ms, System: 314.2 ms]
  Range (min … max):   140.7 ms … 158.0 ms    50 runs

Benchmark 2: puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     146.8 ms ±   4.5 ms    [User: 359.8 ms, System: 308.3 ms]
  Range (min … max):   138.2 ms … 160.1 ms    50 runs

Summary
  puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.00 ± 0.04 times faster than puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

But the simplification is still nice, and will decrease the delta
between what we have now and a lazy version map.


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-08 20:27_

---

_Review requested from @konstin by @BurntSushi on 2024-02-08 20:27_

---

_@konstin approved on 2024-02-08 20:33_

---

_Merged by @BurntSushi on 2024-02-08 20:33_

---

_Closed by @BurntSushi on 2024-02-08 20:33_

---

_Branch deleted on 2024-02-08 20:33_

---
