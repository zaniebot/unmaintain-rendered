```yaml
number: 7895
title: "Simplify `key in dct and dct[key]` to `dct.get(key)`"
type: pull_request
state: merged
author: harupy
labels:
  - rule
assignees: []
merged: true
base: main
head: unnecessary-key-check
created_at: 2023-10-10T14:02:31Z
updated_at: 2023-10-13T01:18:25Z
url: https://github.com/astral-sh/ruff/pull/7895
synced_at: 2026-01-12T15:55:25Z
```

# Simplify `key in dct and dct[key]` to `dct.get(key)`

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #5933

## Test Plan

<!-- How was it tested? -->

`cargo test`


---

_Comment by @github-actions[bot] on 2023-10-10 14:19_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+6, -6, 0 error(s))

<details><summary>airflow (+4, -4)</summary>
<p>

<pre>
- [*] 16066 fixable with the `--fix` option (6343 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 16069 fixable with the `--fix` option (6343 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1580'>airflow/providers/google/cloud/hooks/bigquery.py:1580:35:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
+ <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1580'>airflow/providers/google/cloud/hooks/bigquery.py:1580:35:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
- <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1587'>airflow/providers/google/cloud/hooks/bigquery.py:1587:14:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
+ <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/google/cloud/hooks/bigquery.py#L1587'>airflow/providers/google/cloud/hooks/bigquery.py:1587:14:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[CopyJob | QueryJob | LoadJob | ExtractJob]`.
- <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/smtp/hooks/smtp.py#L103'>airflow/providers/smtp/hooks/smtp.py:103:15:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[smtplib.SMTP_SSL | smtplib.SMTP]`.
+ <a href='https://github.com/apache/airflow/blob/1f63199351efe9a240c5ae07ce31b79fb0353d64/airflow/providers/smtp/hooks/smtp.py#L103'>airflow/providers/smtp/hooks/smtp.py:103:15:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[smtplib.SMTP_SSL | smtplib.SMTP]`.
</pre>

</p>
</details>
<details><summary>bokeh (+2, -2)</summary>
<p>

<pre>
- [*] 17800 fixable with the `--fix` option (4727 hidden fixes can be enabled with the `--unsafe-fixes` option).
+ [*] 17801 fixable with the `--fix` option (4727 hidden fixes can be enabled with the `--unsafe-fixes` option).
- <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/src/bokeh/core/validation/decorators.py#L67'>src/bokeh/core/validation/decorators.py:67:13:</a> PYI055 Multiple `type` members in a union. Combine them into one, e.g., `type[Error | Warning]`.
+ <a href='https://github.com/bokeh/bokeh/blob/8fa062ac8938a6c064caa2dd50be7013f04fa2f7/src/bokeh/core/validation/decorators.py#L67'>src/bokeh/core/validation/decorators.py:67:13:</a> PYI055 [*] Multiple `type` members in a union. Combine them into one, e.g., `type[Error | Warning]`.
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI055 | 8 | 4 | 4 |



---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_key_check.rs`:43 on 2023-10-12 05:05_

nit: the highlighted range includes the entire boolean test

```suggestion
        format!("Replace with `{dict}.get({key})`")
```

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF019_RUF019.py.snap`:9 on 2023-10-12 05:10_

Future improvements could be to also support other checks like:

```python
if "k" in d and d["k"] == value:
	...
```

which could be replaced with:
```python
if d.get("k") == value:
	...
```

Instead of `==`, there could be `!=`, `in`, `not in`, etc.

---

_@dhruvmanila approved on 2023-10-12 05:12_

---

_Label `rule` added by @dhruvmanila on 2023-10-12 05:13_

---

_Review requested from @charliermarsh by @dhruvmanila on 2023-10-12 05:13_

---

_Merged by @charliermarsh on 2023-10-13 01:08_

---

_Closed by @charliermarsh on 2023-10-13 01:08_

---

_Branch deleted on 2023-10-13 01:15_

---
