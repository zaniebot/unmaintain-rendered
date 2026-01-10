```yaml
number: 5887
title: less aggressive overlapping markers
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/overlapping-markers-conservative
created_at: 2024-08-07T18:51:46Z
updated_at: 2024-08-13T15:35:49Z
url: https://github.com/astral-sh/uv/pull/5887
synced_at: 2026-01-10T13:09:50Z
```

# less aggressive overlapping markers

---

_Pull request opened by @BurntSushi on 2024-08-07 18:51_

This is like #5733, but implements less aggressive forking. Namely, in
#5733, we would possibly fork any time we say a marker expression, even
if there was only one dependency specification for that package. In
this PR, we only consider forking when there are at least two sibling
dependency specifications for the same package name.

The end result here is that this fixes #4640 but not #4668. It also
doesn't fix some as-yet unreported bugs related to detecting forks in
conflicting transitive dependencies. However, this does have fewer
changes overall in resolutions when compared to the status quo, and
also has a much smaller performance regression in some ad hoc
benchmarks.

Our plan is to compare this PR and #5733 on real world projects to see
how the actual resolutions differ.

Fixes #4640, Closes #4732


---

_Comment by @BurntSushi on 2024-08-07 18:57_

On the transformers benchmark detailed in #5733:

```
$ hyperfine -w10 -p 'rm -rf uv.lock' 'uv-main lock' 'uv-overlapping-markers-aggressive lock' 'uv-overlapping-markers-conservative lock'
Benchmark 1: uv-main lock
  Time (mean ± σ):      49.4 ms ±   2.6 ms    [User: 47.3 ms, System: 52.2 ms]
  Range (min … max):    44.1 ms …  55.8 ms    63 runs

Benchmark 2: uv-overlapping-markers-aggressive lock
  Time (mean ± σ):     101.4 ms ±   3.9 ms    [User: 106.9 ms, System: 72.7 ms]
  Range (min … max):    94.8 ms … 109.0 ms    30 runs

Benchmark 3: uv-overlapping-markers-conservative lock
  Time (mean ± σ):      53.6 ms ±   3.2 ms    [User: 51.1 ms, System: 54.8 ms]
  Range (min … max):    47.0 ms …  61.2 ms    54 runs

Summary
  uv-main lock ran
    1.09 ± 0.09 times faster than uv-overlapping-markers-conservative lock
    2.05 ± 0.13 times faster than uv-overlapping-markers-aggressive lock
```

And on Home Assistant:

```
$ hyperfine -w10 -p 'rm -rf uv.lock' 'uv-main lock' 'uv-overlapping-markers-aggressive lock' 'uv-overlapping-markers-conservative lock'
Benchmark 1: uv-main lock
  Time (mean ± σ):     181.6 ms ±   2.7 ms    [User: 145.5 ms, System: 115.4 ms]
  Range (min … max):   176.4 ms … 186.7 ms    16 runs

Benchmark 2: uv-overlapping-markers-aggressive lock
  Time (mean ± σ):     223.4 ms ±   3.8 ms    [User: 184.9 ms, System: 121.8 ms]
  Range (min … max):   219.4 ms … 232.2 ms    13 runs

Benchmark 3: uv-overlapping-markers-conservative lock
  Time (mean ± σ):     180.8 ms ±   2.6 ms    [User: 143.3 ms, System: 115.9 ms]
  Range (min … max):   177.2 ms … 187.5 ms    16 runs

Summary
  uv-overlapping-markers-conservative lock ran
    1.00 ± 0.02 times faster than uv-main lock
    1.24 ± 0.03 times faster than uv-overlapping-markers-aggressive lock
```

Note that `uv-overlapping-markers-{conservative,aggressive}` are both derived from @ibraheemdev's BDD marker work (which isn't on `main` yet). Without the BDD work, `uv-overlapping-markers-aggressive` is slower (3-4x regression from `main` instead of 1-2x).

---

_Comment by @codspeed-hq[bot] on 2024-08-09 19:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ag/overlapping-markers-conservative)

### Merging #5887 will **degrade performances by 10.88%**

<sub>Comparing <code>ag/overlapping-markers-conservative</code> (ca708f2) with <code>main</code> (8dbf43c)</sub>



### Summary

`❌ 1` regressions
`✅ 13` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ag/overlapping-markers-conservative)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ag/overlapping-markers-conservative` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter_universal` | 282.1 ms | 316.5 ms | -10.88% |


---

_Comment by @BurntSushi on 2024-08-13 15:18_

This does regress the warm resolve for jupyter by about 10%, but I think that's acceptable for a change like this where we improve correctness at the known cost of possibly forking more. Of course, we should investigate further to improve perf and possibly even avoid forking if we can determine it won't be useful, but I think that should be reserved for future work.

---
