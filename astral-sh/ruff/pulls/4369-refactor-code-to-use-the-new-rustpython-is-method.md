```yaml
number: 4369
title: "Refactor code to use the new RustPython `is` method"
type: pull_request
state: merged
author: youknowone
labels: []
assignees: []
merged: true
base: main
head: ast-utilities
created_at: 2023-05-11T09:54:52Z
updated_at: 2023-05-11T20:54:43Z
url: https://github.com/astral-sh/ruff/pull/4369
synced_at: 2026-01-12T15:55:15Z
```

# Refactor code to use the new RustPython `is` method

---

_@youknowone_

Related parser patch https://github.com/RustPython/Parser/pull/20

I made a few changes as example.

---

_@youknowone reviewed on 2023-05-11 09:56_

---

_Review comment by @youknowone on `crates/ruff/src/rules/flake8_annotations/rules.rs`:422 on 2023-05-11 09:56_

Or even `expr.node.is_constant_none()`?

---

_@MichaReiser reviewed on 2023-05-11 10:03_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_annotations/rules.rs`:422 on 2023-05-11 10:03_

I think it's fine the way it is. An alternative would be to do `expr.as_constant().map_or(false, |constant| constant.is_none())` but that's at least as verbose. 

---

_@MichaReiser approved on 2023-05-11 10:04_

---

_Converted to draft by @youknowone on 2023-05-11 10:12_

---

_Comment by @youknowone on 2023-05-11 10:12_

changed to draft not to accidently merge it before parser merge the patch.

---

_Comment by @github-actions[bot] on 2023-05-11 10:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     14.2±0.12ms     2.9 MB/sec    1.00     13.7±0.05ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    409.6±2.24µs     7.2 MB/sec    1.01    412.2±0.80µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.01ms     4.5 MB/sec    1.00      5.7±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.02ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1445.7±3.27µs    11.5 MB/sec    1.00   1441.6±3.78µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    161.9±0.33µs    18.2 MB/sec    1.00    159.9±0.17µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.02ms     8.4 MB/sec
parser/large/dataset.py                    1.01      5.4±0.01ms     7.5 MB/sec    1.00      5.3±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.01   1053.5±0.65µs    15.8 MB/sec    1.00   1042.3±1.08µs    16.0 MB/sec
parser/numpy/globals.py                    1.02    107.7±0.17µs    27.4 MB/sec    1.00    105.9±0.47µs    27.9 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.02ms    11.0 MB/sec    1.00      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.10ms     2.4 MB/sec    1.00     16.7±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.03ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04    436.0±6.59µs     6.8 MB/sec    1.00    419.7±5.51µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.06ms     3.6 MB/sec    1.00      7.1±0.13ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.04ms     5.0 MB/sec    1.02      8.3±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.5±13.26µs     9.7 MB/sec    1.01  1740.7±12.28µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    185.2±1.38µs    15.9 MB/sec    1.00    183.5±2.08µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.02      3.8±0.02ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.04ms     6.0 MB/sec    1.00      6.8±0.08ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1284.5±9.68µs    13.0 MB/sec    1.01  1301.6±10.63µs    12.8 MB/sec
parser/numpy/globals.py                    1.00    132.8±1.35µs    22.2 MB/sec    1.01    134.4±1.55µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.02ms     8.9 MB/sec    1.01      2.9±0.02ms     8.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @youknowone on 2023-05-11 10:46_

---

_Renamed from "apply ast utilities" to "Refactor code to use the new RustPython `is` method" by @MichaReiser on 2023-05-11 14:16_

---

_Merged by @MichaReiser on 2023-05-11 14:16_

---

_Closed by @MichaReiser on 2023-05-11 14:16_

---

_Branch deleted on 2023-05-11 20:54_

---
