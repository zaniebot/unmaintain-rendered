```yaml
number: 5265
title: Intern package, extra, and group names
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
draft: true
base: main
head: charlie/ustr
created_at: 2024-07-21T22:15:56Z
updated_at: 2024-09-23T23:30:47Z
url: https://github.com/astral-sh/uv/pull/5265
synced_at: 2026-01-10T12:53:31Z
```

# Intern package, extra, and group names

---

_Pull request opened by @charliermarsh on 2024-07-21 22:15_

## Summary

This PR uses [ustr](https://github.com/anderslanglands/ustr) to intern package, extra, and group names.

In theory this should reduce allocations and make hashing and comparisons much cheaper.

Interestingly, though, I'm seeing about a 3-5% regression in local benchmarking:

```
❯ python -m scripts.bench \
    --uv-path ./target/release/uv \
    --uv-path ./target/release/main \
    ./scripts/requirements/jupyter.in --benchmark resolve-warm --min-runs 100
Benchmark 1: ./target/release/uv (resolve-warm)
  Time (mean ± σ):      22.2 ms ±   1.0 ms    [User: 17.5 ms, System: 46.1 ms]
  Range (min … max):    19.6 ms …  24.9 ms    114 runs

Benchmark 2: ./target/release/main (resolve-warm)
  Time (mean ± σ):      21.4 ms ±   0.8 ms    [User: 17.5 ms, System: 44.2 ms]
  Range (min … max):    19.3 ms …  23.6 ms    114 runs

Summary
  './target/release/main (resolve-warm)' ran
    1.04 ± 0.06 times faster than './target/release/uv (resolve-warm)'
```

Probably won't proceed much further, so just putting this up for posterity.


---

_Label `performance` added by @charliermarsh on 2024-07-21 22:16_

---

_Comment by @codspeed-hq[bot] on 2024-07-24 14:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/ustr)

### Merging #5265 will **degrade performances by 11.11%**

<sub>Comparing <code>charlie/ustr</code> (e2e3944) with <code>main</code> (cf971a8)</sub>



### Summary

`❌ 2` regressions
`✅ 11` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/ustr)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/ustr` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_parsing[flyte-short-compatible]` | 6.2 µs | 6.9 µs | -10.66% |
| ❌ | `wheelname_tag_compatibility[flyte-long-incompatible]` | 1.4 µs | 1.6 µs | -11.11% |


---

_Closed by @charliermarsh on 2024-09-23 23:30_

---
