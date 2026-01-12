```yaml
number: 5638
title: "Support autofix for some multiline `str.format` calls"
type: pull_request
state: merged
author: harupy
labels:
  - fixes
assignees: []
merged: true
base: main
head: support-multiline-format-call
created_at: 2023-07-10T09:51:20Z
updated_at: 2023-07-10T13:53:32Z
url: https://github.com/astral-sh/ruff/pull/5638
synced_at: 2026-01-12T03:36:55Z
```

# Support autofix for some multiline `str.format` calls

---

_Pull request opened by @harupy on 2023-07-10 09:51_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #5531

## Test Plan

<!-- How was it tested? -->

New test cases

---

_Comment by @github-actions[bot] on 2023-07-10 10:01_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+12, -0, 0 error(s))

<details><summary>airflow (+2, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/main/airflow/models/dag.py#L2231'>airflow/models/dag.py:2231:24:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/apache/airflow/blob/main/airflow/providers/qubole/hooks/qubole.py#L189'>airflow/providers/qubole/hooks/qubole.py:189:17:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
<details><summary>zulip (+10, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/data_import/slack.py#L1482'>zerver/data_import/slack.py:1482:17:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/lib/url_preview/preview.py#L29'>zerver/lib/url_preview/preview.py:29:32:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_message_fetch.py#L4362'>zerver/tests/test_message_fetch.py:4362:16:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_user_groups.py#L1118'>zerver/tests/test_user_groups.py:1118:13:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/tests/test_user_groups.py#L1131'>zerver/tests/test_user_groups.py:1131:13:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/views/development/integrations.py#L138'>zerver/views/development/integrations.py:138:15:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/views/development/integrations.py#L79'>zerver/views/development/integrations.py:79:15:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/webhooks/front/view.py#L63'>zerver/webhooks/front/view.py:63:12:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/webhooks/front/view.py#L81'>zerver/webhooks/front/view.py:81:12:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/main/zerver/webhooks/groove/view.py#L69'>zerver/webhooks/groove/view.py:69:39:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| UP032 | 12 | 12 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.04ms     4.8 MB/sec    1.00      8.4±0.03ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   1804.8±3.89µs     9.2 MB/sec    1.00   1800.8±7.83µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    199.0±0.49µs    14.8 MB/sec    1.00    198.9±1.00µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.01ms     6.2 MB/sec    1.00      4.1±0.02ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.09ms     2.9 MB/sec    1.00     14.1±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.4±2.10µs     8.1 MB/sec    1.01    365.3±1.04µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.2±0.04ms     4.1 MB/sec    1.00      6.2±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1463.5±1.67µs    11.4 MB/sec    1.00   1465.6±6.70µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.2±0.31µs    18.9 MB/sec    1.00    155.8±0.26µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.00      3.2±0.01ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15     11.0±0.44ms     3.7 MB/sec    1.00      9.6±0.16ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.11      2.2±0.08ms     7.4 MB/sec    1.00      2.0±0.03ms     8.2 MB/sec
formatter/numpy/globals.py                 1.05    231.3±7.68µs    12.8 MB/sec    1.00    220.4±4.04µs    13.4 MB/sec
formatter/pydantic/types.py                1.14      5.2±0.15ms     4.9 MB/sec    1.00      4.6±0.06ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.14     18.3±0.28ms     2.2 MB/sec    1.00     16.0±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.20      5.0±0.21ms     3.3 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.15   492.4±16.13µs     6.0 MB/sec    1.00    429.5±5.02µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.17      8.2±0.33ms     3.1 MB/sec    1.00      7.0±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.15      9.3±0.34ms     4.4 MB/sec    1.00      8.1±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.13  1865.3±36.47µs     8.9 MB/sec    1.00  1655.9±22.63µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.06    192.2±3.11µs    15.3 MB/sec    1.00    182.0±2.61µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.14      4.2±0.11ms     6.1 MB/sec    1.00      3.6±0.06ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-10 13:43_

---

_Renamed from "Autofix multiline format call" to "Support autofix for some multiline `str.format` calls" by @charliermarsh on 2023-07-10 13:47_

---

_Label `autofix` added by @charliermarsh on 2023-07-10 13:47_

---

_Merged by @charliermarsh on 2023-07-10 13:49_

---

_Closed by @charliermarsh on 2023-07-10 13:49_

---
