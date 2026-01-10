```yaml
number: 3643
title: Add airflow benchmark
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
  - testing
  - benchmarks
assignees: []
merged: true
base: main
head: airflow-bench
created_at: 2024-05-17T16:36:38Z
updated_at: 2024-05-23T17:25:55Z
url: https://github.com/astral-sh/uv/pull/3643
synced_at: 2026-01-10T14:32:20Z
```

# Add airflow benchmark

---

_Pull request opened by @ibraheemdev on 2024-05-17 16:36_

## Summary

This seems to be one of the most consistent benchmark cases we have in terms of standard deviation:
```
➜  hyperfine "target/profiling/main pip compile scripts/requirements/airflow.in" --runs 200
Benchmark 1: target/profiling/main pip compile scripts/requirements/airflow.in
  Time (mean ± σ):     292.6 ms ±   6.6 ms    [User: 414.1 ms, System: 194.2 ms]
  Range (min … max):   282.7 ms … 320.1 ms    200 runs
```

For smaller benchmarks, scispacy and dtlssocket seem to be a bit more consistent than our current jupyter benchmark, but it hasn't given us any problems so I'll leave it for now.

---

_@konstin approved on 2024-05-17 16:40_

---

_@zanieb approved on 2024-05-17 17:50_

---

_Label `performance` added by @zanieb on 2024-05-17 17:50_

---

_Label `testing` added by @zanieb on 2024-05-17 17:50_

---

_Comment by @ibraheemdev on 2024-05-17 19:03_

Hm we might have to install source distributions in a separate CI step to get CodSpeed to work.

---

_Comment by @konstin on 2024-05-17 19:05_

Since this is a cached case benchmark, we can build all dependencies (`pip build` should do), upload them to github pages and use `--find-links`. I can't tell if the effort is justified here though.

---

_Comment by @konstin on 2024-05-21 12:21_

In https://github.com/astral-sh/uv/pull/3688 we had a massive speedup on airflow but nearly no effect on jupyter, so this would be helpful for this case.

---

_Label `benchmarks` added by @ibraheemdev on 2024-05-23 16:43_

---

_Comment by @codspeed-hq[bot] on 2024-05-23 16:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:airflow-bench)

### Merging #3643 will **not alter performance**

<sub>Comparing <code>ibraheemdev:airflow-bench</code> (bacd1a0) with <code>main</code> (8f0f4d2)</sub>



### Summary

`✅ 12` untouched benchmarks

`🆕 1` new benchmarks



### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheemdev:airflow-bench` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 🆕 | `resolve_warm_airflow` | N/A | 1.5 s | N/A |


---

_Merged by @charliermarsh on 2024-05-23 17:25_

---

_Closed by @charliermarsh on 2024-05-23 17:25_

---
