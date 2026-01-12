```yaml
number: 6567
title: Clarify FBT documentation and refine rule names
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/fbt
created_at: 2023-08-14T17:33:23Z
updated_at: 2023-08-14T19:24:18Z
url: https://github.com/astral-sh/ruff/pull/6567
synced_at: 2026-01-12T15:55:21Z
```

# Clarify FBT documentation and refine rule names

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/6530.


---

_Label `documentation` added by @charliermarsh on 2023-08-14 17:33_

---

_Converted to draft by @charliermarsh on 2023-08-14 17:35_

---

_Comment by @github-actions[bot] on 2023-08-14 18:12_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.14ms     9.5 MB/sec    1.00      4.3±0.17ms     9.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   874.2±33.95µs    19.0 MB/sec    1.06   924.8±65.49µs    18.0 MB/sec
formatter/numpy/globals.py                 1.04     93.1±0.62µs    31.7 MB/sec    1.00     89.5±1.89µs    33.0 MB/sec
formatter/pydantic/types.py                1.00  1742.3±70.86µs    14.6 MB/sec    1.02  1775.3±44.28µs    14.4 MB/sec
linter/all-rules/large/dataset.py          1.00     11.9±0.40ms     3.4 MB/sec    1.01     12.1±0.33ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.10ms     5.2 MB/sec    1.04      3.3±0.16ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   438.3±16.37µs     6.7 MB/sec    1.05   459.6±13.18µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.17ms     4.1 MB/sec    1.03      6.4±0.20ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.1±0.15ms     6.6 MB/sec    1.04      6.4±0.21ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1375.3±38.03µs    12.1 MB/sec    1.01  1394.1±51.41µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.8±4.91µs    18.2 MB/sec    1.09    175.8±8.89µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.13ms     9.1 MB/sec    1.05      3.0±0.02ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.9±0.35ms     6.9 MB/sec    1.01      6.0±0.40ms     6.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1122.3±81.58µs    14.8 MB/sec    1.01  1131.0±75.92µs    14.7 MB/sec
formatter/numpy/globals.py                 1.00    107.5±6.42µs    27.4 MB/sec    1.02    109.5±9.26µs    26.9 MB/sec
formatter/pydantic/types.py                1.00      2.2±0.13ms    11.4 MB/sec    1.03      2.3±0.24ms    11.0 MB/sec
linter/all-rules/large/dataset.py          1.00     20.0±0.53ms     2.0 MB/sec    1.00     20.0±0.68ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.23ms     3.1 MB/sec    1.01      5.4±0.22ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.03   665.7±38.25µs     4.4 MB/sec    1.00   645.5±32.38µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00     10.3±0.40ms     2.5 MB/sec    1.00     10.3±0.44ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.42ms     3.8 MB/sec    1.01     10.9±0.43ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.3±0.12ms     7.4 MB/sec    1.00      2.2±0.11ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.03   282.7±19.76µs    10.4 MB/sec    1.00   275.7±19.09µs    10.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.21ms     5.3 MB/sec    1.00      4.8±0.24ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@tjkuson reviewed on 2023-08-14 18:29_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_boolean_trap/rules/boolean_default_value_in_function_definition.rs`:25 on 2023-08-14 18:29_

Does this example trigger the rule?

---

_@charliermarsh reviewed on 2023-08-14 18:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_boolean_trap/rules/boolean_default_value_in_function_definition.rs`:25 on 2023-08-14 18:49_

No, sorry, that was part of my confusion. The two rules now use similar documentation, and make it clear that the problem is just around positional arguments broadly, not default values specifically.

---

_Marked ready for review by @charliermarsh on 2023-08-14 18:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_boolean_trap/rules/boolean_default_value_in_function_definition.rs`:25 on 2023-08-14 18:57_

How does it look now? Wdyt?

---

_@charliermarsh reviewed on 2023-08-14 18:57_

---

_@tjkuson reviewed on 2023-08-14 19:20_

---

_Review comment by @tjkuson on `crates/ruff/src/rules/flake8_boolean_trap/rules/boolean_default_value_in_function_definition.rs`:25 on 2023-08-14 19:20_

I think it's a big improvement!

---

_Merged by @charliermarsh on 2023-08-14 19:24_

---

_Closed by @charliermarsh on 2023-08-14 19:24_

---

_Branch deleted on 2023-08-14 19:24_

---
