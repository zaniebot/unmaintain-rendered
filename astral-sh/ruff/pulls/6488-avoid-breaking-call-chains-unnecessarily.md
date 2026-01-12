```yaml
number: 6488
title: Avoid breaking call chains unnecessarily
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/call-chains
created_at: 2023-08-10T21:54:07Z
updated_at: 2023-08-11T13:59:36Z
url: https://github.com/astral-sh/ruff/pull/6488
synced_at: 2026-01-12T02:52:04Z
```

# Avoid breaking call chains unnecessarily

---

_Pull request opened by @charliermarsh on 2023-08-10 21:54_

## Summary

This PR attempts to fix the formatting of the following expression:

```python
max_message_id = (
    Message.objects.filter(recipient=recipient).order_by("id").reverse()[0].id
)
```

Specifically, Black preserves _that_ formatting, while we do:

```python
max_message_id = (
    Message.objects.filter(recipient=recipient)
    .order_by("id")
    .reverse()[0]
    .id
)
```

The fix here is to add a group around the entire call chain.

## Test Plan

Before:

- `zulip`: 0.99702
- `django`: 0.99784
- `warehouse`: 0.99585
- `build`: 0.75623
- `transformers`: 0.99470
- `cpython`: 0.75989
- `typeshed`: 0.74853

After:

- `zulip`: 0.99703
- `django`: 0.99791
- `warehouse`: 0.99586
- `build`: 0.75623
- `transformers`: 0.99470
- `cpython`: 0.75989
- `typeshed`: 0.74853


---

_Label `formatter` added by @charliermarsh on 2023-08-10 21:56_

---

_Comment by @github-actions[bot] on 2023-08-10 22:23_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.02ms     5.0 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1659.5±48.69µs    10.0 MB/sec    1.00  1637.0±19.45µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    188.0±5.93µs    15.7 MB/sec    1.00    187.7±4.44µs    15.7 MB/sec
formatter/pydantic/types.py                1.00      3.5±0.09ms     7.3 MB/sec    1.01      3.5±0.11ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.00     10.2±0.02ms     4.0 MB/sec    1.01     10.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.01      2.8±0.00ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    392.0±0.74µs     7.5 MB/sec    1.01    396.0±0.77µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.04ms     4.8 MB/sec    1.01      5.4±0.01ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.01ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1198.6±15.62µs    13.9 MB/sec    1.00   1192.8±5.69µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    142.0±2.37µs    20.8 MB/sec    1.01    144.0±4.32µs    20.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.03ms    10.4 MB/sec    1.00      2.4±0.04ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.5±0.15ms     3.9 MB/sec    1.00     10.3±0.19ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.02  1984.3±28.59µs     8.4 MB/sec    1.00  1943.0±24.07µs     8.6 MB/sec
formatter/numpy/globals.py                 1.02    219.0±4.28µs    13.5 MB/sec    1.00    213.6±6.36µs    13.8 MB/sec
formatter/pydantic/types.py                1.01      4.4±0.07ms     5.8 MB/sec    1.00      4.3±0.07ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.2±0.24ms     3.1 MB/sec    1.03     13.5±0.21ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.03ms     4.7 MB/sec    1.02      3.6±0.05ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    447.8±7.83µs     6.6 MB/sec    1.01    451.8±7.93µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.13ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.08ms     5.7 MB/sec    1.01      7.2±0.07ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1514.2±20.94µs    11.0 MB/sec    1.00  1513.1±18.79µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    176.4±3.57µs    16.7 MB/sec    1.01    179.0±4.69µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @konstin by @charliermarsh on 2023-08-11 00:12_

---

_Marked ready for review by @charliermarsh on 2023-08-11 00:12_

---

_@charliermarsh reviewed on 2023-08-11 00:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:38 on 2023-08-11 00:12_

(This is purely indenting the existing body in a `format_with`. No changes in the indented body.)

---

_@MichaReiser requested changes on 2023-08-11 05:46_

Please update your test plan with the similarity index. It is otherwise hard to judge if this is an incorrect special case or indeed the expected behavior

---

_Comment by @charliermarsh on 2023-08-11 06:04_

Every time I do this manually, a little part of me dies.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-11 06:06_

---

_Comment by @charliermarsh on 2023-08-11 06:07_

Done.

---

_@MichaReiser approved on 2023-08-11 06:42_

Thanks. 

Yeah, the process is cumbersome especially because you need to manually delete text. 

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_attribute.rs`:145 on 2023-08-11 10:46_

nit: you can use `==` here

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/parentheses/call_chains.py`:159 on 2023-08-11 10:56_

This works but i'd like to have it be part of the tests

```suggestion
)

max_message_id = (
    Message.objects.filter(recipient=recipient).order_by("id").reverse()[0].id()
)
```

---

_@konstin approved on 2023-08-11 10:56_

---

_Comment by @charliermarsh on 2023-08-11 13:22_

> Yeah, the process is cumbersome especially because you need to manually delete text.

Also because I have to go find another PR on main to find the baseline, right?

---

_Merged by @charliermarsh on 2023-08-11 13:33_

---

_Closed by @charliermarsh on 2023-08-11 13:33_

---

_Branch deleted on 2023-08-11 13:33_

---
