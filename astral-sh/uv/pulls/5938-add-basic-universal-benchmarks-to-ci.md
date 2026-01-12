```yaml
number: 5938
title: Add basic universal benchmarks to CI
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - benchmarks
assignees: []
merged: true
base: main
head: ibraheem/universal-benchmarks
created_at: 2024-08-08T21:32:26Z
updated_at: 2024-08-09T16:52:30Z
url: https://github.com/astral-sh/uv/pull/5938
synced_at: 2026-01-12T16:07:07Z
```

# Add basic universal benchmarks to CI

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/uv/issues/4921.

---

_Label `internal` added by @ibraheemdev on 2024-08-08 21:32_

---

_Label `benchmarks` added by @ibraheemdev on 2024-08-08 21:32_

---

_Review requested from @konstin by @ibraheemdev on 2024-08-08 21:32_

---

_Review comment by @charliermarsh on `crates/bench/benches/uv.rs`:145 on 2024-08-09 01:10_

I think this means the benchmark will vary by interpreter rather than installing for a specific, consistent platform. Is that intentional?

---

_@charliermarsh reviewed on 2024-08-09 01:10_

---

_Comment by @konstin on 2024-08-09 08:02_

I'd love to have these!

---

_@ibraheemdev reviewed on 2024-08-09 09:26_

---

_Review comment by @ibraheemdev on `crates/bench/benches/uv.rs`:145 on 2024-08-09 09:26_

Right right, reverted that change.

---

_Comment by @codspeed-hq[bot] on 2024-08-09 10:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheem/universal-benchmarks)

### Merging #5938 will **degrade performances by 50.06%**

<sub>Comparing <code>ibraheem/universal-benchmarks</code> (5fe4d6f) with <code>main</code> (4330f97)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks

`🆕 1` new benchmarks


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ibraheem/universal-benchmarks)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheem/universal-benchmarks` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_airflow` | 1 s | 2.1 s | -50.06% |
| 🆕 | `resolve_warm_jupyter_universal` | N/A | 272.4 ms | N/A |


---

_Comment by @ibraheemdev on 2024-08-09 10:20_

Hmmm the new benchmarks add 7 minutes to the CI job. Maybe we need to test a simpler universal resolution than airflow.

---

_Comment by @zanieb on 2024-08-09 13:47_

It _might_ be okay if the benches run longer, though not ideal, they don't block pull requests.

Trying a simpler case makes sense too though.

---

_Comment by @konstin on 2024-08-09 14:00_

7s is out of proportion (`taskset -c 0` is a single efficiency core):

platform-specific:
```
$ hyperfine "taskset -c 0 uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 -p 3.11" "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 -p 3.11"

Benchmark 1: taskset -c 0 uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 -p 3.11
  Time (mean ± σ):     289.3 ms ±   4.6 ms    [User: 188.1 ms, System: 99.2 ms]
  Range (min … max):   282.5 ms … 295.4 ms    10 runs
 
Benchmark 2: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 -p 3.11
  Time (mean ± σ):     175.3 ms ±   7.2 ms    [User: 213.8 ms, System: 147.5 ms]
  Range (min … max):   167.0 ms … 186.8 ms    17 runs
```

universal:
```
$ hyperfine \
    "taskset -c 0 uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal" \
    "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal" \
    "taskset -c 0 uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal -p 3.11" \
    "uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal -p 3.11"

Benchmark 1: taskset -c 0 uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal
  Time (mean ± σ):     227.6 ms ±   5.5 ms    [User: 135.1 ms, System: 90.4 ms]
  Range (min … max):   220.4 ms … 239.7 ms    12 runs
 
Benchmark 2: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal
  Time (mean ± σ):     143.6 ms ±   6.5 ms    [User: 165.9 ms, System: 131.4 ms]
  Range (min … max):   130.5 ms … 157.6 ms    22 runs
 
Benchmark 3: taskset -c 0 uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal -p 3.11
  Time (mean ± σ):     557.2 ms ±   6.5 ms    [User: 412.6 ms, System: 141.9 ms]
  Range (min … max):   547.5 ms … 566.0 ms    10 runs
 
Benchmark 4: uv pip compile scripts/requirements/airflow.in --exclude-newer 2024-06-20 --universal -p 3.11
  Time (mean ± σ):     386.5 ms ±   4.7 ms    [User: 446.4 ms, System: 189.2 ms]
  Range (min … max):   379.0 ms … 392.5 ms    10 runs
```

3.11 also has very big markers, the other PR may change this a lot. I've also seen that we're doing expensive thing on the first run, such as building pyspark.

---

_Comment by @zanieb on 2024-08-09 14:43_

Giving a larger runner a quick try https://github.com/astral-sh/uv/pull/5959

---

_Merged by @ibraheemdev on 2024-08-09 16:52_

---

_Closed by @ibraheemdev on 2024-08-09 16:52_

---

_Branch deleted on 2024-08-09 16:52_

---
