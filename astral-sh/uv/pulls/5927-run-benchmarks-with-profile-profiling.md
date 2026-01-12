```yaml
number: 5927
title: "Run benchmarks with `--profile profiling`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
assignees: []
merged: true
base: main
head: ibraheem/bench-profiling
created_at: 2024-08-08T19:59:26Z
updated_at: 2024-09-10T18:25:55Z
url: https://github.com/astral-sh/uv/pull/5927
synced_at: 2026-01-12T16:07:06Z
```

# Run benchmarks with `--profile profiling`

---

_@ibraheemdev_

## Summary

The CodSpeed flamegraphs are currently useless after https://github.com/astral-sh/uv/pull/5745.

---

_Label `internal` added by @ibraheemdev on 2024-08-08 19:59_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-08-08 19:59_

---

_@charliermarsh approved on 2024-08-08 20:00_

---

_Comment by @ibraheemdev on 2024-08-08 20:07_

Looks like `cargo-codspeed` doesn't support this flag.

---

_Converted to draft by @ibraheemdev on 2024-08-08 20:07_

---

_Comment by @codspeed-hq[bot] on 2024-09-10 18:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheem/bench-profiling)

### Merging #5927 will **degrade performances by 13.79%**

<sub>Comparing <code>ibraheem/bench-profiling</code> (317306f) with <code>main</code> (cfa9299)</sub>



### Summary

`❌ 4` regressions
`✅ 10` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ibraheem/bench-profiling)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheem/bench-profiling` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `build_platform_tags[burntsushi-archlinux]` | 1.1 ms | 1.3 ms | -11.97% |
| ❌ | `wheelname_parsing[flyte-long-compatible]` | 8.9 µs | 10 µs | -10.95% |
| ❌ | `wheelname_parsing[flyte-short-compatible]` | 5.5 µs | 6.4 µs | -13.79% |
| ❌ | `wheelname_parsing[flyte-short-incompatible]` | 5.5 µs | 6.4 µs | -13.59% |


---

_Marked ready for review by @ibraheemdev on 2024-09-10 18:17_

---

_Comment by @ibraheemdev on 2024-09-10 18:18_

The regressions are because the `profiling` profile does not enable LTO, which seems acceptable unless we want to add another profile?

---

_@BurntSushi approved on 2024-09-10 18:21_

I think it makes sense to merge with the regressions since we are changing how we measure perf with this PR.

---

_Merged by @ibraheemdev on 2024-09-10 18:25_

---

_Closed by @ibraheemdev on 2024-09-10 18:25_

---

_Branch deleted on 2024-09-10 18:25_

---
