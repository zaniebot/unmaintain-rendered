```yaml
number: 8841
title: "[`isort`]  Add support for length-sort settings"
type: pull_request
state: merged
author: bluthej
labels:
  - isort
assignees: []
merged: true
base: main
head: length-sort-option
created_at: 2023-11-26T17:47:11Z
updated_at: 2023-11-28T06:06:50Z
url: https://github.com/astral-sh/ruff/pull/8841
synced_at: 2026-01-10T23:40:55Z
```

# [`isort`]  Add support for length-sort settings

---

_Pull request opened by @bluthej on 2023-11-26 17:47_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes #1567.

Add both `length-sort` and `length-sort-straight` settings for isort.

Here are a few notable points:
- The length is determined using the [`unicode_width`](https://crates.io/crates/unicode-width) crate, i.e. we are talking about displayed length (this is explicitly mentioned in the description of the setting)
- The dots are taken into account in the length to be compatible with the original isort
- I had to reorder a few fields of the module key struct for it all to make sense (notably the `force_to_top` field is now the first one)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->

I added tests for the following cases:
- Basic tests for length-sort with ASCII characters only
- Tests with non-ASCII characters
- Tests with relative imports
- Tests for length-sort-straight


---

_Comment by @github-actions[bot] on 2023-11-26 17:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -39 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -12 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L103'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:103:31:</a> ANN001 Missing type annotation for function argument `connection_id`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L103'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:103:46:</a> ANN001 Missing type annotation for function argument `hostname`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L103'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:103:5:</a> ANN201 Missing return type annotation for public function `configure_hive_connection`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L110'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:110:10:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L117'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:117:16:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L52'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:52:27:</a> ANN001 Missing type annotation for function argument `table_name`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L52'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:52:5:</a> ANN201 Missing return type annotation for public function `create_dynamodb_table`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L72'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:72:29:</a> ANN001 Missing type annotation for function argument `table_name`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L72'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:72:5:</a> ANN201 Missing return type annotation for public function `get_dynamodb_item_count`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L86'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:86:5:</a> T201 `print` found
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L93'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:93:27:</a> ANN001 Missing type annotation for function argument `table_name`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L93'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:93:5:</a> ANN201 Missing return type annotation for public function `delete_dynamodb_table`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -27 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L25'>src/bokeh/server/views/metadata_handler.py:25:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L28'>src/bokeh/server/views/metadata_handler.py:28:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L31'>src/bokeh/server/views/metadata_handler.py:31:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L32'>src/bokeh/server/views/metadata_handler.py:32:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L39'>src/bokeh/server/views/metadata_handler.py:39:5:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D200 One-line docstring should fit on one line
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D300 Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D400 First line should end with a period
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:15:</a> ANN201 Missing return type annotation for public function `get`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:15:</a> D102 Missing docstring in public method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:19:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:26:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:26:</a> ARG002 Unused method argument: `args`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:34:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:34:</a> ARG002 Unused method argument: `kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L64'>src/bokeh/server/views/metadata_handler.py:64:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L66'>src/bokeh/server/views/metadata_handler.py:66:41:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D205 1 blank line required between summary line and description
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D300 Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D400 First line should end with a period
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L8'>src/bokeh/server/views/metadata_handler.py:8:1:</a> D208 [*] Docstring is over-indented
</pre>

</p>
</details>
<details><summary>Changes by rule (21 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ANN001 | 5 | 0 | 5 | 0 | 0 |
| ANN201 | 5 | 0 | 5 | 0 | 0 |
| E402 | 4 | 0 | 4 | 0 | 0 |
| Q000 | 2 | 0 | 2 | 0 | 0 |
| D210 | 2 | 0 | 2 | 0 | 0 |
| D300 | 2 | 0 | 2 | 0 | 0 |
| D400 | 2 | 0 | 2 | 0 | 0 |
| D415 | 2 | 0 | 2 | 0 | 0 |
| Q002 | 2 | 0 | 2 | 0 | 0 |
| ARG002 | 2 | 0 | 2 | 0 | 0 |
| COM812 | 1 | 0 | 1 | 0 | 0 |
| DTZ001 | 1 | 0 | 1 | 0 | 0 |
| T201 | 1 | 0 | 1 | 0 | 0 |
| D200 | 1 | 0 | 1 | 0 | 0 |
| D102 | 1 | 0 | 1 | 0 | 0 |
| ANN101 | 1 | 0 | 1 | 0 | 0 |
| ANN002 | 1 | 0 | 1 | 0 | 0 |
| ANN003 | 1 | 0 | 1 | 0 | 0 |
| C408 | 1 | 0 | 1 | 0 | 0 |
| D205 | 1 | 0 | 1 | 0 | 0 |
| D208 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -57 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L103'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:103:31:</a> ANN001 Missing type annotation for function argument `connection_id`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L103'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:103:46:</a> ANN001 Missing type annotation for function argument `hostname`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L103'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:103:5:</a> ANN201 Missing return type annotation for public function `configure_hive_connection`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L110'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:110:10:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L117'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:117:16:</a> DTZ001 The use of `datetime.datetime()` without `tzinfo` argument is not allowed
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L1'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L52'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:52:27:</a> ANN001 Missing type annotation for function argument `table_name`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L52'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:52:5:</a> ANN201 Missing return type annotation for public function `create_dynamodb_table`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L72'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:72:29:</a> ANN001 Missing type annotation for function argument `table_name`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L72'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:72:5:</a> ANN201 Missing return type annotation for public function `get_dynamodb_item_count`
- <a href='https://github.com/apache/airflow/blob/16585b178fab53b7c6d063426105664e55b14bfe/tests/system/providers/amazon/aws/example_hive_to_dynamodb.py#L86'>tests/system/providers/amazon/aws/example_hive_to_dynamodb.py:86:5:</a> T201 `print` found
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -44 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L12'>src/bokeh/server/views/metadata_handler.py:12:1:</a> E265 Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L14'>src/bokeh/server/views/metadata_handler.py:14:1:</a> E265 Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L17'>src/bokeh/server/views/metadata_handler.py:17:15:</a> E261 [*] Insert at least two spaces before an inline comment
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L1'>src/bokeh/server/views/metadata_handler.py:1:1:</a> E265 Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L20'>src/bokeh/server/views/metadata_handler.py:20:1:</a> E265 Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L22'>src/bokeh/server/views/metadata_handler.py:22:1:</a> E265 Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L25'>src/bokeh/server/views/metadata_handler.py:25:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L28'>src/bokeh/server/views/metadata_handler.py:28:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L31'>src/bokeh/server/views/metadata_handler.py:31:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L32'>src/bokeh/server/views/metadata_handler.py:32:1:</a> E402 Module level import not at top of file
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L34'>src/bokeh/server/views/metadata_handler.py:34:1:</a> E265 Block comment should start with `# `
... 11 additional changes omitted for rule E265
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L39'>src/bokeh/server/views/metadata_handler.py:39:5:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D200 One-line docstring should fit on one line
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D400 First line should end with a period
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L51'>src/bokeh/server/views/metadata_handler.py:51:5:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:15:</a> ANN201 Missing return type annotation for public function `get`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:15:</a> D102 Missing docstring in public method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:19:</a> ANN101 Missing type annotation for `self` in method
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:26:</a> ANN002 Missing type annotation for `*args`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:26:</a> ARG002 Unused method argument: `args`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:34:</a> ANN003 Missing type annotation for `**kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L56'>src/bokeh/server/views/metadata_handler.py:56:34:</a> ARG002 Unused method argument: `kwargs`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L64'>src/bokeh/server/views/metadata_handler.py:64:20:</a> C408 Unnecessary `dict` call (rewrite as a literal)
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L66'>src/bokeh/server/views/metadata_handler.py:66:41:</a> Q000 [*] Single quotes found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D205 1 blank line required between summary line and description
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D400 First line should end with a period
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> D415 First line should end with a period, question mark, or exclamation point
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L7'>src/bokeh/server/views/metadata_handler.py:7:1:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/server/views/metadata_handler.py#L8'>src/bokeh/server/views/metadata_handler.py:8:1:</a> D208 [*] Docstring is over-indented
... 10 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (24 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E265 | 16 | 0 | 16 | 0 | 0 |
| ANN001 | 5 | 0 | 5 | 0 | 0 |
| ANN201 | 5 | 0 | 5 | 0 | 0 |
| E402 | 4 | 0 | 4 | 0 | 0 |
| Q000 | 2 | 0 | 2 | 0 | 0 |
| D210 | 2 | 0 | 2 | 0 | 0 |
| D300 | 2 | 0 | 2 | 0 | 0 |
| D400 | 2 | 0 | 2 | 0 | 0 |
| D415 | 2 | 0 | 2 | 0 | 0 |
| Q002 | 2 | 0 | 2 | 0 | 0 |
| ARG002 | 2 | 0 | 2 | 0 | 0 |
| COM812 | 1 | 0 | 1 | 0 | 0 |
| DTZ001 | 1 | 0 | 1 | 0 | 0 |
| CPY001 | 1 | 0 | 1 | 0 | 0 |
| T201 | 1 | 0 | 1 | 0 | 0 |
| E261 | 1 | 0 | 1 | 0 | 0 |
| D200 | 1 | 0 | 1 | 0 | 0 |
| D102 | 1 | 0 | 1 | 0 | 0 |
| ANN101 | 1 | 0 | 1 | 0 | 0 |
| ANN002 | 1 | 0 | 1 | 0 | 0 |
| ANN003 | 1 | 0 | 1 | 0 | 0 |
| C408 | 1 | 0 | 1 | 0 | 0 |
| D205 | 1 | 0 | 1 | 0 | 0 |
| D208 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review requested from @charliermarsh by @charliermarsh on 2023-11-27 23:59_

---

_Label `isort` added by @charliermarsh on 2023-11-27 23:59_

---

_@charliermarsh approved on 2023-11-28 05:50_

Great, thanks!

---

_Renamed from "[isort] Length sort settings" to "[`isort`]  Add support for length-sort settings" by @charliermarsh on 2023-11-28 05:50_

---

_Merged by @charliermarsh on 2023-11-28 06:00_

---

_Closed by @charliermarsh on 2023-11-28 06:00_

---
