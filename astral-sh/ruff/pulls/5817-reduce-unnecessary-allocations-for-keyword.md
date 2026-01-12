```yaml
number: 5817
title: Reduce unnecessary allocations for keyword detection
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/callargs
created_at: 2023-07-17T02:12:58Z
updated_at: 2023-07-17T02:40:49Z
url: https://github.com/astral-sh/ruff/pull/5817
synced_at: 2026-01-12T03:30:21Z
```

# Reduce unnecessary allocations for keyword detection

---

_Pull request opened by @charliermarsh on 2023-07-17 02:12_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-07-17 02:12_

---

_Label `performance` added by @charliermarsh on 2023-07-17 02:12_

---

_Merged by @charliermarsh on 2023-07-17 02:22_

---

_Closed by @charliermarsh on 2023-07-17 02:22_

---

_Branch deleted on 2023-07-17 02:22_

---

_Comment by @github-actions[bot] on 2023-07-17 02:27_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+6, -6)</summary>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/assign_cherry_picked_prs_with_milestone.py#L161'>dev/assign_cherry_picked_prs_with_milestone.py:161:20:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/assign_cherry_picked_prs_with_milestone.py#L161'>dev/assign_cherry_picked_prs_with_milestone.py:161:9:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/prepare_bulk_issues.py#L101'>dev/prepare_bulk_issues.py:101:20:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/prepare_bulk_issues.py#L101'>dev/prepare_bulk_issues.py:101:9:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/prepare_bulk_issues.py#L74'>dev/prepare_bulk_issues.py:74:20:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/prepare_bulk_issues.py#L74'>dev/prepare_bulk_issues.py:74:9:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/prepare_release_issue.py#L195'>dev/prepare_release_issue.py:195:20:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/prepare_release_issue.py#L195'>dev/prepare_release_issue.py:195:9:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/provider_packages/prepare_provider_packages.py#L426'>dev/provider_packages/prepare_provider_packages.py:426:20:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/dev/provider_packages/prepare_provider_packages.py#L426'>dev/provider_packages/prepare_provider_packages.py:426:9:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
- <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py#L120'>scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:120:20:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
+ <a href='https://github.com/apache/airflow/blob/09c5114f67f152c44b555ee408d76b26cfe0691f/scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py#L120'>scripts/ci/pre_commit/pre_commit_check_pre_commit_hooks.py:120:9:</a> S701 Using jinja2 templates with `autoescape=False` is dangerous and can lead to XSS. Ensure `autoescape=True` or use the `select_autoescape` function.
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| S701 | 12 | 6 | 6 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.02ms     4.2 MB/sec    1.01      9.8±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1888.3±3.03µs     8.8 MB/sec    1.00   1896.9±3.20µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    205.1±0.37µs    14.4 MB/sec    1.00    206.0±0.29µs    14.3 MB/sec
formatter/pydantic/types.py                1.01      4.2±0.01ms     6.0 MB/sec    1.00      4.2±0.03ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.10ms     2.9 MB/sec    1.00     13.9±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.00ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    374.7±0.75µs     7.9 MB/sec    1.00    374.7±1.32µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.01      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.02      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1405.7±4.68µs    11.8 MB/sec    1.02   1430.8±2.03µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    147.0±0.38µs    20.1 MB/sec    1.02    149.3±0.73µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.02      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.7±0.48ms     3.2 MB/sec    1.13     14.3±0.51ms     2.8 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.17ms     6.6 MB/sec    1.11      2.8±0.14ms     6.0 MB/sec
formatter/numpy/globals.py                 1.00   287.3±22.19µs    10.3 MB/sec    1.09   313.8±21.44µs     9.4 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.27ms     4.6 MB/sec    1.10      6.1±0.24ms     4.2 MB/sec
linter/all-rules/large/dataset.py          1.01     18.2±0.42ms     2.2 MB/sec    1.00     18.0±0.52ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.17ms     3.5 MB/sec    1.01      4.8±0.22ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   584.3±31.55µs     5.0 MB/sec    1.00   587.1±35.60µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.2±0.31ms     3.1 MB/sec    1.00      8.2±0.29ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.2±0.31ms     4.4 MB/sec    1.01      9.3±0.38ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1926.3±75.42µs     8.6 MB/sec    1.01  1950.8±107.52µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   227.3±19.41µs    13.0 MB/sec    1.00   226.1±11.45µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.2±0.19ms     6.1 MB/sec    1.00      4.1±0.14ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
