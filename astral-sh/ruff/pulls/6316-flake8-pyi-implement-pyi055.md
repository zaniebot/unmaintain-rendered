```yaml
number: 6316
title: "[`flake8-pyi`] Implement PYI055"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI055
created_at: 2023-08-03T20:07:13Z
updated_at: 2023-08-04T01:49:05Z
url: https://github.com/astral-sh/ruff/pull/6316
synced_at: 2026-01-12T15:55:21Z
```

# [`flake8-pyi`] Implement PYI055

---

_@LaBatata101_

## Summary

Checks for the presence of multiple `type`s in a union. See [original source](https://github.com/PyCQA/flake8-pyi/blob/2a86db8271dc500247a8a69419536240334731cf/pyi.py#L1315).

ref #848 

## Test Plan

Snapshots and manual runs flake8.


---

_Comment by @zanieb on 2023-08-03 20:13_

Let's turn this on for `.py` files too now that we've done https://github.com/astral-sh/ruff/pull/6297

---

_Comment by @LaBatata101 on 2023-08-03 20:15_

> Let's turn this on for `.py` files too now that we've done #6297

Working on it!

---

_Review comment by @LaBatata101 on `crates/ruff/src/checkers/ast/analyze/expression.rs`:103 on 2023-08-03 20:29_

This was forgotten in #6297 I will be fixing in this PR if that's ok

---

_@LaBatata101 reviewed on 2023-08-03 20:29_

---

_Comment by @github-actions[bot] on 2023-08-03 20:34_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -0, 0 error(s))

<details><summary>airflow (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/airflow/providers/google/cloud/hooks/bigquery.py#L1580'>airflow/providers/google/cloud/hooks/bigquery.py:1580:35:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/airflow/providers/google/cloud/hooks/bigquery.py#L1587'>airflow/providers/google/cloud/hooks/bigquery.py:1587:14:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/airflow/providers/imap/hooks/imap.py#L81'>airflow/providers/imap/hooks/imap.py:81:15:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[imaplib.IMAP4_SSL | imaplib.IMAP4]`.
+ <a href='https://github.com/apache/airflow/blob/e3d82c6be0e0e1468ade053c37690aa1e0e4882d/airflow/providers/smtp/hooks/smtp.py#L101'>airflow/providers/smtp/hooks/smtp.py:101:15:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[smtplib.SMTP_SSL | smtplib.SMTP]`.
</pre>

</p>
</details>
<details><summary>bokeh (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/38d05d037223234aedeb25e35502abf3332bf0b4/src/bokeh/core/validation/decorators.py#L67'>src/bokeh/core/validation/decorators.py:67:13:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[Error | Warning]`.
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI055 | 5 | 5 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.08ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1586.6±18.54µs    10.5 MB/sec    1.01   1597.1±4.50µs    10.4 MB/sec
formatter/numpy/globals.py                 1.00    180.1±3.80µs    16.4 MB/sec    1.02    183.5±3.10µs    16.1 MB/sec
formatter/pydantic/types.py                1.00      3.4±0.02ms     7.5 MB/sec    1.01      3.4±0.05ms     7.4 MB/sec
linter/all-rules/large/dataset.py          1.00     10.8±0.16ms     3.8 MB/sec    1.03     11.1±0.12ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.02ms     6.0 MB/sec    1.01      2.8±0.02ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    385.6±2.48µs     7.7 MB/sec    1.00    381.3±1.66µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      4.9±0.02ms     5.2 MB/sec    1.03      5.1±0.07ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.04ms     7.6 MB/sec    1.00      5.3±0.04ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1128.5±4.00µs    14.8 MB/sec    1.00   1119.4±3.57µs    14.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    125.4±0.55µs    23.5 MB/sec    1.00    124.8±0.45µs    23.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.4±0.01ms    10.6 MB/sec    1.00      2.4±0.01ms    10.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.17ms     4.2 MB/sec    1.04     10.1±0.28ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1906.1±40.57µs     8.7 MB/sec    1.02  1942.2±56.29µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00    210.9±7.23µs    14.0 MB/sec    1.04    219.6±9.44µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.08ms     6.1 MB/sec    1.04      4.3±0.15ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.29ms     3.1 MB/sec    1.06     14.1±0.41ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.13ms     4.7 MB/sec    1.02      3.6±0.06ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   434.6±12.09µs     6.8 MB/sec    1.02   444.4±16.60µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.12ms     4.2 MB/sec    1.06      6.4±0.23ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.14ms     6.0 MB/sec    1.04      7.0±0.13ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1402.7±43.39µs    11.9 MB/sec    1.02  1432.2±29.19µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.2±4.85µs    18.4 MB/sec    1.04   166.4±10.12µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.05ms     8.5 MB/sec    1.04      3.1±0.05ms     8.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/analyze/expression.rs`:1113 on 2023-08-04 00:04_

Should we consider moving this check into a shared block like we do for the `Union[...]` case above?

---

_@zanieb reviewed on 2023-08-04 00:04_

---

_@zanieb reviewed on 2023-08-04 00:07_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:68 on 2023-08-04 00:07_

How was using this helper? I wrote it and would appreciate any feedback :)

---

_@zanieb reviewed on 2023-08-04 00:09_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:14 on 2023-08-04 00:09_

Oops guessing this snapshot isn't meant to be committed here

---

_@zanieb reviewed on 2023-08-04 00:12_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:14 on 2023-08-04 00:12_

(Nevermind I see this is for the py file)

---

_@zanieb approved on 2023-08-04 00:12_

LGTM! Thank you :)

---

_@LaBatata101 reviewed on 2023-08-04 01:07_

---

_Review comment by @LaBatata101 on `crates/ruff/src/checkers/ast/analyze/expression.rs`:1113 on 2023-08-04 01:07_

That would be nice, removing the duplicated code. Does the variable name `is_unchecked_union` fit in here as well?

---

_@LaBatata101 reviewed on 2023-08-04 01:09_

---

_Review comment by @LaBatata101 on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_type_union.rs`:68 on 2023-08-04 01:09_

It was very easy to use, good job!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-04 01:11_

---

_Label `rule` added by @charliermarsh on 2023-08-04 01:18_

---

_Merged by @charliermarsh on 2023-08-04 01:36_

---

_Closed by @charliermarsh on 2023-08-04 01:36_

---

_Branch deleted on 2023-08-04 01:47_

---
