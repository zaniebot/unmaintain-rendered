```yaml
number: 6444
title: Set default version to py38
type: pull_request
state: merged
author: rco-ableton
labels: []
assignees: []
merged: true
base: main
head: actually-set-default-target
created_at: 2023-08-09T10:34:54Z
updated_at: 2023-08-09T15:27:48Z
url: https://github.com/astral-sh/ruff/pull/6444
synced_at: 2026-01-12T15:55:21Z
```

# Set default version to py38

---

_@rco-ableton_

## Summary

In https://github.com/astral-sh/ruff/pull/6397, the documentation was updated stating that the default target-version is now "py38", but the actual default value wasn't updated and remained py310. This commit updates the default value to match what the documentation says.

## Test Plan

I ran `cargo test`, which passed for me, and `cargo insta review`, which claimed no snapshots needed to be reviewed.


---

_Comment by @github-actions[bot] on 2023-08-09 10:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      9.3±0.21ms     4.4 MB/sec    1.00      9.0±0.23ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1798.9±45.40µs     9.3 MB/sec    1.00  1796.2±75.87µs     9.3 MB/sec
formatter/numpy/globals.py                 1.00   213.1±11.42µs    13.8 MB/sec    1.01   214.8±13.63µs    13.7 MB/sec
formatter/pydantic/types.py                1.02      3.9±0.17ms     6.5 MB/sec    1.00      3.9±0.14ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     12.3±0.28ms     3.3 MB/sec    1.01     12.4±0.36ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.10ms     5.1 MB/sec    1.00      3.2±0.09ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   458.6±15.53µs     6.4 MB/sec    1.00   455.5±11.46µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.20ms     4.0 MB/sec    1.00      6.3±0.16ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.2±0.17ms     6.6 MB/sec    1.09      6.7±0.15ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1361.7±70.96µs    12.2 MB/sec    1.03  1404.6±41.19µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.3±8.64µs    17.0 MB/sec    1.01    175.9±8.73µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.07ms     9.2 MB/sec    1.08      3.0±0.08ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.4±0.09ms     3.9 MB/sec    1.02     10.6±0.11ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1962.1±12.04µs     8.5 MB/sec    1.01  1977.8±14.85µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    199.1±2.56µs    14.8 MB/sec    1.02    202.4±6.31µs    14.6 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.8 MB/sec    1.01      4.5±0.05ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.10ms     3.1 MB/sec    1.00     13.2±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.05ms     4.5 MB/sec    1.00      3.7±0.04ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    378.1±7.28µs     7.8 MB/sec    1.00   378.9±10.58µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.07ms     3.7 MB/sec    1.00      7.0±0.09ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.06ms     5.7 MB/sec    1.00      7.2±0.06ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1465.5±18.00µs    11.4 MB/sec    1.00  1465.5±14.61µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    146.5±2.04µs    20.1 MB/sec    1.00    146.8±2.03µs    20.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     8.0 MB/sec    1.00      3.2±0.03ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-09 11:41_

Thanks! Looks like we actually _did_ miss this, though the original diff here again only updated documentation. I pushed a commit to this branch to update the default on the Rust side.

---

_Review requested from @zanieb by @charliermarsh on 2023-08-09 11:41_

---

_Review request for @zanieb removed by @charliermarsh on 2023-08-09 11:41_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-09 11:44_

---

_Comment by @charliermarsh on 2023-08-09 11:52_

Looks like we do need to update a bunch of tests here (since they assumed 3.10).

---

_@charliermarsh reviewed on 2023-08-09 12:02_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_use_pathlib/mod.rs`:72 on 2023-08-09 12:02_

@zanieb - We should probably make this the default for `Settings::for_rule`, what do you think? (But we can hash out in separate PR, I'm just trying to get tests passing here.)

---

_@charliermarsh reviewed on 2023-08-09 12:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/defaults.rs`:27 on 2023-08-09 12:05_

This is the relevant change.

---

_Merged by @charliermarsh on 2023-08-09 12:08_

---

_Closed by @charliermarsh on 2023-08-09 12:08_

---

_@rco-ableton reviewed on 2023-08-09 12:12_

---

_Review comment by @rco-ableton on `crates/ruff/src/settings/defaults.rs`:27 on 2023-08-09 12:12_

Thanks, I'd missed this somehow 😅 

---

_Branch deleted on 2023-08-09 13:25_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_use_pathlib/mod.rs`:72 on 2023-08-09 15:04_

I agree that makes sense to me

---

_@zanieb reviewed on 2023-08-09 15:04_

---

_Comment by @zanieb on 2023-08-09 15:27_

😮‍💨 Thanks for fixing my mistake!

---
