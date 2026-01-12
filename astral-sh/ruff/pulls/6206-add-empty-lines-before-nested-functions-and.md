```yaml
number: 6206
title: Add empty lines before nested functions and classes
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/nested-function
created_at: 2023-07-31T19:11:21Z
updated_at: 2023-08-01T15:47:23Z
url: https://github.com/astral-sh/ruff/pull/6206
synced_at: 2026-01-12T15:55:20Z
```

# Add empty lines before nested functions and classes

---

_@charliermarsh_

## Summary

This PR ensures that if a function or class is the first statement in a nested suite that _isn't_ a function or class body, we insert a leading newline.

For example, given:

```python
def f():
    if True:

        def register_type():
            pass
```

We _want_ to preserve the newline, whereas today, we remove it.

Note that this only applies when the function or class doesn't have any leading comments.

Closes https://github.com/astral-sh/ruff/issues/6066.


---

_Review requested from @konstin by @charliermarsh on 2023-07-31 19:11_

---

_Comment by @github-actions[bot] on 2023-07-31 19:44_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.2±0.13ms     5.0 MB/sec    1.05      8.6±0.11ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1625.3±25.34µs    10.2 MB/sec    1.06  1722.8±37.61µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    182.0±3.79µs    16.2 MB/sec    1.07    194.3±7.01µs    15.2 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.12ms     7.2 MB/sec    1.04      3.7±0.09ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.0±0.08ms     3.7 MB/sec    1.01     11.1±0.10ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.01      2.8±0.03ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    380.8±0.36µs     7.7 MB/sec    1.00    381.8±0.66µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.08ms     5.2 MB/sec    1.03      5.1±0.13ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.8±0.05ms     7.0 MB/sec    1.01      5.8±0.06ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1204.9±21.81µs    13.8 MB/sec    1.00  1210.6±12.42µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.0±3.45µs    22.0 MB/sec    1.00    134.2±0.22µs    22.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.6±0.06ms     9.9 MB/sec    1.00      2.6±0.04ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.15ms     4.0 MB/sec    1.02     10.2±0.15ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1943.7±32.39µs     8.6 MB/sec    1.00  1939.2±26.89µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    211.1±4.61µs    14.0 MB/sec    1.01    212.9±6.92µs    13.9 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.07ms     5.9 MB/sec    1.00      4.3±0.06ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.02     14.0±0.17ms     2.9 MB/sec    1.00     13.7±0.15ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.7±0.06ms     4.6 MB/sec    1.00      3.6±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    446.9±7.70µs     6.6 MB/sec    1.00    440.1±8.07µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.19ms     4.0 MB/sec    1.00      6.2±0.13ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.6±0.12ms     5.3 MB/sec    1.00      7.5±0.11ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1520.4±35.64µs    11.0 MB/sec    1.01  1536.2±29.39µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.4±3.36µs    17.4 MB/sec    1.01    171.1±3.55µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.04ms     7.8 MB/sec    1.01      3.3±0.04ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @charliermarsh on 2023-08-01 04:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:68 on 2023-08-01 06:00_

Nit: Should the example include the empty line? I was reading the example and it was unclear to me what empty line it would insert.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:75 on 2023-08-01 06:01_

Nit: I prefer to move the "common" case out of branches
```suggestion
				empty_line().fmt(f)?;
			}
			
			first.format().fmt(f)?;
```

---

_@MichaReiser approved on 2023-08-01 06:02_

> Note that this only applies when the function or class doesn't have any leading comments.

Because Black only inserts the new line when there's no leading comment? This feels a bit strange to me.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:11 on 2023-08-01 07:14_

nit: maybe `SuiteParent` or `SuiteParentKind` rather than `SuiteLevel`

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:22 on 2023-08-01 07:15_

nit: rename to `Other`, `Function` and `Class` are also nested.

---

_@konstin approved on 2023-08-01 07:15_

---

_Merged by @charliermarsh on 2023-08-01 15:31_

---

_Closed by @charliermarsh on 2023-08-01 15:31_

---

_Branch deleted on 2023-08-01 15:31_

---
