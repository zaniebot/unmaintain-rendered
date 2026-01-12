```yaml
number: 6520
title: "Clarifying `target-version` in `flake8-future-annotations` docs"
type: pull_request
state: merged
author: jamesbraza
labels:
  - documentation
assignees: []
merged: true
base: main
head: fa-102-target-version
created_at: 2023-08-12T05:10:14Z
updated_at: 2023-08-12T19:18:53Z
url: https://github.com/astral-sh/ruff/pull/6520
synced_at: 2026-01-12T15:55:21Z
```

# Clarifying `target-version` in `flake8-future-annotations` docs

---

_@jamesbraza_

Closes https://github.com/astral-sh/ruff/issues/6461.

---

_@jamesbraza reviewed on 2023-08-12 05:10_

---

_Review comment by @jamesbraza on `crates/ruff/src/rules/flake8_future_annotations/rules/future_required_type_annotation.rs`:28 on 2023-08-12 05:10_

Will need someone to confirm this is the right way to link to `target-version`

---

_Comment by @github-actions[bot] on 2023-08-12 05:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.50ms     3.7 MB/sec    1.07     11.7±0.42ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.07ms     8.0 MB/sec    1.05      2.2±0.06ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   242.1±11.82µs    12.2 MB/sec    1.01   244.2±16.17µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.14ms     5.7 MB/sec    1.08      4.9±0.28ms     5.2 MB/sec
linter/all-rules/large/dataset.py          1.02     14.2±0.34ms     2.9 MB/sec    1.00     13.9±0.26ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.7±0.10ms     4.5 MB/sec    1.00      3.6±0.10ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   525.8±13.96µs     5.6 MB/sec    1.00   523.1±28.52µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.4±0.22ms     3.5 MB/sec    1.00      7.2±0.13ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.05      7.6±0.21ms     5.4 MB/sec    1.00      7.3±0.15ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1611.0±38.94µs    10.3 MB/sec    1.00  1574.2±44.40µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.7±8.83µs    14.9 MB/sec    1.01    200.2±9.06µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.3±0.08ms     7.6 MB/sec    1.00      3.2±0.07ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.09ms     4.1 MB/sec    1.00      9.9±0.10ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1929.8±18.72µs     8.6 MB/sec    1.00  1907.4±20.45µs     8.7 MB/sec
formatter/numpy/globals.py                 1.02    215.7±5.24µs    13.7 MB/sec    1.00    211.2±5.75µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.06ms     6.1 MB/sec    1.00      4.2±0.05ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.01     12.6±0.08ms     3.2 MB/sec    1.00     12.5±0.08ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.03ms     4.8 MB/sec    1.00      3.4±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.03    441.0±5.56µs     6.7 MB/sec    1.00    426.8±6.74µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.5±0.05ms     3.9 MB/sec    1.00      6.4±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.07ms     5.9 MB/sec    1.00      6.8±0.06ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1468.1±13.94µs    11.3 MB/sec    1.00  1464.6±24.39µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.04    176.5±2.75µs    16.7 MB/sec    1.00    169.3±6.67µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.1±0.03ms     8.3 MB/sec    1.00      3.0±0.04ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-12 18:50_

Thanks, I confirmed that the link works locally and fixed some unrelated pylint rules while I was investigating it.

---

_Label `documentation` added by @charliermarsh on 2023-08-12 18:50_

---

_Merged by @charliermarsh on 2023-08-12 19:01_

---

_Closed by @charliermarsh on 2023-08-12 19:01_

---
