```yaml
number: 3604
title: "Doc/CLN: pass pre-commit"
type: pull_request
state: merged
author: luke396
labels: []
assignees: []
merged: true
base: main
head: doc-improved-by-precommit
created_at: 2023-03-19T07:07:38Z
updated_at: 2023-03-20T02:18:44Z
url: https://github.com/astral-sh/ruff/pull/3604
synced_at: 2026-01-12T15:55:13Z
```

# Doc/CLN: pass pre-commit

---

_@luke396_

xref #3603

---

_@luke396 reviewed on 2023-03-19 07:10_

---

_Review comment by @luke396 on `scripts/generate_mkdocs.py`:11 on 2023-03-19 07:10_

Especially here, it seems that they should not be automatically removed

---

_Comment by @github-actions[bot] on 2023-03-19 07:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.01ms     2.9 MB/sec    1.06     14.6±0.01ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.00ms     4.5 MB/sec    1.04      3.9±0.00ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.2±1.51µs     6.9 MB/sec    1.05    447.2±1.53µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.04      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.02ms     5.4 MB/sec    1.09      8.3±0.01ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1684.4±2.79µs     9.9 MB/sec    1.06   1790.9±2.58µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    174.3±0.36µs    16.9 MB/sec    1.07    186.6±0.67µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.06      3.8±0.00ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.07ms     2.7 MB/sec    1.07     15.8±0.10ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.1 MB/sec    1.06      4.3±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.3±7.69µs     6.6 MB/sec    1.06    473.6±9.80µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.04ms     3.8 MB/sec    1.06      7.0±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.09      8.9±0.04ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1759.3±17.17µs     9.5 MB/sec    1.07  1883.4±18.15µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.6±2.21µs    16.2 MB/sec    1.10   200.0±20.39µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.06      4.0±0.03ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:11 on 2023-03-19 15:31_

Ah, I think this is "correct", but can we add `requires-python = ">=3.9"` to the `pyproject.toml` file in this directory? Right now it's defaulting to Python 3.10.

---

_@charliermarsh reviewed on 2023-03-19 15:31_

---

_Marked ready for review by @charliermarsh on 2023-03-19 19:11_

---

_Review comment by @charliermarsh on `scripts/generate_mkdocs.py`:11 on 2023-03-19 19:11_

Gonna add and merge.

---

_@charliermarsh reviewed on 2023-03-19 19:11_

---

_Merged by @charliermarsh on 2023-03-19 19:20_

---

_Closed by @charliermarsh on 2023-03-19 19:20_

---

_Comment by @onerandomusername on 2023-03-19 19:33_

How did this manage to make it to the default branch anyhow?

---

_Branch deleted on 2023-03-20 02:18_

---
