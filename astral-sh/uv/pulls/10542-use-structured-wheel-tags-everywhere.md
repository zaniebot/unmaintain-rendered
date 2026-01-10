```yaml
number: 10542
title: Use structured wheel tags everywhere
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - no-build
assignees: []
merged: true
base: main
head: charlie/all-tags
created_at: 2025-01-12T16:53:50Z
updated_at: 2025-01-14T01:39:40Z
url: https://github.com/astral-sh/uv/pull/10542
synced_at: 2026-01-10T11:44:57Z
```

# Use structured wheel tags everywhere

---

_Pull request opened by @charliermarsh on 2025-01-12 16:53_

## Summary

This PR extends the thinking in #10525 to platform tags, and then uses the structured tag enums everywhere, rather than passing around strings. I think this is a big improvement! It means we're no longer doing ad hoc tag parsing all over the place.


---

_Label `internal` added by @charliermarsh on 2025-01-12 16:55_

---

_Label `no-build` added by @charliermarsh on 2025-01-12 17:26_

---

_Comment by @codspeed-hq[bot] on 2025-01-12 17:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fall-tags)

### Merging #10542 will **degrade performances by 36.27%**

<sub>Comparing <code>charlie/all-tags</code> (1320c7b) with <code>main</code> (2ffa319)</sub>



### Summary

`⚡ 6` improvements  
`❌ 4` regressions  
`✅ 4` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fall-tags)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/all-tags` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` build_platform_tags[burntsushi-archlinux] `` | 957.3 µs | 367.2 µs | ×2.6 |
| ❌ | `` wheelname_parsing[flyte-long-compatible] `` | 10.1 µs | 15.8 µs | -36.27% |
| ❌ | `` wheelname_parsing[flyte-long-incompatible] `` | 13.5 µs | 17.6 µs | -23.38% |
| ❌ | `` wheelname_parsing[flyte-short-compatible] `` | 6.1 µs | 7.8 µs | -21.11% |
| ❌ | `` wheelname_parsing[flyte-short-incompatible] `` | 6.2 µs | 7.8 µs | -21% |
| ⚡ | `` wheelname_tag_compatibility[flyte-long-compatible] `` | 2.1 µs | 1.8 µs | +14.43% |
| ⚡ | `` wheelname_tag_compatibility[flyte-long-incompatible] `` | 1.5 µs | 1.2 µs | +21.99% |
| ⚡ | `` wheelname_tag_compatibility[flyte-short-compatible] `` | 2 µs | 1.6 µs | +23.25% |
| ⚡ | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 1,078.1 ns | 896.1 ns | +20.3% |
| ⚡ | `` resolve_warm_jupyter `` | 73.4 ms | 63.3 ms | +15.93% |


---

_Comment by @charliermarsh on 2025-01-12 17:41_

Hmm... I wonder if the `wheelname_parsing` regressions are real. The thing is, this doesn't allocate anymore, whereas the previous implementation allocated multiple times (strings and vectors). I'll have to benchmark it on my own machine.

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/tags.rs`:1081 on 2025-01-12 20:57_

No changes here which is really nice. Confirms that the tag generation and formatting remains unchanged.

---

_@charliermarsh reviewed on 2025-01-12 20:57_

---

_@charliermarsh reviewed on 2025-01-12 21:05_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/lib.rs`:1347 on 2025-01-12 21:05_

In https://github.com/astral-sh/uv/pull/10546, this is trimmed down to 264.

---

_Review comment by @konstin on `crates/uv-platform-tags/src/abi_tag.rs`:225 on 2025-01-13 10:45_

We can use split_once to avoid the manual indexing which can panic

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform_tag.rs`:33 on 2025-01-13 10:48_

depending on whether we care about preserving those verbatim, we can translate the manylinux tags to their modern representation through their associated glibc version (mapping at https://peps.python.org/pep-0600/#legacy-manylinux-tags)

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform_tag.rs`:161 on 2025-01-13 10:56_

If this is perf bottleneck, rather than memchr, i'd use our knowledge that the first number can only be a `2`, so the second underscore must be two bytes after the first, given that it is in bounds and that there is an underscore at this position (when byte indexed, to not panic)

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform_tag.rs`:253 on 2025-01-13 10:56_

Same as for manylinux here, except the major is 1

---

_Review comment by @konstin on `crates/uv-platform-tags/src/platform_tag.rs`:575 on 2025-01-13 11:05_

An option to trim down the error handling boilerplate a bit is moving the shared tag outside, similar to:

```rust
#[error("{kind}, platform tag: {tag}")]
struct ParsePlatformTagError {
      tag: String,
      kind: ParsePlatformTagErrorKind,
}

enum ParsePlatformTagErrorKind {
      #[error("Invalid format for {platform}")]
      InvalidFormat { platform: &'static str },
      // ...
}
```

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-13 15:40_

---

_Review requested from @konstin by @charliermarsh on 2025-01-13 15:40_

---

_@BurntSushi approved on 2025-01-13 16:12_

LGTM.

The regressions on wheelname parsing are interesting, but it looks like the higher level benchmarks point to an improvement?

---

_Comment by @charliermarsh on 2025-01-13 16:25_

Yeah. I need to run them locally. We are doing a bit more work, but we’re also not allocating anymore…

---

_@zanieb approved on 2025-01-13 18:05_

---

_@konstin approved on 2025-01-13 18:29_

---

_Comment by @charliermarsh on 2025-01-13 19:13_

I don't think the CodSpeed regressions are "real".

On main:

```
wheelname_parsing/flyte-short-incompatible
                        time:   [159.47 ns 160.13 ns 160.82 ns]
                        thrpt:  [201.63 MiB/s 202.49 MiB/s 203.33 MiB/s]
                 change:
                        time:   [-3.0123% -1.8530% -0.6083%] (p = 0.00 < 0.05)
                        thrpt:  [+0.6120% +1.8880% +3.1059%]
                        Change within noise threshold.
wheelname_parsing/flyte-short-compatible
                        time:   [159.66 ns 161.03 ns 162.68 ns]
                        thrpt:  [175.87 MiB/s 177.67 MiB/s 179.20 MiB/s]
                 change:
                        time:   [-2.2943% -1.2318% -0.1728%] (p = 0.02 < 0.05)
                        thrpt:  [+0.1731% +1.2472% +2.3481%]
                        Change within noise threshold.
wheelname_parsing/flyte-long-incompatible
                        time:   [357.67 ns 358.70 ns 359.72 ns]
                        thrpt:  [342.00 MiB/s 342.97 MiB/s 343.96 MiB/s]
                 change:
                        time:   [-2.3844% -2.0267% -1.6664%] (p = 0.00 < 0.05)
                        thrpt:  [+1.6946% +2.0686% +2.4427%]
```

On this branch:

```
wheelname_parsing/flyte-short-incompatible
                        time:   [119.45 ns 119.72 ns 120.01 ns]
                        thrpt:  [270.18 MiB/s 270.84 MiB/s 271.46 MiB/s]
                 change:
                        time:   [-1.2435% -0.9227% -0.5367%] (p = 0.00 < 0.05)
                        thrpt:  [+0.5396% +0.9313% +1.2592%]
                        Change within noise threshold.
wheelname_parsing/flyte-short-compatible
                        time:   [119.13 ns 119.40 ns 119.70 ns]
                        thrpt:  [239.01 MiB/s 239.61 MiB/s 240.17 MiB/s]
                 change:
                        time:   [-1.9422% -1.7395% -1.5472%] (p = 0.00 < 0.05)
                        thrpt:  [+1.5715% +1.7703% +1.9807%]
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  4 (4.00%) low severe
  4 (4.00%) low mild
  2 (2.00%) high mild
  2 (2.00%) high severe
wheelname_parsing/flyte-long-incompatible
                        time:   [389.86 ns 390.80 ns 392.03 ns]
                        thrpt:  [313.82 MiB/s 314.80 MiB/s 315.56 MiB/s]
                 change:
                        time:   [-0.4042% +0.1993% +0.8119%] (p = 0.52 > 0.05)
                        thrpt:  [-0.8054% -0.1989% +0.4059%]
                        No change in performance detected.
wheelname_parsing/flyte-long-compatible
                        time:   [282.53 ns 283.03 ns 283.65 ns]
                        thrpt:  [383.28 MiB/s 384.12 MiB/s 384.81 MiB/s]
                 change:
                        time:   [+0.9606% +1.1332% +1.3430%] (p = 0.00 < 0.05)
                        thrpt:  [-1.3252% -1.1205% -0.9514%]
```

After the #10546 PR:

```
wheelname_parsing/flyte-short-incompatible
                        time:   [111.87 ns 112.46 ns 112.95 ns]
                        thrpt:  [287.07 MiB/s 288.31 MiB/s 289.85 MiB/s]
                 change:
                        time:   [-0.1758% +0.5279% +1.2135%] (p = 0.12 > 0.05)
                        thrpt:  [-1.1989% -0.5251% +0.1761%]
                        No change in performance detected.
Found 23 outliers among 100 measurements (23.00%)
  17 (17.00%) low severe
  2 (2.00%) high mild
  4 (4.00%) high severe
wheelname_parsing/flyte-short-compatible
                        time:   [110.01 ns 110.39 ns 110.77 ns]
                        thrpt:  [258.28 MiB/s 259.18 MiB/s 260.07 MiB/s]
                 change:
                        time:   [-0.8660% -0.1473% +0.5616%] (p = 0.69 > 0.05)
                        thrpt:  [-0.5585% +0.1475% +0.8736%]
                        No change in performance detected.
Found 24 outliers among 100 measurements (24.00%)
  16 (16.00%) low severe
  2 (2.00%) low mild
  3 (3.00%) high mild
  3 (3.00%) high severe
wheelname_parsing/flyte-long-incompatible
                        time:   [358.74 ns 359.57 ns 360.53 ns]
                        thrpt:  [341.23 MiB/s 342.14 MiB/s 342.94 MiB/s]
                 change:
                        time:   [-0.9707% -0.5934% -0.2323%] (p = 0.00 < 0.05)
                        thrpt:  [+0.2328% +0.5969% +0.9802%]
                        Change within noise threshold.
Found 6 outliers among 100 measurements (6.00%)
  6 (6.00%) high mild
wheelname_parsing/flyte-long-compatible
                        time:   [258.74 ns 258.92 ns 259.10 ns]
                        thrpt:  [419.59 MiB/s 419.90 MiB/s 420.18 MiB/s]
                 change:
                        time:   [-0.5224% -0.3401% -0.1560%] (p = 0.00 < 0.05)
                        thrpt:  [+0.1563% +0.3413% +0.5252%]
                        Change within noise threshold.
```

So the short tag case is about 30% _faster_, and the long tag case is either unchanged or 15% faster.

---

_Comment by @charliermarsh on 2025-01-13 20:13_

(My guess is that we're doing more CPU work than before (due to the parsing) but no longer allocating.)

---

_Comment by @konstin on 2025-01-13 20:31_

Checking with hyperfine, there is a speedup
```
$ hyperfine --warmup 2 --runs 100 --export-json tags.json "./uv-main pip compile scripts/requirements/airflow.in" "./uv-tags pip compile scripts/requirements/airflow.in"
Benchmark 1: ./uv-main pip compile scripts/requirements/airflow.in
  Time (mean ± σ):     119.6 ms ±   5.1 ms    [User: 134.8 ms, System: 111.6 ms]
  Range (min … max):   111.2 ms … 141.2 ms    100 runs
 
Benchmark 2: ./uv-tags pip compile scripts/requirements/airflow.in
  Time (mean ± σ):     114.3 ms ±   3.9 ms    [User: 129.6 ms, System: 108.6 ms]
  Range (min … max):   106.7 ms … 127.9 ms    100 runs
 
Summary
  ./uv-tags pip compile scripts/requirements/airflow.in ran
    1.05 ± 0.06 times faster than ./uv-main pip compile scripts/requirements/airflow.in
```
and the speedup is significant (welch t-test, t = 8.12, p = 6.55e-14).

---

_@charliermarsh reviewed on 2025-01-14 00:59_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:33 on 2025-01-14 00:59_

I _think_ it's important to preserve them verbatim.

---

_@charliermarsh reviewed on 2025-01-14 01:02_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:161 on 2025-01-14 01:02_

Is this true, though? Like, in theory, we could start returning numbers that aren't `2` from the `get_interpreter_info.py`.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-14 01:37_

---

_Merged by @charliermarsh on 2025-01-14 01:39_

---

_Closed by @charliermarsh on 2025-01-14 01:39_

---

_Branch deleted on 2025-01-14 01:39_

---
