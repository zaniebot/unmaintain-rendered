```yaml
number: 6074
title: Ignore some common builtin overrides on standard library subclasses
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/standard-library
created_at: 2023-07-25T15:42:58Z
updated_at: 2023-07-25T16:21:31Z
url: https://github.com/astral-sh/ruff/pull/6074
synced_at: 2026-01-12T15:55:20Z
```

# Ignore some common builtin overrides on standard library subclasses

---

_@charliermarsh_

## Summary

If a user subclasses `threading.Event`, e.g. with:

```python
from threading import Event


class CustomEvent(Event):
    def set(self) -> None:
        ...
```

They no control over the method name (`set`). This PR allows `threading.Event#set` and `logging.Filter#filter` overrides, and avoids flagging A003 in such cases. Ideally, we'd avoid flagging all overridden methods, but... that's a lot more difficult, and this is at least _better_ than what we do now.

Closes https://github.com/astral-sh/ruff/issues/6057.

Closes https://github.com/astral-sh/ruff/issues/5956.


---

_@zanieb approved on 2023-07-25 15:45_

---

_Merged by @charliermarsh on 2023-07-25 15:54_

---

_Closed by @charliermarsh on 2023-07-25 15:54_

---

_Branch deleted on 2023-07-25 15:54_

---

_Comment by @github-actions[bot] on 2023-07-25 15:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -6, 0 error(s))

<details><summary>airflow (+0, -4)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/9f3af9c862faec73cd477e8398b7607d2d045b37/airflow/cli/commands/celery_command.py#L123'>airflow/cli/commands/celery_command.py:123:17:</a> A003 Class attribute `filter` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/9f3af9c862faec73cd477e8398b7607d2d045b37/airflow/utils/log/secrets_masker.py#L192'>airflow/utils/log/secrets_masker.py:192:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/9f3af9c862faec73cd477e8398b7607d2d045b37/airflow/utils/log/trigger_handler.py#L41'>airflow/utils/log/trigger_handler.py:41:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
- <a href='https://github.com/apache/airflow/blob/9f3af9c862faec73cd477e8398b7607d2d045b37/airflow/utils/log/trigger_handler.py#L64'>airflow/utils/log/trigger_handler.py:64:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
</pre>

</p>
</details>
<details><summary>zulip (+0, -2)</summary>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/213387249e7ba7772084411b22d8cef64b135dd0/zerver/lib/logging_util.py#L111'>zerver/lib/logging_util.py:111:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
- <a href='https://github.com/zulip/zulip/blob/213387249e7ba7772084411b22d8cef64b135dd0/zerver/lib/logging_util.py#L116'>zerver/lib/logging_util.py:116:9:</a> A003 Class attribute `filter` is shadowing a Python builtin
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| A003 | 6 | 0 | 6 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.6±0.22ms     3.9 MB/sec    1.02     10.7±0.16ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.05ms     7.9 MB/sec    1.00      2.1±0.09ms     8.1 MB/sec
formatter/numpy/globals.py                 1.03    237.5±6.94µs    12.4 MB/sec    1.00   229.7±10.16µs    12.8 MB/sec
formatter/pydantic/types.py                1.07      4.5±0.09ms     5.6 MB/sec    1.00      4.3±0.16ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.06     14.5±0.29ms     2.8 MB/sec    1.00     13.7±0.32ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.7±0.07ms     4.6 MB/sec    1.00      3.6±0.12ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.07   483.6±10.15µs     6.1 MB/sec    1.00   452.0±18.66µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.13ms     3.8 MB/sec    1.00      6.5±0.23ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.03      7.5±0.13ms     5.4 MB/sec    1.00      7.2±0.27ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1593.3±35.31µs    10.5 MB/sec    1.00  1524.7±56.35µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.06    178.6±4.64µs    16.5 MB/sec    1.00    168.5±6.04µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.06      3.4±0.05ms     7.5 MB/sec    1.00      3.2±0.09ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     14.6±0.43ms     2.8 MB/sec    1.00     13.9±0.60ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.8±0.09ms     5.9 MB/sec    1.00      2.8±0.30ms     6.0 MB/sec
formatter/numpy/globals.py                 1.02   321.1±16.39µs     9.2 MB/sec    1.00   314.5±18.64µs     9.4 MB/sec
formatter/pydantic/types.py                1.02      6.3±0.31ms     4.0 MB/sec    1.00      6.2±0.24ms     4.1 MB/sec
linter/all-rules/large/dataset.py          1.00     19.4±0.72ms     2.1 MB/sec    1.02     19.7±0.83ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.3±0.25ms     3.1 MB/sec    1.00      5.1±0.17ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   619.1±27.58µs     4.8 MB/sec    1.00   616.5±27.28µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.8±0.29ms     2.9 MB/sec    1.00      8.8±0.27ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.2±0.31ms     4.0 MB/sec    1.02     10.4±0.21ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.08ms     7.8 MB/sec    1.00      2.1±0.09ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    259.1±9.77µs    11.4 MB/sec    1.05   270.9±28.16µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.23ms     5.6 MB/sec    1.01      4.6±0.25ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
