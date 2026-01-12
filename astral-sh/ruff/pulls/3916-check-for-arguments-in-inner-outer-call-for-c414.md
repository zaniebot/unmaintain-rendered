```yaml
number: 3916
title: "Check for arguments in inner/outer call for `C414`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/c414-sorted-args
created_at: 2023-04-08T07:27:25Z
updated_at: 2023-04-10T04:57:34Z
url: https://github.com/astral-sh/ruff/pull/3916
synced_at: 2026-01-12T15:55:14Z
```

# Check for arguments in inner/outer call for `C414`

---

_@dhruvmanila_

fixes: #3913 

---

_Comment by @github-actions[bot] on 2023-04-08 07:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.2±0.08ms     2.7 MB/sec    1.00     15.0±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.01    416.8±1.65µs     7.1 MB/sec    1.00    412.0±2.44µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.05ms     3.9 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.04ms     5.2 MB/sec    1.00      7.8±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1733.0±2.08µs     9.6 MB/sec    1.00   1719.0±3.62µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.01    181.9±0.49µs    16.2 MB/sec    1.00    180.6±0.81µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.01ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.18ms     2.5 MB/sec    1.00     16.4±0.39ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.05ms     4.0 MB/sec    1.00      4.1±0.09ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    498.3±7.84µs     5.9 MB/sec    1.00    486.5±6.35µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.10ms     3.7 MB/sec    1.00      6.8±0.14ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.08ms     4.9 MB/sec    1.00      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1832.8±27.57µs     9.1 MB/sec    1.00  1799.4±22.19µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.5±5.08µs    15.1 MB/sec    1.00    195.7±5.61µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.9±0.07ms     6.6 MB/sec    1.00      3.7±0.06ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_comprehensions/C414.py`:15 on 2023-04-09 02:35_

If I'm reading the description and code right, this is no longer flagged. But isn't this the same as just `sorted(x)`? Doesn't the inner sorted get "undone" by the outer sorted? Same with `set(sorted(x, key=lambda y: y))`.

---

_@charliermarsh reviewed on 2023-04-09 02:35_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_comprehensions/C414.py`:15 on 2023-04-09 03:01_

Oh right, I completely missed that. I was thinking from a different perspective. This means that the inner arguments need not be preserved and only the outer ones should be preserved.

---

_@dhruvmanila reviewed on 2023-04-09 03:01_

---

_@dhruvmanila reviewed on 2023-04-09 03:03_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_comprehensions/C414.py`:15 on 2023-04-09 03:03_

Ok, so the solution is to not preserve the inner arguments except for the `iterable`.

---

_Renamed from "Check for arguments in `sorted()` call for C414" to "Check for arguments in inner/outer call for C414" by @dhruvmanila on 2023-04-09 03:39_

---

_Renamed from "Check for arguments in inner/outer call for C414" to "Check for arguments in inner/outer call for `C414`" by @dhruvmanila on 2023-04-09 03:39_

---

_Merged by @charliermarsh on 2023-04-09 22:33_

---

_Closed by @charliermarsh on 2023-04-09 22:33_

---

_Branch deleted on 2023-04-10 04:57_

---
