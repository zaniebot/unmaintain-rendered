```yaml
number: 5669
title: "[`flake8-bugbear`] Implement `re-sub-positional-args` (`B034`)"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/B034
created_at: 2023-07-11T02:44:07Z
updated_at: 2023-07-11T04:09:35Z
url: https://github.com/astral-sh/ruff/pull/5669
synced_at: 2026-01-12T15:55:19Z
```

# [`flake8-bugbear`] Implement `re-sub-positional-args` (`B034`)

---

_@charliermarsh_

## Summary

Needed to do some coding to end the day.

Closes #5665.

---

_Label `rule` added by @charliermarsh on 2023-07-11 02:44_

---

_Comment by @github-actions[bot] on 2023-07-11 02:56_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+14, -0, 0 error(s))

<details><summary>airflow (+7, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/amazon/aws/hooks/s3.py#L1020'>airflow/providers/amazon/aws/hooks/s3.py:1020:18:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/amazon/aws/hooks/s3.py#L356'>airflow/providers/amazon/aws/hooks/s3.py:356:24:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/amazon/aws/hooks/s3.py#L473'>airflow/providers/amazon/aws/hooks/s3.py:473:18:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/amazon/aws/hooks/s3.py#L550'>airflow/providers/amazon/aws/hooks/s3.py:550:24:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/amazon/aws/hooks/s3.py#L575'>airflow/providers/amazon/aws/hooks/s3.py:575:26:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/amazon/aws/sensors/s3.py#L116'>airflow/providers/amazon/aws/sensors/s3.py:116:22:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/apache/airflow/blob/main/dev/provider_packages/prepare_provider_packages.py#L1270'>dev/provider_packages/prepare_provider_packages.py:1270:16:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
</pre>

</p>
</details>
<details><summary>pip (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/main/tests/lib/__init__.py#L1190'>tests/lib/__init__.py:1190:12:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
</pre>

</p>
</details>
<details><summary>disnake (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/utils.py#L821'>disnake/utils.py:821:12:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/DisnakeDev/disnake/blob/master/disnake/utils.py#L860'>disnake/utils.py:860:16:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
</pre>

</p>
</details>
<details><summary>zulip (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/email_mirror.py#L314'>zerver/lib/email_mirror.py:314:12:</a> B034 `re.split` should pass `maxsplit` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/widget.py#L36'>zerver/lib/widget.py:36:22:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/migrations/0041_create_attachments_for_old_messages.py#L14'>zerver/migrations/0041_create_attachments_for_old_messages.py:14:12:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/webhooks/gitlab/view.py#L92'>zerver/webhooks/gitlab/view.py:92:35:</a> B034 `re.sub` should pass `count` and `flags` as keyword arguments to avoid confusion due to unintuitive argument positions
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B034 | 14 | 14 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.4±0.19ms     4.8 MB/sec    1.02      8.6±0.02ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1840.1±3.45µs     9.0 MB/sec    1.01   1863.8±6.07µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    199.8±0.86µs    14.8 MB/sec    1.02    204.0±8.48µs    14.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.01      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.05ms     2.9 MB/sec    1.00     14.3±0.07ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.00ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    367.5±1.17µs     8.0 MB/sec    1.00    368.8±1.66µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.06ms     4.1 MB/sec    1.00      6.3±0.02ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.03ms     5.6 MB/sec    1.01      7.3±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1487.8±7.33µs    11.2 MB/sec    1.01   1505.8±2.35µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.8±0.38µs    18.5 MB/sec    1.02    163.7±5.84µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.01      3.2±0.01ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.6±0.49ms     3.5 MB/sec    1.00     11.6±0.43ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.12ms     6.5 MB/sec    1.00      2.6±0.14ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   298.4±28.91µs     9.9 MB/sec    1.00   297.1±20.63µs     9.9 MB/sec
formatter/pydantic/types.py                1.03      5.8±0.42ms     4.4 MB/sec    1.00      5.6±0.24ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     19.9±0.85ms     2.0 MB/sec    1.00     19.9±0.76ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.24ms     3.2 MB/sec    1.02      5.3±0.36ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   626.0±32.64µs     4.7 MB/sec    1.02   638.2±46.13µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.32ms     2.9 MB/sec    1.04      9.0±0.52ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.47ms     4.0 MB/sec    1.01     10.1±0.31ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.01      2.2±0.09ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   258.0±31.46µs    11.4 MB/sec    1.00   258.1±27.54µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.23ms     5.6 MB/sec    1.00      4.5±0.23ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @zanieb by @charliermarsh on 2023-07-11 02:57_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-07-11 02:57_

---

_@zanieb reviewed on 2023-07-11 03:31_

---

_Review comment by @zanieb on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_311__pep_654.py.snap`:1 on 2023-07-11 03:31_

Why deleted?

---

_@zanieb approved on 2023-07-11 03:35_

Love the implementation!

---

_@charliermarsh reviewed on 2023-07-11 03:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@py_311__pep_654.py.snap`:1 on 2023-07-11 03:44_

Hmm strange -- no idea! I will restore. Wonder what's going on here, maybe an instability in the formatting?

---

_Merged by @charliermarsh on 2023-07-11 03:52_

---

_Closed by @charliermarsh on 2023-07-11 03:52_

---

_Branch deleted on 2023-07-11 03:52_

---
