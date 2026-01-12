```yaml
number: 4066
title: "Ignore `N815` for `TypedDict` fields"
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: ignore-n815-for-typed-dict
created_at: 2023-04-22T15:59:31Z
updated_at: 2023-04-23T04:59:49Z
url: https://github.com/astral-sh/ruff/pull/4066
synced_at: 2026-01-12T15:55:14Z
```

# Ignore `N815` for `TypedDict` fields

---

_@JonathanPlasse_

- Close #4014

---

_Comment by @github-actions[bot] on 2023-04-22 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     14.7±0.06ms     2.8 MB/sec    1.00     14.3±0.07ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.00ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    456.9±0.75µs     6.5 MB/sec    1.01    462.5±0.89µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.2±0.02ms     4.1 MB/sec    1.00      6.0±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.04      7.5±0.02ms     5.4 MB/sec    1.00      7.2±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1655.1±4.80µs    10.1 MB/sec    1.00   1619.2±2.43µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.2±0.37µs    16.3 MB/sec    1.00    181.2±0.50µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.4±0.01ms     7.4 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec    1.01      5.8±0.00ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1142.2±0.66µs    14.6 MB/sec    1.01   1148.9±0.76µs    14.5 MB/sec
parser/numpy/globals.py                    1.00    114.6±0.36µs    25.7 MB/sec    1.01    115.4±0.28µs    25.6 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.01      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     22.3±0.73ms  1869.8 KB/sec    1.00     21.1±0.62ms  1973.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.7±0.25ms     2.9 MB/sec    1.00      5.5±0.24ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.05   675.7±46.69µs     4.4 MB/sec    1.00   646.5±57.72µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.04      9.4±0.33ms     2.7 MB/sec    1.00      9.0±0.48ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.05     10.9±0.31ms     3.7 MB/sec    1.00     10.3±0.39ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05      2.4±0.08ms     6.9 MB/sec    1.00      2.3±0.12ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.04   290.6±11.50µs    10.2 MB/sec    1.00   279.9±19.84µs    10.5 MB/sec
linter/default-rules/pydantic/types.py     1.05      5.0±0.18ms     5.1 MB/sec    1.00      4.8±0.33ms     5.3 MB/sec
parser/large/dataset.py                    1.02      9.0±0.30ms     4.5 MB/sec    1.00      8.8±0.29ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.04  1755.4±71.30µs     9.5 MB/sec    1.00  1687.5±61.20µs     9.9 MB/sec
parser/numpy/globals.py                    1.06   180.0±12.21µs    16.4 MB/sec    1.00   169.5±10.91µs    17.4 MB/sec
parser/pydantic/types.py                   1.05      3.9±0.16ms     6.5 MB/sec    1.00      3.7±0.14ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/rules/mixed_case_variable_in_class_scope.rs`:42 on 2023-04-22 16:27_

I think this should check `checker.ctx.match_typing_expr`.

---

_@charliermarsh reviewed on 2023-04-22 16:27_

---

_@charliermarsh reviewed on 2023-04-22 16:28_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pep8_naming/rules/mixed_case_variable_in_class_scope.rs`:48 on 2023-04-22 16:28_

Let's maybe move to a helper (`is_typeddict`) and do the check at the end here (`&& !helpers...`).

---

_Renamed from "Ignore N815 for TypedDict" to "Ignore `N815` for `TypedDict` fields" by @charliermarsh on 2023-04-22 22:17_

---

_Merged by @charliermarsh on 2023-04-22 22:17_

---

_Closed by @charliermarsh on 2023-04-22 22:17_

---

_Branch deleted on 2023-04-23 04:59_

---
