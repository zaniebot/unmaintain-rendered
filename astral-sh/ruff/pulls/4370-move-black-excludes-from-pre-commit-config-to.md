```yaml
number: 4370
title: Move black excludes from pre-commit config to pyproject.toml
type: pull_request
state: merged
author: calumy
labels:
  - internal
assignees: []
merged: true
base: main
head: move-black-excludes
created_at: 2023-05-11T10:48:05Z
updated_at: 2023-05-11T13:06:02Z
url: https://github.com/astral-sh/ruff/pull/4370
synced_at: 2026-01-12T15:55:15Z
```

# Move black excludes from pre-commit config to pyproject.toml

---

_@calumy_

Currently, when black is run for the project with `black .`, it tries to format files in:
- `crates/ruff/resources/test/fixtures` 
- `crates/ruff_python_formatter/resources/test/fixtures/black/simple_cases`. 

This behaviour differs from when running black with pre-commit as these files are excluded. 

The files that black is attempting to format should not be formatted as they are test fixtures.

This PR moves the exclusion from the pre-commit config to a black specific config in `pyproject.toml`.

---

_Comment by @github-actions[bot] on 2023-05-11 11:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.2±0.04ms     2.7 MB/sec    1.00     14.9±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.7±0.01ms     4.5 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.03    378.7±0.76µs     7.8 MB/sec    1.00    366.5±2.34µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.4±0.04ms     4.0 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.07      8.0±0.02ms     5.1 MB/sec    1.00      7.5±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.06   1643.4±4.61µs    10.1 MB/sec    1.00   1556.7±6.92µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.03    173.3±0.58µs    17.0 MB/sec    1.00    167.7±0.42µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.5±0.00ms     7.2 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.0 MB/sec    1.01      5.9±0.01ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1128.9±0.93µs    14.7 MB/sec    1.02   1149.7±1.62µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    116.8±0.28µs    25.3 MB/sec    1.00    116.4±0.24µs    25.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.03ms    10.3 MB/sec    1.01      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.31ms     2.4 MB/sec    1.03     17.1±0.21ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     3.9 MB/sec    1.00      4.2±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.4±7.64µs     6.0 MB/sec    1.01   497.7±11.32µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.10ms     3.6 MB/sec    1.00      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.03      8.4±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1725.5±21.05µs     9.6 MB/sec    1.04  1786.2±22.72µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    193.8±3.06µs    15.2 MB/sec    1.04    201.3±6.21µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.03      3.8±0.05ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.7±0.09ms     6.1 MB/sec    1.02      6.8±0.04ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1265.4±16.77µs    13.2 MB/sec    1.02  1295.2±17.39µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    127.3±2.11µs    23.2 MB/sec    1.04    131.9±3.67µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.02      2.9±0.03ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-11 13:00_

---

_Closed by @charliermarsh on 2023-05-11 13:00_

---

_Label `internal` added by @charliermarsh on 2023-05-11 13:00_

---

_Comment by @charliermarsh on 2023-05-11 13:00_

Thanks!

---

_Branch deleted on 2023-05-11 13:06_

---
