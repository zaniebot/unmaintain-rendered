```yaml
number: 5237
title: Initialize caches for packages and standalone files
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cache-roots
created_at: 2023-06-21T02:34:08Z
updated_at: 2023-06-21T17:52:01Z
url: https://github.com/astral-sh/ruff/pull/5237
synced_at: 2026-01-12T03:43:30Z
```

# Initialize caches for packages and standalone files

---

_Pull request opened by @charliermarsh on 2023-06-21 02:34_

## Summary

While fixing https://github.com/astral-sh/ruff/pull/5233, I noticed that in FastAPI, 343 out of 823 files weren't hitting the cache. It turns out these are standalone files in the documentation that lack a "package root". Later, when looking up the cache entries, we fallback to the package directory.

This PR ensures that we initialize the cache for both kinds of files: those that are in a package, and those that aren't.

The total size of the FastAPI cache for me is now 388K. I also suspect that this approach is much faster than as initially written, since before, we were probably initializing one cache per _directory_.

## Test Plan

Ran `cargo run -p ruff_cli -- check ../fastapi --verbose`; verified that, on second execution, there were no "Checking" entries in the logs.


---

_Review requested from @Thomasdezeeuw by @charliermarsh on 2023-06-21 02:34_

---

_@charliermarsh reviewed on 2023-06-21 02:34_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:97 on 2023-06-21 02:34_

I didn't spend any time optimizing this, feel free!

---

_@charliermarsh reviewed on 2023-06-21 02:35_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:97 on 2023-06-21 02:35_

(Not sure if worth calling `.unique()` above, if it's worth the `.collect()` allocation in order to allow the `.par_iter()`, if it's worth using the `.par_iter()` at all given far fewer entries, etc.)

---

_Comment by @github-actions[bot] on 2023-06-21 02:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.01ms     6.3 MB/sec    1.00      6.5±0.02ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.02   1365.7±3.12µs    12.2 MB/sec    1.00   1339.1±3.53µs    12.4 MB/sec
formatter/numpy/globals.py                 1.01    131.1±0.18µs    22.5 MB/sec    1.00    129.5±0.20µs    22.8 MB/sec
formatter/pydantic/types.py                1.01      2.7±0.03ms     9.3 MB/sec    1.00      2.7±0.01ms     9.4 MB/sec
linter/all-rules/large/dataset.py          1.02     13.3±0.04ms     3.1 MB/sec    1.00     13.0±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.4±0.00ms     4.9 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    430.8±0.76µs     6.8 MB/sec    1.00    427.5±1.32µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.01ms     6.1 MB/sec    1.00      6.6±0.02ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1462.9±4.42µs    11.4 MB/sec    1.00   1450.2±2.14µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.2±0.29µs    18.0 MB/sec    1.00    163.4±0.20µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.0±0.26ms     4.1 MB/sec    1.05     10.5±0.48ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.08ms     8.2 MB/sec    1.01      2.0±0.09ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00   202.2±12.71µs    14.6 MB/sec    1.06   213.8±16.36µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.13ms     6.2 MB/sec    1.03      4.2±0.17ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.01     19.8±0.50ms     2.1 MB/sec    1.00     19.7±0.43ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.20ms     3.2 MB/sec    1.00      5.2±0.19ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.02   656.7±36.96µs     4.5 MB/sec    1.00   645.8±28.63µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.38ms     2.8 MB/sec    1.02      9.1±0.47ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.28ms     4.0 MB/sec    1.04     10.6±0.49ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.15ms     7.5 MB/sec    1.00      2.2±0.08ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   259.9±14.22µs    11.4 MB/sec    1.00   260.7±12.38µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.16ms     5.5 MB/sec    1.04      4.8±0.21ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-21 03:13_

---

_Comment by @charliermarsh on 2023-06-21 03:13_

388K is great for the cache size BTW -- that's > 10x smaller than when we started, I think?

---

_@MichaReiser approved on 2023-06-21 06:55_

I don't have much context on how packages work. I'll leave it to @Thomasdezeeuw review. 

I would be in favor of removing the `par_iter` if it doesn't impact performance much.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/commands/run.rs`:117 on 2023-06-21 08:14_

(maybe for another) I understand the need to remove the `unwrap` as it was causing panics, but it does mean something went wrong before this state (as the `caches` should be filled if we reach this point). Maybe we should emit a warning or something?

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/commands/run.rs`:89 on 2023-06-21 08:15_

Why are we collecting only to iterate over it again?

---

_@Thomasdezeeuw reviewed on 2023-06-21 08:17_

---

_@charliermarsh reviewed on 2023-06-21 14:24_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/run.rs`:89 on 2023-06-21 14:24_

None of those structs implement `.par_iter()`.

---

_Merged by @charliermarsh on 2023-06-21 17:29_

---

_Closed by @charliermarsh on 2023-06-21 17:29_

---

_Branch deleted on 2023-06-21 17:29_

---
