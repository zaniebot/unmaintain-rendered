```yaml
number: 9257
title: "[pylint] - implement `super-without-brackets`/`W0245`"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-W0245
created_at: 2023-12-23T07:05:21Z
updated_at: 2024-01-02T22:05:45Z
url: https://github.com/astral-sh/ruff/pull/9257
synced_at: 2026-01-10T23:07:18Z
```

# [pylint] - implement `super-without-brackets`/`W0245`

---

_Pull request opened by @diceroll123 on 2023-12-23 07:05_

## Summary

Implement [`super-without-brackets`/`W0245`](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/super-without-brackets.html)

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-12-23 07:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1653 -1653 violations, +0 -0 fixes in 2 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+42 -42 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1611 -1611 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L42'>src/bokeh/application/application.py:42:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L42'>src/bokeh/application/application.py:42:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L43'>src/bokeh/application/application.py:43:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L43'>src/bokeh/application/application.py:43:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L44'>src/bokeh/application/application.py:44:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L44'>src/bokeh/application/application.py:44:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L50'>src/bokeh/application/application.py:50:5:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/application.py#L50'>src/bokeh/application/application.py:50:5:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L45'>src/bokeh/application/handlers/code.py:45:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L45'>src/bokeh/application/handlers/code.py:45:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L46'>src/bokeh/application/handlers/code.py:46:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L46'>src/bokeh/application/handlers/code.py:46:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L33'>src/bokeh/application/handlers/code_runner.py:33:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L33'>src/bokeh/application/handlers/code_runner.py:33:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L34'>src/bokeh/application/handlers/code_runner.py:34:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L34'>src/bokeh/application/handlers/code_runner.py:34:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L65'>src/bokeh/application/handlers/directory.py:65:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L65'>src/bokeh/application/handlers/directory.py:65:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L66'>src/bokeh/application/handlers/directory.py:66:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L66'>src/bokeh/application/handlers/directory.py:66:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L78'>src/bokeh/application/handlers/directory.py:78:5:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L78'>src/bokeh/application/handlers/directory.py:78:5:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/document_lifecycle.py#L25'>src/bokeh/application/handlers/document_lifecycle.py:25:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/document_lifecycle.py#L25'>src/bokeh/application/handlers/document_lifecycle.py:25:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L44'>src/bokeh/application/handlers/function.py:44:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L44'>src/bokeh/application/handlers/function.py:44:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L45'>src/bokeh/application/handlers/function.py:45:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L45'>src/bokeh/application/handlers/function.py:45:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L52'>src/bokeh/application/handlers/handler.py:52:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L52'>src/bokeh/application/handlers/handler.py:52:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L28'>src/bokeh/application/handlers/lifecycle.py:28:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L28'>src/bokeh/application/handlers/lifecycle.py:28:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Relative imports from parent modules are banned
... 3174 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TID252 | 3306 | 1653 | 1653 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1572 -1572 violations, +0 -0 fixes in 2 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+42 -42 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/apache/airflow/blob/e28627f6a52db0a300d81cca69fa1450b4d5c312/tests/providers/amazon/aws/hooks/test_eks.py#L57'>tests/providers/amazon/aws/hooks/test_eks.py:57:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
... 74 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1530 -1530 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L45'>src/bokeh/application/handlers/code.py:45:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L45'>src/bokeh/application/handlers/code.py:45:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L46'>src/bokeh/application/handlers/code.py:46:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L46'>src/bokeh/application/handlers/code.py:46:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code.py#L47'>src/bokeh/application/handlers/code.py:47:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L33'>src/bokeh/application/handlers/code_runner.py:33:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L33'>src/bokeh/application/handlers/code_runner.py:33:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L34'>src/bokeh/application/handlers/code_runner.py:34:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/code_runner.py#L34'>src/bokeh/application/handlers/code_runner.py:34:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L65'>src/bokeh/application/handlers/directory.py:65:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L65'>src/bokeh/application/handlers/directory.py:65:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L66'>src/bokeh/application/handlers/directory.py:66:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L66'>src/bokeh/application/handlers/directory.py:66:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L67'>src/bokeh/application/handlers/directory.py:67:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L78'>src/bokeh/application/handlers/directory.py:78:5:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/directory.py#L78'>src/bokeh/application/handlers/directory.py:78:5:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/document_lifecycle.py#L25'>src/bokeh/application/handlers/document_lifecycle.py:25:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/document_lifecycle.py#L25'>src/bokeh/application/handlers/document_lifecycle.py:25:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L44'>src/bokeh/application/handlers/function.py:44:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L44'>src/bokeh/application/handlers/function.py:44:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L45'>src/bokeh/application/handlers/function.py:45:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/function.py#L45'>src/bokeh/application/handlers/function.py:45:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L52'>src/bokeh/application/handlers/handler.py:52:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L52'>src/bokeh/application/handlers/handler.py:52:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/handler.py#L53'>src/bokeh/application/handlers/handler.py:53:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L28'>src/bokeh/application/handlers/lifecycle.py:28:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L28'>src/bokeh/application/handlers/lifecycle.py:28:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Relative imports from parent modules are banned
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/lifecycle.py#L29'>src/bokeh/application/handlers/lifecycle.py:29:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/notebook.py#L36'>src/bokeh/application/handlers/notebook.py:36:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/notebook.py#L36'>src/bokeh/application/handlers/notebook.py:36:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/notebook.py#L37'>src/bokeh/application/handlers/notebook.py:37:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/notebook.py#L37'>src/bokeh/application/handlers/notebook.py:37:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/script.py#L52'>src/bokeh/application/handlers/script.py:52:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/script.py#L52'>src/bokeh/application/handlers/script.py:52:1:</a> TID252 Relative imports from parent modules are banned
+ <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/server_lifecycle.py#L29'>src/bokeh/application/handlers/server_lifecycle.py:29:1:</a> TID252 Prefer absolute imports over relative imports from parent modules
- <a href='https://github.com/bokeh/bokeh/blob/929ee8ab38207eca08d1601a0164952fb21c9c6a/src/bokeh/application/handlers/server_lifecycle.py#L29'>src/bokeh/application/handlers/server_lifecycle.py:29:1:</a> TID252 Relative imports from parent modules are banned
... 3012 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TID252 | 3144 | 1572 | 1572 | 0 | 0 |

</p>
</details>




---

_Comment by @diceroll123 on 2023-12-23 07:37_

Is it possible to narrow the guard clauses down more?

I'm unsure what the best way to have the semantic model know if we're within a subclass would be here. Please advise!

---

_@charliermarsh reviewed on 2023-12-23 13:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/super_without_brackets.rs`:62 on 2023-12-23 13:27_

We could probably check if we're in a function immediately within a class?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/super_without_brackets.rs`:62 on 2023-12-23 13:28_

Actually, can you try using `function_type::classify` for this, to detect that we're in a method?

---

_@charliermarsh reviewed on 2023-12-23 13:28_

---

_@charliermarsh reviewed on 2023-12-23 13:29_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/super_without_brackets.rs`:62 on 2023-12-23 13:29_

You will need to pass the parent scope via something like:

```rust
let Some(parent) = &checker.semantic().first_non_type_parent_scope(scope) else {
    return;
};
```

Instead of the current scope.

---

_Label `rule` added by @charliermarsh on 2023-12-23 13:29_

---

_Label `preview` added by @charliermarsh on 2023-12-23 13:29_

---

_@diceroll123 reviewed on 2023-12-23 17:45_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/super_without_brackets.rs`:62 on 2023-12-23 17:45_

Updated! Seems much better now, LMK!

And thanks!

---

_@charliermarsh approved on 2024-01-02 19:44_

Thanks!

---

_Merged by @charliermarsh on 2024-01-02 21:57_

---

_Closed by @charliermarsh on 2024-01-02 21:57_

---
