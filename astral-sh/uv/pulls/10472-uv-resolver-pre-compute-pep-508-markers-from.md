```yaml
number: 10472
title: "uv-resolver: pre-compute PEP 508 markers from universal markers"
type: pull_request
state: merged
author: BurntSushi
labels:
  - performance
assignees: []
merged: true
base: main
head: ag/optimize-without-extras
created_at: 2025-01-10T15:48:00Z
updated_at: 2025-01-10T16:23:21Z
url: https://github.com/astral-sh/uv/pull/10472
synced_at: 2026-01-10T11:44:51Z
```

# uv-resolver: pre-compute PEP 508 markers from universal markers

---

_Pull request opened by @BurntSushi on 2025-01-10 15:48_

It turns out that we use `UniversalMarker::pep508` quite a bit. To the
point that it makes sense to pre-compute it when constructing a
`UniversalMarker`.

This still isn't necessarily the fastest thing we can do, but this
results in a major speed-up and `without_extras` no longer shows up for
me in a profile.

Motivating benchmarks. First, from #10430:

```
$ hyperfine 'rm -f uv.lock && uv lock' 'rm -f uv.lock && uv-ag-optimize-without-extras lock'
Benchmark 1: rm -f uv.lock && uv lock
  Time (mean ± σ):     408.3 ms ± 276.6 ms    [User: 333.6 ms, System: 111.1 ms]
  Range (min … max):   316.9 ms … 1195.3 ms    10 runs

  Warning: The first benchmarking run for this command was significantly slower than the rest (1.195 s). This could be caused by (filesystem) caches that were not filled until after the first run. You should consider using the '--warmup' option to fill those caches before the actual benchmark. Alternatively, use the '--prepare' option to clear the caches before each timing run.

Benchmark 2: rm -f uv.lock && uv-ag-optimize-without-extras lock
  Time (mean ± σ):     209.4 ms ±   2.2 ms    [User: 209.8 ms, System: 103.8 ms]
  Range (min … max):   206.1 ms … 213.4 ms    14 runs

Summary
  rm -f uv.lock && uv-ag-optimize-without-extras lock ran
    1.95 ± 1.32 times faster than rm -f uv.lock && uv lock
```

And now from #10438:

```
$ hyperfine 'uv pip compile requirements.in -c constraints.txt --universal --no-progress --python-version 3.8 --offline > /dev/null' 'uv-ag-optimize-without-extras pip compile requirements.in -c constraints.txt --universal --no-progress --python-version 3.8 --offline > /dev/null'
Benchmark 1: uv pip compile requirements.in -c constraints.txt --universal --no-progress --python-version 3.8 --offline > /dev/null
  Time (mean ± σ):     12.718 s ±  0.052 s    [User: 12.818 s, System: 0.140 s]
  Range (min … max):   12.650 s … 12.815 s    10 runs

Benchmark 2: uv-ag-optimize-without-extras pip compile requirements.in -c constraints.txt --universal --no-progress --python-version 3.8 --offline > /dev/null
  Time (mean ± σ):     419.5 ms ±   6.7 ms    [User: 434.7 ms, System: 100.6 ms]
  Range (min … max):   412.7 ms … 434.3 ms    10 runs

Summary
  uv-ag-optimize-without-extras pip compile requirements.in -c constraints.txt --universal --no-progress --python-version 3.8 --offline > /dev/null ran
   30.32 ± 0.50 times faster than uv pip compile requirements.in -c constraints.txt --universal --no-progress --python-version 3.8 --offline > /dev/null
```

Fixes #10430, Fixes #10438


---

_Review requested from @konstin by @BurntSushi on 2025-01-10 15:48_

---

_Comment by @charliermarsh on 2025-01-10 15:58_

Oh come on

---

_@charliermarsh approved on 2025-01-10 15:59_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/universal_marker.rs`:89 on 2025-01-10 15:59_

Should this be false?

---

_@charliermarsh reviewed on 2025-01-10 15:59_

---

_Label `performance` added by @charliermarsh on 2025-01-10 15:59_

---

_Merged by @BurntSushi on 2025-01-10 16:23_

---

_Closed by @BurntSushi on 2025-01-10 16:23_

---

_Branch deleted on 2025-01-10 16:23_

---
