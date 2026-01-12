```yaml
number: 10453
title: "Use `ArcStr` for marker values"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/arc
created_at: 2025-01-10T01:07:46Z
updated_at: 2025-01-10T20:15:15Z
url: https://github.com/astral-sh/uv/pull/10453
synced_at: 2026-01-12T16:09:18Z
```

# Use `ArcStr` for marker values

---

_@charliermarsh_

N.B. After fixing #10430, `ArcStr` became the fastest implementation (and the gains were significantly reduced, down to 1-2%). See: https://github.com/astral-sh/uv/pull/10453#issuecomment-2583344414.

## Summary

I tried out a variety of small string crates, but `Arc<str>` outperformed them, giving a ~10% speed-up:

```console
❯ hyperfine "../arcstr lock" "../flexstr lock" "uv lock" "../arc lock" "../compact_str lock" --prepare "rm -f uv.lock" --min-runs 50 --warmup 20
Benchmark 1: ../arcstr lock
  Time (mean ± σ):     304.6 ms ±   2.3 ms    [User: 302.9 ms, System: 117.8 ms]
  Range (min … max):   299.0 ms … 311.3 ms    50 runs

Benchmark 2: ../flexstr lock
  Time (mean ± σ):     319.2 ms ±   1.7 ms    [User: 317.7 ms, System: 118.2 ms]
  Range (min … max):   316.8 ms … 323.3 ms    50 runs

Benchmark 3: uv lock
  Time (mean ± σ):     330.6 ms ±   1.5 ms    [User: 328.1 ms, System: 139.3 ms]
  Range (min … max):   326.6 ms … 334.2 ms    50 runs

Benchmark 4: ../arc lock
  Time (mean ± σ):     303.0 ms ±   1.2 ms    [User: 301.6 ms, System: 118.4 ms]
  Range (min … max):   300.3 ms … 305.3 ms    50 runs

Benchmark 5: ../compact_str lock
  Time (mean ± σ):     320.4 ms ±   2.0 ms    [User: 318.7 ms, System: 120.8 ms]
  Range (min … max):   317.3 ms … 326.7 ms    50 runs

Summary
  ../arc lock ran
    1.01 ± 0.01 times faster than ../arcstr lock
    1.05 ± 0.01 times faster than ../flexstr lock
    1.06 ± 0.01 times faster than ../compact_str lock
    1.09 ± 0.01 times faster than uv lock
```


---

_Label `performance` added by @charliermarsh on 2025-01-10 01:07_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-10 01:07_

---

_Review requested from @konstin by @charliermarsh on 2025-01-10 01:07_

---

_Marked ready for review by @charliermarsh on 2025-01-10 01:07_

---

_@zanieb approved on 2025-01-10 04:41_

Nice and simple

---

_Comment by @konstin on 2025-01-10 10:08_

Which test case did you use?

---

_@konstin approved on 2025-01-10 10:08_

---

_@BurntSushi approved on 2025-01-10 12:42_

Very interesting. I'm a little surprised that `Arc<str>` wins here (although it seems virtually tied with `arcstr`), especially given that these strings tend to be extremely small.

---

_Comment by @charliermarsh on 2025-01-10 13:10_

I used the test case from #10430.

@BurntSushi — Which would you have expected to win here? Do you think I did anything wrong?

---

_Comment by @BurntSushi on 2025-01-10 14:08_

> @BurntSushi — Which would you have expected to win here? Do you think I did anything wrong?

No not all. Basically, _if_ most of our strings are very short, I'd expect this to trigger the "fast path" inside the small string data types where creation doesn't need a heap alloc and access doesn't need an indirection. Hazarding a guess, this might also be related to the size of a string itself. An `ArcStr` is only 8 bytes, an `Arc<str>` is 16 bytes but both `compact_str::CompactString` and `flexstr::SharedStr` are 24 bytes. So maybe perf here is more related CPU cache efficiency? Very hard to say without a deep dive here. But I approve of your methods here for an easy win.

Maybe there are lots of cases where the strings aren't small though, and this is triggering the slower path in the small string types.

---

_Comment by @charliermarsh on 2025-01-10 14:15_

I could also try re-benching it once #10430 is addressed? It's possible that the cloning here is dominant and that's somehow faster with `Arc`.

---

_Comment by @charliermarsh on 2025-01-10 17:46_

Ok, now that #10430 is addressed, I re-ran this. It looks like `arcstr` is the best-performing, but the gains are much smaller (1-2%).

For the case from #10430:

```
❯ hyperfine "../arcstr lock" "../flex_str lock" "../uv lock" "../arc lock" "../compact_str lock" --prepare "rm -f uv.lock" --min-runs 50 --warmup 20
Benchmark 1: ../arcstr lock
  Time (mean ± σ):     218.1 ms ±   2.6 ms    [User: 217.3 ms, System: 113.5 ms]
  Range (min … max):   214.8 ms … 230.6 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 2: ../flex_str lock
  Time (mean ± σ):     220.8 ms ±   1.9 ms    [User: 220.1 ms, System: 112.2 ms]
  Range (min … max):   218.8 ms … 231.6 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.

Benchmark 3: ../uv lock
  Time (mean ± σ):     221.0 ms ±   1.1 ms    [User: 220.3 ms, System: 112.4 ms]
  Range (min … max):   218.8 ms … 224.2 ms    50 runs

Benchmark 4: ../arc lock
  Time (mean ± σ):     218.7 ms ±   1.0 ms    [User: 218.1 ms, System: 112.2 ms]
  Range (min … max):   216.9 ms … 221.3 ms    50 runs

Benchmark 5: ../compact_str lock
  Time (mean ± σ):     219.3 ms ±   1.2 ms    [User: 219.3 ms, System: 112.2 ms]
  Range (min … max):   216.9 ms … 223.7 ms    50 runs

Summary
  ../arcstr lock ran
    1.00 ± 0.01 times faster than ../arc lock
    1.01 ± 0.01 times faster than ../compact_str lock
    1.01 ± 0.01 times faster than ../flex_str lock
    1.01 ± 0.01 times faster than ../uv lock
```

For `airflow.in`:

```
❯ hyperfine "../arcstr pip compile --universal ../scripts/requirements/airflow.in" "../flex_str pip compile --universal ../scripts/requirements/airflow.in" "../uv pip compile --universal ../scripts/requirements/airflow.in" "../arc pip compile --universal ../scripts/requirements/airflow.in" "../compact_str pip compile --universal ../scripts/requirements/airflow.in" --min-runs 50 --warmup 20
Benchmark 1: ../arcstr pip compile --universal ../scripts/requirements/airflow.in
  Time (mean ± σ):     132.2 ms ±   2.5 ms    [User: 136.7 ms, System: 226.6 ms]
  Range (min … max):   126.6 ms … 141.2 ms    50 runs

Benchmark 2: ../flex_str pip compile --universal ../scripts/requirements/airflow.in
  Time (mean ± σ):     134.3 ms ±   2.8 ms    [User: 138.4 ms, System: 240.2 ms]
  Range (min … max):   130.2 ms … 146.0 ms    50 runs

Benchmark 3: ../uv pip compile --universal ../scripts/requirements/airflow.in
  Time (mean ± σ):     134.7 ms ±   4.3 ms    [User: 138.9 ms, System: 240.3 ms]
  Range (min … max):   129.8 ms … 153.8 ms    50 runs

Benchmark 4: ../arc pip compile --universal ../scripts/requirements/airflow.in
  Time (mean ± σ):     133.8 ms ±   3.4 ms    [User: 138.3 ms, System: 233.6 ms]
  Range (min … max):   129.1 ms … 150.3 ms    50 runs

Benchmark 5: ../compact_str pip compile --universal ../scripts/requirements/airflow.in
  Time (mean ± σ):     133.8 ms ±   2.4 ms    [User: 139.0 ms, System: 231.1 ms]
  Range (min … max):   128.5 ms … 138.8 ms    50 runs

Summary
  ../arcstr pip compile --universal ../scripts/requirements/airflow.in ran
    1.01 ± 0.03 times faster than ../arc pip compile --universal ../scripts/requirements/airflow.in
    1.01 ± 0.03 times faster than ../compact_str pip compile --universal ../scripts/requirements/airflow.in
    1.02 ± 0.03 times faster than ../flex_str pip compile --universal ../scripts/requirements/airflow.in
    1.02 ± 0.04 times faster than ../uv pip compile --universal ../scripts/requirements/airflow.in
```

Any objections to using `arcstr`? We could also leave it as-is, though `arcstr` is both faster and uses less memory.


---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-10 17:46_

---

_Comment by @BurntSushi on 2025-01-10 18:27_

I'd hedge toward `arcstr` given its smaller size. But totally fine keeping status quo given the updated measurements.

---

_Comment by @charliermarsh on 2025-01-10 18:27_

Sounds good, I'll ship that.

---

_Renamed from "Use `Arc<str>` for marker values" to "Use `ArcStr` for marker values" by @charliermarsh on 2025-01-10 19:58_

---

_Merged by @charliermarsh on 2025-01-10 20:15_

---

_Closed by @charliermarsh on 2025-01-10 20:15_

---

_Branch deleted on 2025-01-10 20:15_

---
