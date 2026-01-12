```yaml
number: 5901
title: Include file permissions in key for cached files
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: cache/permissions-2
created_at: 2023-07-19T20:48:45Z
updated_at: 2023-07-25T17:46:06Z
url: https://github.com/astral-sh/ruff/pull/5901
synced_at: 2026-01-12T15:55:19Z
```

# Include file permissions in key for cached files

---

_@zanieb_

Reimplements https://github.com/astral-sh/ruff/pull/3104
Closes https://github.com/astral-sh/ruff/issues/5726

Note that we will generate the hash for a cache key twice in normal operation. Once to check for the cached item and again to update the cache. We could optimize this by generating the hash once in `diagnostics::lint_file` and passing the `u64` into `get` and `update`. We'd probably want to wrap it in a `CacheKeyHash` enum for type safety.

## Test plan

Unit tests for Windows and Unix.

Manual test with case from issue

```
❯ touch fake.py
❯ chmod +x fake.py
❯ ./target/debug/ruff --select EXE fake.py
fake.py:1:1: EXE002 The file is executable but no shebang is present
Found 1 error.
❯ chmod -x fake.py
❯ ./target/debug/ruff --select EXE fake.py
```

---

_Comment by @github-actions[bot] on 2023-07-19 21:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.2±0.03ms     4.4 MB/sec    1.00      9.2±0.04ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1870.6±1.87µs     8.9 MB/sec    1.00   1871.0±3.01µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    208.4±0.91µs    14.2 MB/sec    1.01    211.4±0.24µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.07     13.6±0.09ms     3.0 MB/sec    1.00     12.8±0.08ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      3.4±0.03ms     4.9 MB/sec    1.00      3.2±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    431.1±1.89µs     6.8 MB/sec    1.00    423.4±1.25µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.1±0.02ms     4.2 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.13      7.5±0.02ms     5.4 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09   1549.4±2.59µs    10.7 MB/sec    1.00   1423.4±1.80µs    11.7 MB/sec
linter/default-rules/numpy/globals.py      1.07    165.8±2.16µs    17.8 MB/sec    1.00    155.6±0.69µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.10      3.3±0.01ms     7.7 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.9±0.05ms     4.5 MB/sec    1.00      8.8±0.12ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.01  1727.8±35.23µs     9.6 MB/sec    1.00   1709.1±9.45µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    179.8±1.81µs    16.4 MB/sec    1.04    186.4±7.05µs    15.8 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.04ms     6.7 MB/sec    1.00      3.8±0.03ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.05ms     3.3 MB/sec    1.00     12.2±0.12ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.01ms     5.3 MB/sec    1.00      3.2±0.02ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    332.1±2.82µs     8.9 MB/sec    1.00    331.3±3.26µs     8.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.03ms     4.6 MB/sec    1.00      5.5±0.03ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1360.6±7.48µs    12.2 MB/sec    1.00   1325.0±4.69µs    12.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    143.1±1.42µs    20.6 MB/sec    1.00    140.6±1.14µs    21.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.9±0.01ms     8.7 MB/sec    1.00      2.9±0.01ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-25 14:18_

Works on macOS but not Linux, investigating...

---

_Marked ready for review by @zanieb on 2023-07-25 15:46_

---

_Review requested from @MichaReiser by @zanieb on 2023-07-25 15:46_

---

_Review requested from @charliermarsh by @zanieb on 2023-07-25 15:47_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:538 on 2023-07-25 15:59_

Nit: You can use `assert_eq` here (and some other places where the tests use `assert(left == right)`)

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:596 on 2023-07-25 15:59_

Nit: `assert_eq`

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/diagnostics.rs`:142 on 2023-07-25 16:02_

Nit: We could move this code into a `FileCacheKey::from_path` function to improve readability (it also gives you the option two have entire different implementations for windows and UNIX to avoid the conditional code blocks)

---

_@MichaReiser approved on 2023-07-25 16:02_

Thank you so much for fixing this complicated and weird issue. Testing with real IO is so painful.

---

_@charliermarsh reviewed on 2023-07-25 16:42_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/cache.rs`:522 on 2023-07-25 16:42_

The answer might be no, but feel obligated to ask if there’s any way to design around this potential test flakiness.

---

_@zanieb reviewed on 2023-07-25 16:58_

---

_Review comment by @zanieb on `crates/ruff_cli/src/cache.rs`:522 on 2023-07-25 16:58_

Thanks for the additional nudge, testing https://github.com/astral-sh/ruff/pull/5901/commits/dc322440f1277c09b2b191918806e56f2b173a54

---

_Merged by @zanieb on 2023-07-25 17:06_

---

_Closed by @zanieb on 2023-07-25 17:06_

---

_Branch deleted on 2023-07-25 17:06_

---

_@MichaReiser reviewed on 2023-07-25 17:10_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:522 on 2023-07-25 17:10_

I don't think so. The problem here seems to be the precision at which the file system (and kernel) represent times

https://stackoverflow.com/questions/14392975/timestamp-accuracy-on-ext4-sub-millsecond

https://apenwarr.ca/log/20181113

---

_Review comment by @zanieb on `crates/ruff_cli/src/cache.rs`:522 on 2023-07-25 17:28_

@MichaReiser let me know if my patch causes problems on your superspeed machine

---

_@zanieb reviewed on 2023-07-25 17:28_

---

_@MichaReiser reviewed on 2023-07-25 17:46_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:522 on 2023-07-25 17:46_

ohhh nice! Works perfectly

---
