```yaml
number: 4587
title: "Migrate some rules from `Fix::unspecified`"
type: pull_request
state: merged
author: sladyn98
labels: []
assignees: []
merged: true
base: main
head: fix-unspecified
created_at: 2023-05-22T21:19:46Z
updated_at: 2023-05-24T04:49:07Z
url: https://github.com/astral-sh/ruff/pull/4587
synced_at: 2026-01-12T03:50:03Z
```

# Migrate some rules from `Fix::unspecified`

---

_Pull request opened by @sladyn98 on 2023-05-22 21:19_

This PR aims to migrate `unspecified` code to `safe` or `maybe_correct`

Refers: #4184

---

_@sladyn98 reviewed on 2023-05-22 21:20_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_bugbear/rules/duplicate_exceptions.rs`:97 on 2023-05-22 21:20_

Is there a specific rule to follow when changing to `safe` or `maybe_correct`, right now I just went off the issue description where it might introduce semantic changes

---

_Converted to draft by @sladyn98 on 2023-05-22 21:21_

---

_@zanieb reviewed on 2023-05-22 22:29_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_bugbear/rules/duplicate_exceptions.rs`:97 on 2023-05-22 22:29_

fyi the names have changed per https://github.com/charliermarsh/ruff/issues/4184#issuecomment-1546782684

The meaning for each applicability level is documented in the `Applicability` enum

---

_Comment by @zanieb on 2023-05-22 22:33_

Could you remove the `Closes #4184`? The issue covers many more cases where the applicability needs to be specified and shouldn't be closed yet. Generally we're expecting a pull request per modified rule so that there can be proper discussion about the correct applicability level.

---

_Review requested from @zanieb by @sladyn98 on 2023-05-22 23:15_

---

_Comment by @github-actions[bot] on 2023-05-22 23:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.22ms     2.5 MB/sec    1.01     16.5±0.19ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.05ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.7±9.97µs     6.1 MB/sec    1.00    485.7±9.79µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.9±0.08ms     3.7 MB/sec    1.00      6.7±0.16ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.03      7.7±0.26ms     5.3 MB/sec    1.00      7.5±0.15ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1624.3±49.78µs    10.3 MB/sec    1.02  1660.7±32.13µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.03    190.1±2.52µs    15.5 MB/sec    1.00    184.2±3.89µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.10ms     7.4 MB/sec    1.00      3.4±0.09ms     7.5 MB/sec
parser/large/dataset.py                    1.00      6.0±0.23ms     6.8 MB/sec    1.04      6.2±0.06ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1165.0±32.34µs    14.3 MB/sec    1.05   1228.8±6.34µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    124.2±2.38µs    23.8 MB/sec    1.01    125.4±1.14µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.06ms     9.7 MB/sec    1.02      2.7±0.02ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.24ms     2.4 MB/sec    1.00     16.6±0.27ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.11ms     3.9 MB/sec    1.01      4.3±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    487.2±7.08µs     6.1 MB/sec    1.00    489.0±6.80µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.14ms     3.6 MB/sec    1.00      7.0±0.16ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.26ms     5.0 MB/sec    1.00      8.1±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1696.3±26.08µs     9.8 MB/sec    1.00  1696.7±22.16µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    187.6±3.24µs    15.7 MB/sec    1.02    190.5±4.81µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.0 MB/sec    1.01      3.7±0.06ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.5±0.06ms     6.2 MB/sec    1.00      6.5±0.06ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1240.4±15.38µs    13.4 MB/sec    1.00  1240.3±15.21µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    126.7±2.43µs    23.3 MB/sec    1.00    127.0±2.16µs    23.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@sladyn98 reviewed on 2023-05-22 23:51_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_bugbear/rules/duplicate_exceptions.rs`:97 on 2023-05-22 23:51_

Yeah it makes sense now. 

---

_Renamed from "Migrate Fix::unspecified to Fix::safe or Fix::maybe_correct" to "Migrate `Fix::unspecified` to `Fix::safe` or `Fix::suggested`" by @sladyn98 on 2023-05-22 23:56_

---

_Marked ready for review by @sladyn98 on 2023-05-23 19:58_

---

_Comment by @sladyn98 on 2023-05-23 19:59_

@MichaReiser Do let me know if the choices made here for safe and suggested are correct. Marked this PR ready for review

---

_Comment by @charliermarsh on 2023-05-23 22:57_

I'll take a look at this tonight.

---

_Review request for @zanieb removed by @charliermarsh on 2023-05-23 22:57_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-05-23 22:57_

---

_Comment by @charliermarsh on 2023-05-24 02:10_

Reviewed these choices and they look reasonable to me based on the definitions.

---

_Renamed from "Migrate `Fix::unspecified` to `Fix::safe` or `Fix::suggested`" to "Migrate some rules from `Fix::unspecified`" by @charliermarsh on 2023-05-24 02:10_

---

_Merged by @charliermarsh on 2023-05-24 02:10_

---

_Closed by @charliermarsh on 2023-05-24 02:10_

---

_Branch deleted on 2023-05-24 04:49_

---
