```yaml
number: 6208
title: Reset model state when exiting deferred visitors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/order
created_at: 2023-07-31T19:37:14Z
updated_at: 2023-07-31T20:05:52Z
url: https://github.com/astral-sh/ruff/pull/6208
synced_at: 2026-01-12T15:55:20Z
```

# Reset model state when exiting deferred visitors

---

_@charliermarsh_

## Summary

Very subtle bug related to the AST traversal. Given:

```python
from __future__ import annotations

from logging import getLogger

__all__ = ("getLogger",)


def foo() -> None:
    pass
```

We end up visiting the `-> None` annotation, then reusing the state snapshot when we go to visit the `__all__` exports, so when we visit `"getLogger"`, we think we're inside of a deferred type annotation.

This PR changes all the deferred visitors to snapshot and restore the state, which is a lot safer -- that way, the visitors avoid modifying the current visitor state. (Previously, they implicitly left the visitor state set to the state of the _last_ thing they visited.)

Closes https://github.com/astral-sh/ruff/issues/6207.

---

_Label `bug` added by @charliermarsh on 2023-07-31 19:37_

---

_Merged by @charliermarsh on 2023-07-31 19:46_

---

_Closed by @charliermarsh on 2023-07-31 19:46_

---

_Branch deleted on 2023-07-31 19:46_

---

_Comment by @github-actions[bot] on 2023-07-31 19:53_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -5, 0 error(s))

<details><summary>airflow (+0, -5)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/2ab78ec441a748ae4d99e429fe336b80a601d7b1/airflow/__init__.py#L128'>airflow/__init__.py:128:36:</a> TCH001 [*] Move application import `airflow.models.dag.DAG` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/2ab78ec441a748ae4d99e429fe336b80a601d7b1/airflow/__init__.py#L130'>airflow/__init__.py:130:41:</a> TCH001 [*] Move application import `airflow.models.xcom_arg.XComArg` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/2ab78ec441a748ae4d99e429fe336b80a601d7b1/airflow/decorators/__init__.py#L21'>airflow/decorators/__init__.py:21:37:</a> TCH001 [*] Move application import `airflow.decorators.base.TaskDecorator` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/2ab78ec441a748ae4d99e429fe336b80a601d7b1/airflow/decorators/__init__.py#L29'>airflow/decorators/__init__.py:29:43:</a> TCH001 [*] Move application import `airflow.decorators.task_group.task_group` into a type-checking block
- <a href='https://github.com/apache/airflow/blob/2ab78ec441a748ae4d99e429fe336b80a601d7b1/airflow/decorators/__init__.py#L30'>airflow/decorators/__init__.py:30:32:</a> TCH001 [*] Move application import `airflow.models.dag.dag` into a type-checking block
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| TCH001 | 5 | 0 | 5 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.50ms     3.9 MB/sec    1.03     10.9±0.52ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.18ms     7.8 MB/sec    1.01      2.1±0.19ms     7.8 MB/sec
formatter/numpy/globals.py                 1.04   230.8±21.36µs    12.8 MB/sec    1.00   221.9±18.40µs    13.3 MB/sec
formatter/pydantic/types.py                1.04      4.5±0.28ms     5.7 MB/sec    1.00      4.3±0.24ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.46ms     3.0 MB/sec    1.04     14.1±0.68ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.17ms     4.7 MB/sec    1.00      3.5±0.14ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   482.4±26.44µs     6.1 MB/sec    1.03   495.9±33.78µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.4±0.49ms     4.0 MB/sec    1.00      6.2±0.29ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.02      7.8±0.60ms     5.2 MB/sec    1.00      7.7±0.27ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1638.0±92.36µs    10.2 MB/sec    1.00  1611.8±145.12µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.06   188.4±13.49µs    15.7 MB/sec    1.00   177.8±13.55µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.12ms     7.4 MB/sec    1.01      3.5±0.18ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     12.1±0.39ms     3.4 MB/sec     1.00     12.2±0.52ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.3±0.12ms     7.3 MB/sec     1.03      2.4±0.06ms     7.1 MB/sec
formatter/numpy/globals.py                 1.00   263.5±12.18µs    11.2 MB/sec     1.02   269.2±14.28µs    11.0 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.21ms     4.9 MB/sec     1.00      5.2±0.15ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.11     17.8±1.67ms     2.3 MB/sec     1.00     16.1±0.67ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.3±0.47ms     3.9 MB/sec     1.00      4.3±0.24ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.05   558.9±71.62µs     5.3 MB/sec     1.00   533.0±63.75µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.24ms     3.4 MB/sec     1.03      7.8±0.68ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.06      9.5±0.32ms     4.3 MB/sec     1.00      9.0±0.27ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1941.0±109.24µs     8.6 MB/sec    1.00  1888.3±103.88µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    211.1±8.62µs    14.0 MB/sec     1.02    216.0±8.44µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.20ms     6.6 MB/sec     1.01      3.9±0.39ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
