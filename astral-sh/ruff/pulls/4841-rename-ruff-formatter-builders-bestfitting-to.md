```yaml
number: 4841
title: "Rename `ruff_formatter::builders::BestFitting` to `FormatBestFitting`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: internal/4840
created_at: 2023-06-03T21:42:00Z
updated_at: 2023-06-03T22:25:22Z
url: https://github.com/astral-sh/ruff/pull/4841
synced_at: 2026-01-12T03:50:04Z
```

# Rename `ruff_formatter::builders::BestFitting` to `FormatBestFitting`

---

_Pull request opened by @zanieb on 2023-06-03 21:42_

Closes https://github.com/charliermarsh/ruff/issues/4840


---

_@MichaReiser approved on 2023-06-03 21:47_

Thanks. 

---

_Review comment by @zanieb on `crates/ruff_formatter/src/macros.rs`:328 on 2023-06-03 21:50_

Failure in documentation generation because this should be `BestFitting`

```
error: unresolved link to `crate::format_element::FormatBestFitting::most_expanded`
   --> crates/ruff_formatter/src/macros.rs:328:23
    |
328 | /// [`MostExpanded`]: crate::format_element::FormatBestFitting::most_expanded
    |                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ no item named `FormatBestFitting` in module `format_element`
    |
```

Do I need to expose `BestFitting` at https://github.com/charliermarsh/ruff/blob/90d47a5980a4b2fd47dabedfe5f2acd7b4081fcf/crates/ruff_formatter/src/lib.rs#L51 and use it here instead? I'm surprised tests passed if this is incorrect as-is.

---

_@zanieb reviewed on 2023-06-03 21:50_

---

_@zanieb reviewed on 2023-06-03 21:53_

---

_Review comment by @zanieb on `crates/ruff_formatter/src/macros.rs`:328 on 2023-06-03 21:53_

`from_arguments_unchecked` is on `FormatBestFitting` so perhaps the docs link is what is wrong

---

_@zanieb reviewed on 2023-06-03 21:54_

---

_Review comment by @zanieb on `crates/ruff_formatter/src/macros.rs`:328 on 2023-06-03 21:54_

https://github.com/charliermarsh/ruff/pull/4841/commits/28cd3afadabeb3e037da0c22c46bb4b213ee23b9

---

_Comment by @github-actions[bot] on 2023-06-03 22:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.09ms     2.9 MB/sec    1.00     14.1±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.5±0.70µs     7.0 MB/sec    1.00    425.1±2.53µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.05ms     6.0 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1498.3±4.29µs    11.1 MB/sec    1.00   1488.7±4.25µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.1±0.30µs    17.2 MB/sec    1.00    171.1±1.37µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.3±0.01ms     7.7 MB/sec    1.01      5.3±0.01ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1037.5±1.17µs    16.0 MB/sec    1.01   1045.6±0.72µs    15.9 MB/sec
parser/numpy/globals.py                    1.00    108.8±0.21µs    27.1 MB/sec    1.00    108.9±0.32µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.2 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.5±0.20ms     2.3 MB/sec    1.01     17.6±0.23ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.06ms     3.8 MB/sec    1.00      4.4±0.09ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    505.9±8.70µs     5.8 MB/sec    1.01    510.8±9.12µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.14ms     3.5 MB/sec    1.03      7.5±0.13ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.11ms     4.8 MB/sec    1.01      8.6±0.15ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1796.5±18.75µs     9.3 MB/sec    1.00  1800.2±21.67µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    203.3±5.91µs    14.5 MB/sec    1.01    205.6±6.30µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.10ms     6.7 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.5±0.05ms     6.3 MB/sec    1.01      6.5±0.05ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1212.3±18.15µs    13.7 MB/sec    1.02  1234.0±15.49µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    124.2±2.49µs    23.8 MB/sec    1.02    126.3±2.25µs    23.4 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.3 MB/sec    1.02      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @MichaReiser on 2023-06-03 22:13_

---

_Closed by @MichaReiser on 2023-06-03 22:13_

---
