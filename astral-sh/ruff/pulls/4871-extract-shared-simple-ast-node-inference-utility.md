```yaml
number: 4871
title: Extract shared simple AST node inference utility
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/data-typ
created_at: 2023-06-05T18:16:17Z
updated_at: 2023-06-05T18:48:53Z
url: https://github.com/astral-sh/ruff/pull/4871
synced_at: 2026-01-12T15:55:16Z
```

# Extract shared simple AST node inference utility

---

_@charliermarsh_

## Summary

We already have this utility for `bad-string-format-type`, and we added something similar for `invalid-str-return`. This PR extracts the utility from the former, and uses it for the latter.

Note that this utility only works on individual AST nodes, so it can detect that `[1, 2, 3]` is a list, but it can't detect that `x = [1, 2, 3]; x` is a list (it treats that as `PythonType::Unknown`).


---

_Label `internal` added by @charliermarsh on 2023-06-05 18:16_

---

_Merged by @charliermarsh on 2023-06-05 18:23_

---

_Closed by @charliermarsh on 2023-06-05 18:23_

---

_Branch deleted on 2023-06-05 18:23_

---

_Comment by @github-actions[bot] on 2023-06-05 18:27_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.03ms     2.9 MB/sec    1.01     13.9±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.8±0.35µs     7.0 MB/sec    1.00    418.8±0.51µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.0 MB/sec    1.01      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1479.3±5.25µs    11.3 MB/sec    1.00   1480.5±1.73µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    164.8±0.17µs    17.9 MB/sec    1.01    165.9±1.03µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.02      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1020.9±0.52µs    16.3 MB/sec    1.00   1018.2±2.32µs    16.4 MB/sec
parser/numpy/globals.py                    1.00    106.5±0.25µs    27.7 MB/sec    1.00    106.9±2.03µs    27.6 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.00      2.2±0.00ms    11.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.4±0.15ms     2.3 MB/sec    1.00     17.3±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.01      4.4±0.06ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    441.4±7.58µs     6.7 MB/sec    1.00    436.9±6.84µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.13ms     3.4 MB/sec    1.00      7.4±0.13ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.7±0.07ms     4.7 MB/sec    1.00      8.5±0.07ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1803.9±20.39µs     9.2 MB/sec    1.00  1779.9±17.17µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    190.6±2.44µs    15.5 MB/sec    1.00    189.0±4.25µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.9±0.03ms     6.5 MB/sec    1.00      3.9±0.09ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.9±0.05ms     5.9 MB/sec    1.00      6.9±0.04ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1285.5±14.55µs    13.0 MB/sec    1.01   1301.0±7.56µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    134.2±1.19µs    22.0 MB/sec    1.00    134.5±1.05µs    21.9 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.02      2.9±0.03ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
