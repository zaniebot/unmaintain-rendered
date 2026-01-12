```yaml
number: 8089
title: Add caching to formatter
type: pull_request
state: merged
author: MichaReiser
labels:
  - cli
  - formatter
assignees: []
merged: true
base: main
head: formatter-cache
created_at: 2023-10-20T05:45:51Z
updated_at: 2023-10-23T09:03:14Z
url: https://github.com/astral-sh/ruff/pull/8089
synced_at: 2026-01-12T02:32:41Z
```

# Add caching to formatter

---

_Pull request opened by @MichaReiser on 2023-10-20 05:45_

## Summary

Implement caching for `ruff format`

The formatter caching is relatively simple. All we need to track is the formatted files. This PR extends our `Cache` implementation to instead track the changes to the cache instead of the new "file cache" contents (similar to an event store). Meaning, the cache now stores a `changes` collection instead of `new_files` and a `Change` either sets the `formatted` flag or updates the `lint` results. 

## Test Plan

Manual testing:

* Checked a file with E701 (Multiple statements on same line): Diagnostic is shown
* Formatted the code
* Ran check: Diagnostic is gone (cache invalidation)

The speedup from the benchmarking shows that caching is working.

## Benchmarking 

**Hot formatter cache, no changes**

```
hyperfine  --warmup 10  \
                    "../ruff/target/release/ruff format . -s" \
                    "../ruff/ruff-main format ../airflow -s"
Benchmark 1: ../ruff/target/release/ruff format . -s
  Time (mean ± σ):     111.9 ms ±   3.6 ms    [User: 67.3 ms, System: 154.8 ms]
  Range (min … max):   104.1 ms … 121.2 ms    27 runs
 
Benchmark 2: ../ruff/ruff-main format ../airflow -s
  Time (mean ± σ):     312.7 ms ±   2.8 ms    [User: 1815.5 ms, System: 163.4 ms]
  Range (min … max):   308.8 ms … 316.7 ms    10 runs
 
Summary
  ../ruff/target/release/ruff format . -s ran
    2.80 ± 0.09 times faster than ../ruff/ruff-main format ../airflow -s
```

**Hot formatter and linter cache, no changes**

slightly slower for projects with many diagnostics

```
../ruff/target/release/ruff check . -s
hyperfine  --warmup 10  \
                    "../ruff/target/release/ruff format . -s" \
                    "../ruff/ruff-main format ../airflow -s"
Benchmark 1: ../ruff/target/release/ruff format . -s
  Time (mean ± σ):     118.4 ms ±   3.1 ms    [User: 76.7 ms, System: 160.9 ms]
  Range (min … max):   109.7 ms … 122.9 ms    24 runs
 
Benchmark 2: ../ruff/ruff-main format ../airflow -s
  Time (mean ± σ):     313.5 ms ±   5.5 ms    [User: 1819.3 ms, System: 163.6 ms]
  Range (min … max):   308.2 ms … 323.7 ms    10 runs
 
Summary
  ../ruff/target/release/ruff format . -s ran
    2.65 ± 0.08 times faster than ../ruff/ruff-main format ../airflow -s
```
**No cache**

``` 
hyperfine  --warmup 10  \
                    "../ruff/target/release/ruff format . -s --no-cache" \
                    "../ruff/ruff-main format ../airflow -s"
Benchmark 1: ../ruff/target/release/ruff format . -s --no-cache
  Time (mean ± σ):     321.4 ms ±   5.1 ms    [User: 1839.7 ms, System: 165.1 ms]
  Range (min … max):   317.7 ms … 332.9 ms    10 runs
 
Benchmark 2: ../ruff/ruff-main format ../airflow -s
  Time (mean ± σ):     320.9 ms ±  10.0 ms    [User: 1807.1 ms, System: 166.3 ms]
  Range (min … max):   310.9 ms … 343.8 ms    10 runs
 
Summary
  ../ruff/ruff-main format ../airflow -s ran
    1.00 ± 0.04 times faster than ../ruff/target/release/ruff format . -s --no-cache
```

**Cold cache** (slower)
```
hyperfine --prepare "rm -rf .ruff_cache" --warmup 10 \
                          "../ruff/target/release/ruff format . -s" \
                          "../ruff/ruff-main format ../airflow -s"
Benchmark 1: ../ruff/target/release/ruff format . -s
  Time (mean ± σ):     342.4 ms ±  13.1 ms    [User: 1842.5 ms, System: 234.0 ms]
  Range (min … max):   328.8 ms … 367.2 ms    10 runs
 
Benchmark 2: ../ruff/ruff-main format ../airflow -s
  Time (mean ± σ):     319.9 ms ±  16.0 ms    [User: 1816.3 ms, System: 166.7 ms]
  Range (min … max):   307.5 ms … 363.1 ms    10 runs
 
Summary
  ../ruff/ruff-main format ../airflow -s ran
    1.07 ± 0.07 times faster than ../ruff/target/release/ruff format . -s
```
  
**Cold cache with changes** (slower)
```
hyperfine --prepare "rm -rf .ruff_cache && git reset --hard -q" --warmup 10 \
                          "../ruff/target/release/ruff format . -s" \
                          "../ruff/ruff-main format ../airflow -s"
Benchmark 1: ../ruff/target/release/ruff format . -s
  Time (mean ± σ):     389.0 ms ±  12.0 ms    [User: 1763.3 ms, System: 354.7 ms]
  Range (min … max):   376.6 ms … 416.2 ms    10 runs
 
Benchmark 2: ../ruff/ruff-main format ../airflow -s
  Time (mean ± σ):     363.5 ms ±   4.7 ms    [User: 1732.4 ms, System: 297.8 ms]
  Range (min … max):   358.1 ms … 370.9 ms    10 runs
 
Summary
  ../ruff/ruff-main format ../airflow -s ran
    1.07 ± 0.04 times faster than ../ruff/target/release/ruff format . -s
```

**Hot cache with changes** (e.g. when switching between branches)
```
hyperfine --prepare "git reset --hard -q" --warmup 10 \
                          "../ruff/target/release/ruff format . -s" \
                          "../ruff/ruff-main format ../airflow -s"
Benchmark 1: ../ruff/target/release/ruff format . -s
  Time (mean ± σ):     220.1 ms ±   8.1 ms    [User: 653.7 ms, System: 323.0 ms]
  Range (min … max):   212.3 ms … 234.1 ms    10 runs
 
Benchmark 2: ../ruff/ruff-main format ../airflow -s
  Time (mean ± σ):     365.4 ms ±   6.2 ms    [User: 1722.6 ms, System: 294.4 ms]
  Range (min … max):   359.1 ms … 375.0 ms    10 runs
 
Summary
  ../ruff/target/release/ruff format . -s ran
    1.66 ± 0.07 times faster than ../ruff/ruff-main format ../airflow -s
```

**Linting with caching**

```
hyperfine --warmup 10 --setup "rm -rf ./.ruff_cache" \
                          "../ruff/target/release/ruff check -s -e ." \
                          "../ruff/ruff-main check -s -e ."
Benchmark 1: ../ruff/target/release/ruff check -s -e .
  Time (mean ± σ):      95.7 ms ±   1.4 ms    [User: 69.3 ms, System: 87.6 ms]
  Range (min … max):    92.1 ms … 101.0 ms    30 runs
 
Benchmark 2: ../ruff/ruff-main check -s -e .
  Time (mean ± σ):      95.8 ms ±   2.3 ms    [User: 68.9 ms, System: 87.5 ms]
  Range (min … max):    92.6 ms … 107.0 ms    30 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Summary
  ../ruff/target/release/ruff check -s -e . ran
    1.00 ± 0.03 times faster than ../ruff/ruff-main check -s -e .
```


---

_Comment by @MichaReiser on 2023-10-20 05:46_

Current dependencies on/for this PR:
* main
  * **PR #8089** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8089" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8089?utm_source=stack-comment).

---

_@MichaReiser reviewed on 2023-10-20 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:172 on 2023-10-20 06:45_

I don't think supporting different cache keys for a single cache instance is a good idea. It can lead to key collisions and is prone to mixing different keys (which has the unexpected result that a stored value can not be retrieved). 

E.g. I accidentally passed the hashed `u64` instead of the `FileCacheKey`. Rust was happy because `u64` implements `CacheKey`.

---

_Label `cli` added by @MichaReiser on 2023-10-20 07:31_

---

_Label `formatter` added by @MichaReiser on 2023-10-20 07:31_

---

_@charliermarsh reviewed on 2023-10-21 01:55_

This generally looks good to me. Is it ready for review / approval?

---

_Comment by @MichaReiser on 2023-10-21 11:05_

> This generally looks good to me. Is it ready for review / approval?

Not yet

---

_Marked ready for review by @MichaReiser on 2023-10-23 02:13_

---

_Comment by @github-actions[bot] on 2023-10-23 02:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/args.rs`:374 on 2023-10-23 03:02_

I think we would need to have different environment variables (`RUFF_CHECK_CACHE_DIR`, `RUFF_FORMAT_CACHE_DIR`) since one could potentially provide different path to the `check` / `format` subcommand.

Or, could make this a global flag?

---

_@dhruvmanila reviewed on 2023-10-23 03:22_

---

_@MichaReiser reviewed on 2023-10-23 03:28_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/args.rs`:374 on 2023-10-23 03:28_

I would wait until we have a specific request to store the caches in different locations. I also don't know how to support different cache locations when `check` lints and formats. 

The alternative would be to have two different caches (the implementation I had before this morning) where `format` and `lint` store their results in different files. Different jobs could then cache the specific sub-directories only. However, the overall design of the `Cache` API felt more fragile and would probably require a more drastic refactor.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:374 on 2023-10-23 03:43_

Not necessary IMO.

---

_@charliermarsh reviewed on 2023-10-23 03:43_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:172 on 2023-10-23 03:45_

Yeah this should be generic on the instance, right? (Looks like it was made non-generic which is also fine with me.)

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:104 on 2023-10-23 03:48_

Why this change, then cloning the path below?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:158 on 2023-10-23 03:48_

Assuming we keep the `par_iter`, perhaps `path.to_path_buf()`?

---

_@charliermarsh approved on 2023-10-23 03:54_

Looks great.

---

_@MichaReiser reviewed on 2023-10-23 08:42_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/commands/format.rs`:104 on 2023-10-23 08:42_

`package_roots` borrows `paths` until the end of the function. That means we can no longer use `into_iter`. I don't expect this to be meaningful, considering that we only hit the `clone` part when an error occurred. 

---

_Merged by @MichaReiser on 2023-10-23 08:43_

---

_Closed by @MichaReiser on 2023-10-23 08:43_

---

_Branch deleted on 2023-10-23 08:43_

---

_Comment by @konstin on 2023-10-23 08:50_

Did you profile what the time is spent on with a hot cache? 100ms sounds a lot to me for just checking modification timestamps.

---

_Comment by @MichaReiser on 2023-10-23 09:02_

> Did you profile what the time is spent on with a hot cache? 100ms sounds a lot to me for just checking modification timestamps.

I did not. I assume it's the python file discovery as well as detecting the python packages, loading of the cache etc. Linting without changes takes 96ms. So it's about the same. 

I agree, that we could probably improve our caching overall. 

---
