```yaml
number: 3726
title: "Add support for `.log(level, msg)` calls in `flake8-logging-format`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/detect-log-fn
created_at: 2023-03-25T06:01:10Z
updated_at: 2023-03-27T03:58:25Z
url: https://github.com/astral-sh/ruff/pull/3726
synced_at: 2026-01-12T15:55:13Z
```

# Add support for `.log(level, msg)` calls in `flake8-logging-format`

---

_@dhruvmanila_

fixes: #2562 

---

_Comment by @github-actions[bot] on 2023-03-25 06:26_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.03ms     2.9 MB/sec    1.01     14.4±0.01ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.01ms     4.4 MB/sec    1.00      3.8±0.00ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.9±1.42µs     6.8 MB/sec    1.00    434.8±1.06µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.01      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1711.6±3.69µs     9.7 MB/sec    1.00   1715.5±2.36µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.3±0.28µs    16.6 MB/sec    1.00    178.3±0.70µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.00ms     6.9 MB/sec    1.00      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     11.1±0.06ms     3.7 MB/sec    1.03     11.4±0.06ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.04ms     5.5 MB/sec    1.01      3.1±0.01ms     5.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    334.9±2.74µs     8.8 MB/sec    1.01    339.7±3.55µs     8.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.09ms     5.2 MB/sec    1.03      5.1±0.03ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.02ms     6.8 MB/sec    1.08      6.4±0.03ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1289.3±8.50µs    12.9 MB/sec    1.04   1346.2±9.81µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    132.4±1.33µs    22.3 MB/sec    1.05    139.5±1.86µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.03ms     9.2 MB/sec    1.06      3.0±0.03ms     8.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_logging_format/G001.py`:5 on 2023-03-25 15:49_

Can we add a test case for, e.g., `logging.log(msg="foo", level=logging.INFO)`?

---

_@charliermarsh reviewed on 2023-03-25 15:49_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_logging_format/G001.py`:5 on 2023-03-25 15:50_

Actually I got it, looks like the code works correctly :)

---

_@charliermarsh reviewed on 2023-03-25 15:50_

---

_@charliermarsh reviewed on 2023-03-25 15:51_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_logging_format/rules.rs`:175 on 2023-03-25 15:51_

I wonder if we should extend this to warn on `logging.log(logging.WARN, "...")`

---

_Merged by @charliermarsh on 2023-03-25 15:55_

---

_Closed by @charliermarsh on 2023-03-25 15:55_

---

_@charliermarsh reviewed on 2023-03-25 15:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_logging_format/rules.rs`:175 on 2023-03-25 15:56_

(Can be in a separate PR if needed, not even sure whether that's correct to flag.)

---

_@dhruvmanila reviewed on 2023-03-26 02:17_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_logging_format/rules.rs`:175 on 2023-03-26 02:17_

Regarding the level variable, I'm not sure as the docs doesn't mention anything about it although the `WARN` level isn't mentioned in the [levels](https://docs.python.org/3/library/logging.html#logging-levels) table.

If we do decide to change it, it'll need to be updated wherever the `level` argument is accepted.

---

_Branch deleted on 2023-03-27 03:58_

---
