```yaml
number: 5214
title: Keep track of when files are last seen in the cache
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/shrink_cache_size
created_at: 2023-06-20T15:54:50Z
updated_at: 2023-06-23T13:40:37Z
url: https://github.com/astral-sh/ruff/pull/5214
synced_at: 2026-01-12T15:55:17Z
```

# Keep track of when files are last seen in the cache

---

_@Thomasdezeeuw_

## Summary

And remove cached files that we haven't seen for a certain period of time, currently 30 days.

For the last seen timestamp we actually use an `u64`, it's smaller on disk than `SystemTime` (which size is OS dependent) and fits in an `AtomicU64` which we can use to update it without locks.

## Test Plan

Added a new unit test, run by `cargo test`.


---

_Review requested from @MichaReiser by @Thomasdezeeuw on 2023-06-20 15:54_

---

_Review requested from @charliermarsh by @Thomasdezeeuw on 2023-06-20 15:54_

---

_Comment by @github-actions[bot] on 2023-06-20 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12      8.0±0.30ms     5.1 MB/sec    1.00      7.1±0.32ms     5.8 MB/sec
formatter/numpy/ctypeslib.py               1.24  1863.5±86.61µs     8.9 MB/sec    1.00  1507.3±78.92µs    11.0 MB/sec
formatter/numpy/globals.py                 1.45   229.8±16.11µs    12.8 MB/sec    1.00   158.1±13.38µs    18.7 MB/sec
formatter/pydantic/types.py                1.39      4.1±0.16ms     6.2 MB/sec    1.00      2.9±0.16ms     8.6 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.89ms     2.6 MB/sec    1.01     15.9±0.47ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.13ms     4.2 MB/sec    1.00      3.9±0.11ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   485.7±25.04µs     6.1 MB/sec    1.16   562.3±27.08µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.26ms     3.8 MB/sec    1.07      7.1±0.29ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.47ms     5.1 MB/sec    1.04      8.3±0.26ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1659.5±76.77µs    10.0 MB/sec    1.05  1736.3±60.91µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.04    206.3±8.71µs    14.3 MB/sec    1.00   199.0±11.17µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.6±0.10ms     7.1 MB/sec    1.00      3.4±0.16ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      8.0±0.11ms     5.1 MB/sec    1.00      7.7±0.03ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.13   1749.1±9.82µs     9.5 MB/sec    1.00   1550.8±8.72µs    10.7 MB/sec
formatter/numpy/globals.py                 1.30    203.9±2.65µs    14.5 MB/sec    1.00   156.9±17.76µs    18.8 MB/sec
formatter/pydantic/types.py                1.29      4.1±0.02ms     6.3 MB/sec    1.00      3.2±0.03ms     8.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.29ms     2.6 MB/sec    1.00     15.7±0.36ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.04      4.2±0.16ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    422.7±5.04µs     7.0 MB/sec    1.00    417.0±5.38µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.04ms     3.7 MB/sec    1.05      7.2±0.15ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.10ms     5.0 MB/sec    1.00      8.1±0.03ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1708.6±16.00µs     9.7 MB/sec    1.00  1679.3±11.70µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    182.1±2.59µs    16.2 MB/sec    1.00    177.8±3.83µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     6.9 MB/sec    1.00      3.7±0.05ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:69 on 2023-06-21 06:38_

Nit: Can we add a `// SAFETY` comment explaining why the truncation is okay in this case?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:110 on 2023-06-21 06:40_

Nit: Move the computation (and truncation) into a helper function to avoid repeating it (and scope the `allow` attribute) or create a `Cache::new_with_package(path, package)` method.

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:184 on 2023-06-21 06:42_

Nit: Wouldn't `Relaxed` be sufficient?

---

_@MichaReiser reviewed on 2023-06-21 06:44_

---

_@Thomasdezeeuw reviewed on 2023-06-21 08:19_

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:184 on 2023-06-21 08:19_

Maybe, but it can also be subtly wrong. So I'm more inclined to use the stronger orderings unless lessening it is proven correct (and faster).

---

_@MichaReiser reviewed on 2023-06-21 09:11_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:184 on 2023-06-21 09:11_

My understanding is that `Release` should be used together with `Aquire` (which we are not?) [source](https://marabos.nl/atomics/memory-ordering.html#release-and-acquire-ordering)

---

_Comment by @charliermarsh on 2023-06-22 20:02_

Looks reasonable to me! I'll let @MichaReiser approve

---

_@MichaReiser approved on 2023-06-23 06:16_

Looks good. Can you please add the test plan in the PR summary? Ideally, we would have a unit test for this as well but I can see how this may be difficult. 

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:184 on 2023-06-23 08:53_

You're right I've changed it to `SeqCst` to generate the correct assembly (`xchg`, see https://godbolt.org/z/WcKfxjjfq).

---

_@Thomasdezeeuw reviewed on 2023-06-23 09:13_

---

_Comment by @Thomasdezeeuw on 2023-06-23 09:14_

> Looks good. Can you please add the test plan in the PR summary? Ideally, we would have a unit test for this as well but I can see how this may be difficult.

Added a test: https://github.com/astral-sh/ruff/pull/5214/files#diff-80a9c2637c432502a7075c792cc60db92282dd786999a78bfa9bb6f025afab35R532-R579.

---

_Merged by @Thomasdezeeuw on 2023-06-23 13:40_

---

_Closed by @Thomasdezeeuw on 2023-06-23 13:40_

---

_Branch deleted on 2023-06-23 13:40_

---
