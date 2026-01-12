```yaml
number: 5136
title: "Avoid propagating `BindingKind::Global` and `BindingKind::Nonlocal`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/globals
created_at: 2023-06-16T02:59:01Z
updated_at: 2023-06-16T15:19:53Z
url: https://github.com/astral-sh/ruff/pull/5136
synced_at: 2026-01-12T03:43:30Z
```

# Avoid propagating `BindingKind::Global` and `BindingKind::Nonlocal`

---

_Pull request opened by @charliermarsh on 2023-06-16 02:59_

## Summary

This PR fixes a small quirk in the semantic model. Typically, when we see an import, like `import foo`, we create a `BindingKind::Importation` for it. However, if `foo` has been declared as a `global`, then we propagate the kind forward. So given:

```python
global foo

import foo
```

We'd create two bindings for `foo`, both with type `global`.

This was originally borrowed from Pyflakes, and it exists to help avoid false-positives like:

```python
def f():
    global foo

    # Don't mark `foo` as "assigned but unused"! It's a global!
    foo = 1
```

This PR removes that behavior, and instead tracks "Does this binding refer to a global?" as a flag. This is much cleaner, since it means we don't "lose" the identity of various bindings.

As a very strange example of why this matters, consider:

```python
def foo():
    global Member

    from module import Member

    x: Member = 1
```

`Member` is only used in a typing context, so we should flag it and say "move it to a `TYPE_CHECKING` block". However, when we go to analyze `from module import Member`, it has `BindingKind::Global`. So we don't even know that it's an import!


---

_Review requested from @konstin by @charliermarsh on 2023-06-16 02:59_

---

_Comment by @github-actions[bot] on 2023-06-16 03:11_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.03ms     5.3 MB/sec    1.00      7.6±0.03ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1582.6±2.57µs    10.5 MB/sec    1.00  1579.9±10.40µs    10.5 MB/sec
formatter/numpy/globals.py                 1.01    154.5±3.80µs    19.1 MB/sec    1.00    153.2±0.35µs    19.3 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.07ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.06     17.1±0.07ms     2.4 MB/sec    1.00     16.2±0.06ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.20ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    502.8±1.24µs     5.9 MB/sec    1.05   526.7±31.32µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.04ms     3.5 MB/sec    1.00      7.3±0.38ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.11      8.9±0.03ms     4.6 MB/sec    1.00      8.1±0.21ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04   1864.6±2.56µs     8.9 MB/sec    1.00  1795.2±47.15µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.04   212.2±17.78µs    13.9 MB/sec    1.00    205.0±8.68µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.1±0.15ms     6.2 MB/sec    1.00      3.9±0.16ms     6.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.08ms     5.2 MB/sec    1.00      7.9±0.08ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.01  1606.1±25.86µs    10.4 MB/sec    1.00  1597.2±18.85µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    157.1±1.74µs    18.8 MB/sec    1.03   161.2±10.51µs    18.3 MB/sec
formatter/pydantic/types.py                1.01      3.2±0.03ms     7.9 MB/sec    1.00      3.2±0.03ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     16.6±0.15ms     2.5 MB/sec    1.00     16.6±0.18ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.9 MB/sec    1.01      4.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.8±9.64µs     6.8 MB/sec    1.00    437.8±7.05µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.10ms     3.5 MB/sec    1.00      7.3±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.07ms     4.8 MB/sec    1.01      8.5±0.07ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1770.5±30.83µs     9.4 MB/sec    1.01  1786.7±37.17µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    190.6±7.02µs    15.5 MB/sec    1.00    191.5±6.42µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.03ms     6.6 MB/sec    1.00      3.9±0.03ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-06-16 06:13_

From the ecosystem checks:

> airflow/configuration.py:1513:13: F841 [*] Local variable `FERNET_KEY` is assigned to but never used

Through https://github.com/apache/airflow/blob/bdfebad5c9491234a78453856bd8c3baac98f75e/airflow/configuration.py#L1805 and seeing `AIRFLOW__CORE__FERNET_KEY` else in the code it looks like they are using that module level argument elsewhere through metaprogramming. It's a bit of a problem because there's no `__all__`, but marking this as an unused local variable is wrong.

Otherwise the ecosystem checks improve.

---

_@konstin approved on 2023-06-16 06:18_

---

_Comment by @charliermarsh on 2023-06-16 14:43_

Thanks, that's a bug, will fix :)

---

_Merged by @charliermarsh on 2023-06-16 15:07_

---

_Closed by @charliermarsh on 2023-06-16 15:07_

---

_Branch deleted on 2023-06-16 15:07_

---
