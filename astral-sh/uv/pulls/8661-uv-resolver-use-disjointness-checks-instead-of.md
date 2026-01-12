```yaml
number: 8661
title: "uv-resolver: use disjointness checks instead of marker equality"
type: pull_request
state: merged
author: BurntSushi
labels:
  - enhancement
  - resolver
assignees: []
merged: true
base: main
head: ag/use-disjointness-checks
created_at: 2024-10-29T14:38:37Z
updated_at: 2024-10-29T15:25:21Z
url: https://github.com/astral-sh/uv/pull/8661
synced_at: 2026-01-12T16:08:25Z
```

# uv-resolver: use disjointness checks instead of marker equality

---

_@BurntSushi_

When this code was written, we didn't have "proper" disjointness checks,
and so simple equality was used instead. Arguably disjointness checks are
more correct, and this would also simplify some case analysis in an ongoing
refactor.


---

_Review requested from @konstin by @BurntSushi on 2024-10-29 14:38_

---

_Label `enhancement` added by @konstin on 2024-10-29 14:39_

---

_Label `resolver` added by @konstin on 2024-10-29 14:39_

---

_@BurntSushi reviewed on 2024-10-29 14:53_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/edit.rs`:2947 on 2024-10-29 14:53_

@konstin It looks like this is the crux of the change here: when adding `requests>=2.31 ; sys_platform == 'win32' and python_version > '3.11'`, `idna==3.6` and `urllib3==2.2.1` get dropped in favor of `idna==2.7` and `urllib3=1.23`. It seems like this change is a good one because it keeps what was there and doesn't suddenly change the resolution.

I think what is shown above is _correct_ (`requests 2.31` is compatible with `idna 2.7`).

---

_@konstin approved on 2024-10-29 14:55_

---

_Comment by @BurntSushi on 2024-10-29 15:25_

Running benchmarks locally doesn't seem to uncover anything:

```
$ uv run resolver --uv-project-path uv-main --uv-project-path uv-ag-use-disjointness-checks --benchmark resolve-warm ../requirements/airflow.in
Benchmark 1: uv-main (resolve-warm)
  Time (mean ± σ):     295.4 ms ±   6.8 ms    [User: 380.3 ms, System: 195.6 ms]
  Range (min … max):   287.2 ms … 306.0 ms    10 runs

Benchmark 2: uv-ag-use-disjointness-checks (resolve-warm)
  Time (mean ± σ):     297.5 ms ±   4.0 ms    [User: 385.2 ms, System: 193.2 ms]
  Range (min … max):   292.8 ms … 304.2 ms    10 runs

Summary
  uv-main (resolve-warm) ran
    1.01 ± 0.03 times faster than uv-ag-use-disjointness-checks (resolve-warm)
```

(Disjointness checks should be pretty fast generally speaking.)

---

_Merged by @BurntSushi on 2024-10-29 15:25_

---

_Closed by @BurntSushi on 2024-10-29 15:25_

---

_Branch deleted on 2024-10-29 15:25_

---
