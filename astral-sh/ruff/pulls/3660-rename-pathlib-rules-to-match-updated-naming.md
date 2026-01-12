```yaml
number: 3660
title: "Rename `pathlib` rules to match updated naming convention"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/names
created_at: 2023-03-22T02:23:37Z
updated_at: 2023-03-22T15:35:47Z
url: https://github.com/astral-sh/ruff/pull/3660
synced_at: 2026-01-12T15:55:13Z
```

# Rename `pathlib` rules to match updated naming convention

---

_@charliermarsh_

Instead of `PathlibAbspath`, we want `OsPathAbspath`, as in "allow `os.path.abspath`".

See: #2902.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-22 02:25_

---

_Renamed from "Rename pathlib rules to match updated naming convention" to "Rename `pathlib` rules to match updated naming convention" by @charliermarsh on 2023-03-22 02:25_

---

_Comment by @github-actions[bot] on 2023-03-22 02:48_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.8±0.50ms     2.4 MB/sec    1.00     16.7±0.42ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.17ms     3.8 MB/sec    1.00      4.3±0.20ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.03   611.1±20.17µs     4.8 MB/sec    1.00   595.9±20.83µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.22ms     3.5 MB/sec    1.00      7.4±0.28ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.04      9.1±0.39ms     4.5 MB/sec    1.00      8.8±0.29ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.0±0.09ms     8.3 MB/sec    1.00  1961.2±62.54µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   240.3±10.93µs    12.3 MB/sec    1.00   238.8±11.01µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.14ms     6.0 MB/sec    1.00      4.2±0.18ms     6.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.7±0.03ms     3.2 MB/sec    1.02     13.0±0.05ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.01      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    369.7±2.24µs     8.0 MB/sec    1.02    375.4±2.44µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.5±0.05ms     4.6 MB/sec    1.01      5.6±0.02ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.03      7.4±0.05ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1478.6±5.00µs    11.3 MB/sec    1.02   1510.6±6.65µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    153.4±1.06µs    19.2 MB/sec    1.03    158.3±4.08µs    18.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.03      3.3±0.04ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2882 on 2023-03-22 08:12_

The name seems reasonable to me. Should we rename the `StringDotFormat` rule to `StringFormat` for consistency or rename these rules to `OsDotPathDotAbspath` (feels excessive to me)

---

_@MichaReiser approved on 2023-03-22 08:13_

---

_@charliermarsh reviewed on 2023-03-22 15:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2882 on 2023-03-22 15:35_

Yeah I think renaming to `StringFormat` or `StringFormatCall` is reasonable.

---

_Merged by @charliermarsh on 2023-03-22 15:35_

---

_Closed by @charliermarsh on 2023-03-22 15:35_

---

_Branch deleted on 2023-03-22 15:35_

---
