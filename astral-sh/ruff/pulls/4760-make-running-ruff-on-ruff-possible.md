```yaml
number: 4760
title: Make running ruff on ruff possible
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: make_running_ruff_on_ruff_possible
created_at: 2023-05-31T16:03:10Z
updated_at: 2023-05-31T16:37:31Z
url: https://github.com/astral-sh/ruff/pull/4760
synced_at: 2026-01-12T15:55:16Z
```

# Make running ruff on ruff possible

---

_@konstin_

I was wondering why `pip install -U ruff && ruff .` in the ruff repo would result in only noise while the pre-commit ruff works. Turns out the pre-commit has an exclude for the resources directories.

## Summary

This adds the excludes from pre-commit to ruff's own pyproject.toml so `ruff .` works on ruff itself

## Test Plan

`pip install -U ruff && ruff .`


---

_@charliermarsh approved on 2023-05-31 16:05_

---

_Comment by @github-actions[bot] on 2023-05-31 16:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.04ms     2.7 MB/sec    1.00     15.1±0.13ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.01ms     4.5 MB/sec    1.00      3.6±0.03ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    377.0±1.21µs     7.8 MB/sec    1.00    376.4±0.84µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.01ms     4.0 MB/sec    1.01      6.4±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1581.1±4.22µs    10.5 MB/sec    1.01   1592.5±4.29µs    10.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.7±0.30µs    17.0 MB/sec    1.02    177.1±3.68µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.7±0.02ms     7.1 MB/sec    1.02      5.9±0.00ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1120.6±5.41µs    14.9 MB/sec    1.02   1147.9±1.78µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    115.7±0.37µs    25.5 MB/sec    1.03    118.7±0.79µs    24.9 MB/sec
parser/pydantic/types.py                   1.00      2.4±0.00ms    10.4 MB/sec    1.03      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.9±0.26ms     2.4 MB/sec    1.00     16.9±0.30ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.4±0.16ms     3.8 MB/sec    1.00      4.3±0.12ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.4±14.57µs     5.8 MB/sec    1.00   506.6±13.34µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.2±0.19ms     3.6 MB/sec    1.00      7.1±0.18ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.15ms     4.9 MB/sec    1.00      8.3±0.13ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1799.0±46.42µs     9.3 MB/sec    1.00  1784.1±39.39µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    208.9±6.41µs    14.1 MB/sec    1.02   212.8±19.56µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.07ms     6.8 MB/sec    1.01      3.8±0.10ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.5±0.09ms     6.3 MB/sec    1.00      6.5±0.10ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1233.3±31.26µs    13.5 MB/sec    1.00  1231.2±43.18µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    125.8±3.84µs    23.5 MB/sec    1.01    127.0±5.45µs    23.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.07ms     9.2 MB/sec    1.00      2.8±0.07ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @konstin on 2023-05-31 16:19_

---

_Closed by @konstin on 2023-05-31 16:19_

---

_Branch deleted on 2023-05-31 16:19_

---
