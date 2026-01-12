```yaml
number: 4854
title: implement E307 for pylint invalid str return type
type: pull_request
state: merged
author: Ryang20718
labels:
  - rule
assignees: []
merged: true
base: main
head: implement_pylint_invalid_str_returnE0307
created_at: 2023-06-04T22:42:49Z
updated_at: 2023-06-05T18:12:58Z
url: https://github.com/astral-sh/ruff/pull/4854
synced_at: 2026-01-12T03:43:29Z
```

# implement E307 for pylint invalid str return type

---

_Pull request opened by @Ryang20718 on 2023-06-04 22:42_

## Summary

Implements  https://pylint.readthedocs.io/en/latest/user_guide/messages/error/invalid-str-returned.html for pylint


Throws an error when
```
a __str__ method returns something which is not a string
```


---

_Marked ready for review by @Ryang20718 on 2023-06-04 23:12_

---

_Comment by @github-actions[bot] on 2023-06-04 23:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.09ms     2.9 MB/sec    1.01     14.3±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.01      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    413.9±0.82µs     7.1 MB/sec    1.01    416.7±0.87µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.02      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1448.3±3.04µs    11.5 MB/sec    1.03   1485.9±7.04µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.5±0.20µs    18.3 MB/sec    1.02    164.8±4.58µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.03      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.8 MB/sec    1.00      5.2±0.01ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1018.5±0.53µs    16.3 MB/sec    1.00   1023.1±0.57µs    16.3 MB/sec
parser/numpy/globals.py                    1.00    106.9±0.35µs    27.6 MB/sec    1.00    107.1±0.27µs    27.5 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.4 MB/sec    1.00      2.2±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.08     18.3±0.12ms     2.2 MB/sec    1.00     16.9±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.5±0.03ms     3.7 MB/sec    1.00      4.3±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.04   447.5±15.02µs     6.6 MB/sec    1.00    428.8±5.80µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.07      7.7±0.05ms     3.3 MB/sec    1.00      7.2±0.04ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.17      9.8±0.05ms     4.2 MB/sec    1.00      8.4±0.03ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11  1958.5±12.39µs     8.5 MB/sec    1.00  1767.0±14.88µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.08    203.8±4.01µs    14.5 MB/sec    1.00    187.9±3.63µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.12      4.3±0.02ms     6.0 MB/sec    1.00      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.23      8.6±0.05ms     4.7 MB/sec    1.00      7.0±0.03ms     5.8 MB/sec
parser/numpy/ctypeslib.py                  1.19  1566.8±13.38µs    10.6 MB/sec    1.00  1314.9±28.70µs    12.7 MB/sec
parser/numpy/globals.py                    1.13    154.2±1.09µs    19.1 MB/sec    1.00    136.3±0.83µs    21.6 MB/sec
parser/pydantic/types.py                   1.20      3.5±0.02ms     7.3 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-05 03:03_

Hmm, we may not have sufficiently good type inference to support this rule reliably right now. For example, Pylint allows this:

```py
class Str:
    """__str__ returns int"""

    def __str__(self):
        x = "s"
        return x
```

Which makes sense. But we don't do type inference yet, so the best we could do is flag the method if you return a constant value that _isn't_ a string (e.g., an `int` or a `float` or a dictionary or something else) -- we'd have to allow arbitrary variables, attribute accesses, function calls, etc.

---

_Comment by @charliermarsh on 2023-06-05 03:04_

(At the very least, we probably need to change the check to only look for non-string constants, which will have a lot of false negatives compared to Pylint, but will avoid the huge false positive rate.)

---

_@charliermarsh reviewed on 2023-06-05 16:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/invalid_str_return.rs`:34 on 2023-06-05 16:01_

We can also flag tuples, dictionaries (literals and comprehensions), sets, generators, lists :)

---

_Label `rule` added by @charliermarsh on 2023-06-05 17:45_

---

_Merged by @charliermarsh on 2023-06-05 17:54_

---

_Closed by @charliermarsh on 2023-06-05 17:54_

---
