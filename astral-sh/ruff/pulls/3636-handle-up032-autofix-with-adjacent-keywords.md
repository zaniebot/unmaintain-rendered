```yaml
number: 3636
title: "Handle `UP032` autofix with adjacent keywords"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-up032-auto-fix
created_at: 2023-03-20T22:15:55Z
updated_at: 2023-03-21T00:33:15Z
url: https://github.com/astral-sh/ruff/pull/3636
synced_at: 2026-01-12T15:55:13Z
```

# Handle `UP032` autofix with adjacent keywords

---

_@JonathanPlasse_

- Close #3627

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:25_

There might be more cases to consider here -- I haven't really thought it through rigorously... But for example, `rf"{expr}"` is valid.

---

_@charliermarsh reviewed on 2023-03-20 22:25_

---

_@charliermarsh reviewed on 2023-03-20 22:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:26_

But I don't think we can _just_ check `r`, because some keywords end in `r`.

---

_Comment by @github-actions[bot] on 2023-03-20 22:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     13.8±1.41ms     3.0 MB/sec     1.02     14.0±1.90ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec     1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    492.3±0.95µs     6.0 MB/sec     1.00    492.5±0.92µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.60ms     4.1 MB/sec     1.00      6.1±0.58ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.28ms     5.6 MB/sec     1.00      7.2±0.28ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1801.8±339.55µs     9.2 MB/sec    1.00  1759.6±321.60µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    182.3±0.54µs    16.2 MB/sec     1.01    183.2±1.34µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.5 MB/sec     1.00      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.21ms     2.5 MB/sec    1.03     16.5±0.24ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.12ms     3.8 MB/sec    1.02      4.5±0.12ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   540.8±12.05µs     5.5 MB/sec    1.00   536.3±22.08µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.13ms     3.6 MB/sec    1.03      7.3±0.29ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.01      8.6±0.10ms     4.7 MB/sec    1.00      8.5±0.14ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1906.3±22.33µs     8.7 MB/sec    1.00  1894.8±31.90µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.8±4.20µs    14.4 MB/sec    1.10   225.2±20.11µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.07ms     6.4 MB/sec    1.02      4.1±0.20ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@JonathanPlasse reviewed on 2023-03-20 22:32_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:32_

`rf"{expr}"` is inside the range of the expression, it the character before the expression that is looked at, not `r` from the string.
Also it is not possible to have `r` before the string in this case as it would be a syntax error `returnr"foo"`.

---

_@JonathanPlasse reviewed on 2023-03-20 22:35_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:35_

There is also `and`, `or`, `not`, ...
That is why I used `is_ascii_alphabetic`.

---

_@charliermarsh reviewed on 2023-03-20 22:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:44_

Ahh right right.

And this is valid, right?

```py
return\
f"foo"
```

---

_@JonathanPlasse reviewed on 2023-03-20 22:56_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:56_

The auto-fix works in this case too.

---

_@JonathanPlasse reviewed on 2023-03-20 22:57_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:270 on 2023-03-20 22:57_

```python
def f():
    return\
"{}".format(1)
```
becomes
```python
def f():
    return\
f"{1}"
```

---

_Renamed from "FIx UP032 auto-fix" to "Handle `UP032` autofix with adjacent keywords" by @charliermarsh on 2023-03-21 00:11_

---

_Label `bug` added by @charliermarsh on 2023-03-21 00:11_

---

_Merged by @charliermarsh on 2023-03-21 00:17_

---

_Closed by @charliermarsh on 2023-03-21 00:17_

---

_Branch deleted on 2023-03-21 00:19_

---
