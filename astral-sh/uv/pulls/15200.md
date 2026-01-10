```yaml
number: 15200
title: Add incompatibility from proxy to base package
type: pull_request
state: merged
author: konstin
labels:
  - performance
  - error messages
assignees: []
merged: true
base: main
head: konsti/add-incompat-for-proxy-packages
created_at: 2025-08-10T11:26:38Z
updated_at: 2025-09-22T11:26:10Z
url: https://github.com/astral-sh/uv/pull/15200
synced_at: 2026-01-10T06:36:15Z
```

# Add incompatibility from proxy to base package

---

_Pull request opened by @konstin on 2025-08-10 11:26_

Add an incompatibility that lets pubgrub skip of marker packages when the base package already has an incompatible version to improve the error messages (https://github.com/astral-sh/uv/issues/15199).

The change is also a small perf improvement. Overall this should be able to improve performance in slow cases by avoiding trying proxy package versions that are impossible anyway, for a (ideally very small cost) for tracking the additional incompatibility and tracking the base package for each proxy package.

```
$ hhyperfine --warmup 2 "uv pip compile --universal scripts/requirements/airflow.in" "target/release/uv pip compile --universal scripts/requirements/airflow.in"
Benchmark 1: uv pip compile --universal scripts/requirements/airflow.in
  Time (mean ± σ):     145.5 ms ±   3.9 ms    [User: 154.7 ms, System: 140.7 ms]
  Range (min … max):   139.2 ms … 153.4 ms    20 runs
 
Benchmark 2: target/release/uv pip compile --universal scripts/requirements/airflow.in
  Time (mean ± σ):     128.7 ms ±   5.5 ms    [User: 141.9 ms, System: 137.3 ms]
  Range (min … max):   121.8 ms … 142.0 ms    23 runs
 
Summary
  target/release/uv pip compile --universal scripts/requirements/airflow.in ran
    1.13 ± 0.06 times faster than uv pip compile --universal scripts/requirements/airflow.in
```

This implementation is the basic version: When we see a proxy `foo{...}>=x,<y` we add a dependency edge `foo{...}>=x,<y` -> `foo>=x,<y`. There are several way to extend this, which likely help more with performance than with error messages.

One idea is that if we see `foo{...}>=x,<y` but we already made a selection for `foo==z` outside that range, we can insert a dependency `foo{...}!=z` -> `foo!=z`. This avoids trying any version of the proxy package except the version that matches our previous selection.

Another is that if we see a dependency `foo>=x,<y`, we also add `foo{...}>=x,y` -> `foo>=x,<y`. This allows backtracking beyond `foo` immediately if all version of `foo{...}>=x,<y` are incompatible, since `foo{...}>=x,<y` incompatible -> `foo>=x,<y` incompatible -> the package that depended of `foo>=x,<y` is incompatible.

The cost for each of these operations is tracking an additional incompatibility per virtual package. An alternative approach is to only add the incompatibility lazily, only when we've tried several version of the virtual package already. This needs to be weighed of with the better error messages that the incompatibility gives, we unfortunately have only few large reference examples.

Requires https://github.com/astral-sh/pubgrub/pull/45

Closes https://github.com/astral-sh/uv/issues/15199

---

_Label `error messages` added by @konstin on 2025-08-10 11:26_

---

_Review requested from @BurntSushi by @konstin on 2025-08-27 19:00_

---

_Review request for @BurntSushi removed by @konstin on 2025-08-27 19:00_

---

_Review requested from @zanieb by @konstin on 2025-08-27 19:00_

---

_Label `performance` added by @konstin on 2025-08-27 19:00_

---

_Review requested from @BurntSushi by @konstin on 2025-08-27 19:00_

---

_Comment by @konstin on 2025-08-27 19:01_

Tagging both zanie and andrew because in an older version I broke a conflict test by accidentally including dependency groups, we should ensure that there are no other gotchas with dependency groups and/or conflicts.

---

_Marked ready for review by @konstin on 2025-08-27 19:01_

---

_@zanieb approved on 2025-08-28 13:30_

Cool. I do not have a deep understanding of the implications here — it makes sense to have another reviewer.

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:13093 on 2025-08-28 16:47_

Oh nice!

---

_Review comment by @BurntSushi on `crates/uv/tests/it/lock.rs`:29861 on 2025-08-28 16:48_

Much better!

---

_@BurntSushi approved on 2025-08-28 16:48_

I think this makes sense to me! This is a nice improvement. :-)

---

_Comment by @konstin on 2025-09-22 11:26_

No changes in any tested package.

---

_Merged by @konstin on 2025-09-22 11:26_

---

_Closed by @konstin on 2025-09-22 11:26_

---

_Branch deleted on 2025-09-22 11:26_

---
