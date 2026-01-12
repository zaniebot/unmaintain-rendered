```yaml
number: 13640
title: "[`flake8-todos`] Allow words starting with todo"
type: pull_request
state: merged
author: autinerd
labels:
  - rule
assignees: []
merged: true
base: main
head: todo-words
created_at: 2024-10-05T09:51:08Z
updated_at: 2024-10-14T10:31:05Z
url: https://github.com/astral-sh/ruff/pull/13640
synced_at: 2026-01-12T15:55:45Z
```

# [`flake8-todos`] Allow words starting with todo

---

_@autinerd_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Allow words which start with "todo" or other todo-directives
Fixes #13638

## Test Plan

Updated snapshots


---

_Comment by @github-actions[bot] on 2024-10-05 10:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1693 violations, +0 -0 fixes in 5 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -81 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L114'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:114:6:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L122'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:122:6:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L138'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:138:16:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L143'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:143:107:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L152'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:152:78:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L162'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:162:83:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L168'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:168:82:</a> COM812 [*] Trailing comma missing
... 10 additional changes omitted for rule COM812
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/tests/www/views/test_views_dataset.py#L103'>tests/www/views/test_views_dataset.py:103:40:</a> PLR2004 Magic value used in comparison, consider replacing `400` with a constant variable
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/tests/www/views/test_views_dataset.py#L108'>tests/www/views/test_views_dataset.py:108:40:</a> PLR2004 Magic value used in comparison, consider replacing `400` with a constant variable
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/tests/www/views/test_views_dataset.py#L111'>tests/www/views/test_views_dataset.py:111:40:</a> ANN001 Missing type annotation for function argument `admin_client`
... 71 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -246 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1004'>superset/config.py:1004:21:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1015'>superset/config.py:1015:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1016'>superset/config.py:1016:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1017'>superset/config.py:1017:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1018'>superset/config.py:1018:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1026'>superset/config.py:1026:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L105'>superset/config.py:105:1:</a> ERA001 Found commented-out code
... 167 additional changes omitted for rule ERA001
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1068'>superset/config.py:1068:89:</a> E501 Line too long (90 > 88)
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1089'>superset/config.py:1089:64:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1139'>superset/config.py:1139:5:</a> D103 Missing docstring in public function
... 236 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -133 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L116'>src/bokeh/colors/color.py:116:9:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L116'>src/bokeh/colors/color.py:116:9:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L116'>src/bokeh/colors/color.py:116:9:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L133'>src/bokeh/colors/color.py:133:9:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L133'>src/bokeh/colors/color.py:133:9:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L133'>src/bokeh/colors/color.py:133:9:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L148'>src/bokeh/colors/color.py:148:9:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L148'>src/bokeh/colors/color.py:148:9:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L148'>src/bokeh/colors/color.py:148:9:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L161'>src/bokeh/colors/color.py:161:12:</a> E741 Ambiguous variable name: `l`
... 123 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1233 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1000'>corporate/lib/stripe.py:1000:57:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1005'>corporate/lib/stripe.py:1005:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1009'>corporate/lib/stripe.py:1009:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1013'>corporate/lib/stripe.py:1013:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1017'>corporate/lib/stripe.py:1017:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1021'>corporate/lib/stripe.py:1021:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1037'>corporate/lib/stripe.py:1037:9:</a> D102 Missing docstring in public method
... 120 additional changes omitted for rule D102
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1038'>corporate/lib/stripe.py:1038:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1043'>corporate/lib/stripe.py:1043:75:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1046'>corporate/lib/stripe.py:1046:101:</a> E501 Line too long (116 > 100)
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1057'>corporate/lib/stripe.py:1057:75:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1063'>corporate/lib/stripe.py:1063:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1068'>corporate/lib/stripe.py:1068:64:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1074'>corporate/lib/stripe.py:1074:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1099'>corporate/lib/stripe.py:1099:16:</a> RET504 Unnecessary assignment to `customer` before `return` statement
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1103'>corporate/lib/stripe.py:1103:61:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1103'>corporate/lib/stripe.py:1103:61:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1103'>corporate/lib/stripe.py:1103:87:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1106'>corporate/lib/stripe.py:1106:92:</a> COM812 [*] Trailing comma missing
... 197 additional changes omitted for rule COM812
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L111'>corporate/lib/stripe.py:111:5:</a> D103 Missing docstring in public function
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1128'>corporate/lib/stripe.py:1128:101:</a> E501 Line too long (104 > 100)
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1130'>corporate/lib/stripe.py:1130:13:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1137'>corporate/lib/stripe.py:1137:86:</a> FBT003 Boolean positional value in function call
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1144'>corporate/lib/stripe.py:1144:9:</a> D205 1 blank line required between summary line and description
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1144'>corporate/lib/stripe.py:1144:9:</a> D212 [*] Multi-line docstring summary should start at the first line
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1145'>corporate/lib/stripe.py:1145:101:</a> E501 Line too long (101 > 100)
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L114'>corporate/lib/stripe.py:114:5:</a> SIM108 Use ternary operator `precision = 0 if cents % 100 == 0 else 2` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1150'>corporate/lib/stripe.py:1150:9:</a> PT018 Assertion should be broken down into multiple parts
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1150'>corporate/lib/stripe.py:1150:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1157'>corporate/lib/stripe.py:1157:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1158'>corporate/lib/stripe.py:1158:17:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1163'>corporate/lib/stripe.py:1163:13:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1164'>corporate/lib/stripe.py:1164:13:</a> S101 Use of `assert` detected
... 207 additional changes omitted for rule S101
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1206'>corporate/lib/stripe.py:1206:17:</a> B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1206'>corporate/lib/stripe.py:1206:23:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1206'>corporate/lib/stripe.py:1206:39:</a> EM101 Exception must not use a string literal, assign to variable first
... 1197 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (89 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| COM812 | 228 | 0 | 228 | 0 | 0 |
| S101 | 212 | 0 | 212 | 0 | 0 |
| ERA001 | 173 | 0 | 173 | 0 | 0 |
| D102 | 126 | 0 | 126 | 0 | 0 |
| E501 | 90 | 0 | 90 | 0 | 0 |
| D103 | 80 | 0 | 80 | 0 | 0 |
| D101 | 61 | 0 | 61 | 0 | 0 |
| FBT001 | 44 | 0 | 44 | 0 | 0 |
| TRY003 | 43 | 0 | 43 | 0 | 0 |
| D300 | 32 | 0 | 32 | 0 | 0 |
| PLR2004 | 31 | 0 | 31 | 0 | 0 |
| D210 | 31 | 0 | 31 | 0 | 0 |
| ANN001 | 29 | 0 | 29 | 0 | 0 |
| Q002 | 29 | 0 | 29 | 0 | 0 |
| D107 | 28 | 0 | 28 | 0 | 0 |
| FBT002 | 27 | 0 | 27 | 0 | 0 |
| EM101 | 27 | 0 | 27 | 0 | 0 |
| EM102 | 19 | 0 | 19 | 0 | 0 |
| RET505 | 18 | 0 | 18 | 0 | 0 |
| C901 | 18 | 0 | 18 | 0 | 0 |
| FIX002 | 17 | 0 | 17 | 0 | 0 |
| TD002 | 17 | 0 | 17 | 0 | 0 |
| TD003 | 17 | 0 | 17 | 0 | 0 |
| D205 | 15 | 0 | 15 | 0 | 0 |
| D212 | 14 | 0 | 14 | 0 | 0 |
| RET504 | 13 | 0 | 13 | 0 | 0 |
| PLR0912 | 13 | 0 | 13 | 0 | 0 |
| ANN201 | 11 | 0 | 11 | 0 | 0 |
| N806 | 11 | 0 | 11 | 0 | 0 |
| ARG001 | 10 | 0 | 10 | 0 | 0 |
| D400 | 10 | 0 | 10 | 0 | 0 |
| D415 | 10 | 0 | 10 | 0 | 0 |
| B904 | 10 | 0 | 10 | 0 | 0 |
| PLR0913 | 9 | 0 | 9 | 0 | 0 |
| PLR0915 | 9 | 0 | 9 | 0 | 0 |
| FIX004 | 9 | 0 | 9 | 0 | 0 |
| RUF012 | 8 | 0 | 8 | 0 | 0 |
| E402 | 7 | 0 | 7 | 0 | 0 |
| SIM103 | 6 | 0 | 6 | 0 | 0 |
| FBT003 | 6 | 0 | 6 | 0 | 0 |
| PT018 | 6 | 0 | 6 | 0 | 0 |
| A001 | 6 | 0 | 6 | 0 | 0 |
| PTH118 | 5 | 0 | 5 | 0 | 0 |
| S105 | 5 | 0 | 5 | 0 | 0 |
| TCH002 | 5 | 0 | 5 | 0 | 0 |
| D200 | 5 | 0 | 5 | 0 | 0 |
| PLR0911 | 5 | 0 | 5 | 0 | 0 |
| B008 | 5 | 0 | 5 | 0 | 0 |
| D401 | 5 | 0 | 5 | 0 | 0 |
| D202 | 5 | 0 | 5 | 0 | 0 |
| BLE001 | 4 | 0 | 4 | 0 | 0 |
| TID252 | 4 | 0 | 4 | 0 | 0 |
| SIM114 | 4 | 0 | 4 | 0 | 0 |
| N802 | 3 | 0 | 3 | 0 | 0 |
| ANN401 | 3 | 0 | 3 | 0 | 0 |
| TCH001 | 3 | 0 | 3 | 0 | 0 |
| E741 | 3 | 0 | 3 | 0 | 0 |
| SIM108 | 3 | 0 | 3 | 0 | 0 |
| RET506 | 3 | 0 | 3 | 0 | 0 |
| D209 | 3 | 0 | 3 | 0 | 0 |
| B007 | 3 | 0 | 3 | 0 | 0 |
| PLW0603 | 3 | 0 | 3 | 0 | 0 |
| PT006 | 2 | 0 | 2 | 0 | 0 |
| PTH123 | 2 | 0 | 2 | 0 | 0 |
| Q000 | 2 | 0 | 2 | 0 | 0 |
| ARG002 | 2 | 0 | 2 | 0 | 0 |
| TRY301 | 2 | 0 | 2 | 0 | 0 |
| D100 | 2 | 0 | 2 | 0 | 0 |
| TRY300 | 2 | 0 | 2 | 0 | 0 |
| DTZ001 | 1 | 0 | 1 | 0 | 0 |
| ANN202 | 1 | 0 | 1 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |
| TCH003 | 1 | 0 | 1 | 0 | 0 |
| UP035 | 1 | 0 | 1 | 0 | 0 |
| ARG005 | 1 | 0 | 1 | 0 | 0 |
| PTH111 | 1 | 0 | 1 | 0 | 0 |
| D105 | 1 | 0 | 1 | 0 | 0 |
| TRY201 | 1 | 0 | 1 | 0 | 0 |
| SLF001 | 1 | 0 | 1 | 0 | 0 |
| RUF001 | 1 | 0 | 1 | 0 | 0 |
| PT017 | 1 | 0 | 1 | 0 | 0 |
| TRY400 | 1 | 0 | 1 | 0 | 0 |
| TD004 | 1 | 0 | 1 | 0 | 0 |
| PLR5501 | 1 | 0 | 1 | 0 | 0 |
| D413 | 1 | 0 | 1 | 0 | 0 |
| PLW2901 | 1 | 0 | 1 | 0 | 0 |
| D104 | 1 | 0 | 1 | 0 | 0 |
| D404 | 1 | 0 | 1 | 0 | 0 |
| RET508 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1857 violations, +0 -0 fixes in 4 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -95 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L114'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:114:6:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L122'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:122:6:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L138'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:138:16:</a> DTZ001 `datetime.datetime()` called without a `tzinfo` argument
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L143'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:143:107:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L152'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:152:78:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L162'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:162:83:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L168'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:168:82:</a> COM812 [*] Trailing comma missing
... 10 additional changes omitted for rule COM812
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py#L1'>providers/tests/system/google/cloud/cloud_sql/example_cloud_sql.py:1:1:</a> CPY001 Missing copyright notice at top of file
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/tests/www/views/test_views_dataset.py#L103'>tests/www/views/test_views_dataset.py:103:40:</a> PLR2004 Magic value used in comparison, consider replacing `400` with a constant variable
- <a href='https://github.com/apache/airflow/blob/c5e4a74cfc9c828f845cbe83239464669efa90d5/tests/www/views/test_views_dataset.py#L108'>tests/www/views/test_views_dataset.py:108:40:</a> PLR2004 Magic value used in comparison, consider replacing `400` with a constant variable
... 85 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -250 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1004'>superset/config.py:1004:21:</a> RUF012 Mutable class attributes should be annotated with `typing.ClassVar`
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1015'>superset/config.py:1015:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1016'>superset/config.py:1016:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1017'>superset/config.py:1017:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1018'>superset/config.py:1018:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1026'>superset/config.py:1026:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L105'>superset/config.py:105:1:</a> ERA001 Found commented-out code
... 167 additional changes omitted for rule ERA001
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1068'>superset/config.py:1068:89:</a> E501 Line too long (90 > 88)
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1089'>superset/config.py:1089:64:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/apache/superset/blob/0e9c0f621ac9ddbcf889045f3d4772b1ee213e8d/superset/config.py#L1139'>superset/config.py:1139:5:</a> D103 Missing docstring in public function
... 240 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -180 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L116'>src/bokeh/colors/color.py:116:9:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L116'>src/bokeh/colors/color.py:116:9:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L116'>src/bokeh/colors/color.py:116:9:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L11'>src/bokeh/colors/color.py:11:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L133'>src/bokeh/colors/color.py:133:9:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L133'>src/bokeh/colors/color.py:133:9:</a> D300 [*] Use triple double quotes `"""`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L133'>src/bokeh/colors/color.py:133:9:</a> Q002 [*] Single quote docstring found but double quotes preferred
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L13'>src/bokeh/colors/color.py:13:1:</a> E265 [*] Block comment should start with `# `
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L148'>src/bokeh/colors/color.py:148:9:</a> D210 [*] No whitespaces allowed surrounding docstring text
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/color.py#L148'>src/bokeh/colors/color.py:148:9:</a> D300 [*] Use triple double quotes `"""`
... 170 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1332 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1000'>corporate/lib/stripe.py:1000:57:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1005'>corporate/lib/stripe.py:1005:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1009'>corporate/lib/stripe.py:1009:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1013'>corporate/lib/stripe.py:1013:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1017'>corporate/lib/stripe.py:1017:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1021'>corporate/lib/stripe.py:1021:9:</a> D102 Missing docstring in public method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1037'>corporate/lib/stripe.py:1037:9:</a> D102 Missing docstring in public method
... 120 additional changes omitted for rule D102
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1038'>corporate/lib/stripe.py:1038:9:</a> SIM103 Return the condition directly
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1042'>corporate/lib/stripe.py:1042:9:</a> PLR6301 Method `get_remote_server_legacy_plan` could be a function, class method, or static method
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1043'>corporate/lib/stripe.py:1043:75:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1046'>corporate/lib/stripe.py:1046:101:</a> E501 Line too long (116 > 100)
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1057'>corporate/lib/stripe.py:1057:75:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1063'>corporate/lib/stripe.py:1063:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1068'>corporate/lib/stripe.py:1068:64:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1074'>corporate/lib/stripe.py:1074:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1099'>corporate/lib/stripe.py:1099:16:</a> RET504 Unnecessary assignment to `customer` before `return` statement
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1103'>corporate/lib/stripe.py:1103:61:</a> FBT001 Boolean-typed positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1103'>corporate/lib/stripe.py:1103:61:</a> FBT002 Boolean default positional argument in function definition
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1103'>corporate/lib/stripe.py:1103:87:</a> COM812 [*] Trailing comma missing
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1106'>corporate/lib/stripe.py:1106:92:</a> COM812 [*] Trailing comma missing
... 197 additional changes omitted for rule COM812
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L111'>corporate/lib/stripe.py:111:5:</a> D103 Missing docstring in public function
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1128'>corporate/lib/stripe.py:1128:101:</a> E501 Line too long (104 > 100)
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1130'>corporate/lib/stripe.py:1130:13:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1137'>corporate/lib/stripe.py:1137:86:</a> FBT003 Boolean positional value in function call
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1144'>corporate/lib/stripe.py:1144:9:</a> D205 1 blank line required between summary line and description
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1144'>corporate/lib/stripe.py:1144:9:</a> D212 [*] Multi-line docstring summary should start at the first line
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1145'>corporate/lib/stripe.py:1145:101:</a> E501 Line too long (101 > 100)
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L114'>corporate/lib/stripe.py:114:5:</a> SIM108 Use ternary operator `precision = 0 if cents % 100 == 0 else 2` instead of `if`-`else`-block
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1150'>corporate/lib/stripe.py:1150:9:</a> PT018 Assertion should be broken down into multiple parts
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1150'>corporate/lib/stripe.py:1150:9:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1157'>corporate/lib/stripe.py:1157:19:</a> DOC501 Raised exception `BillingError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1157'>corporate/lib/stripe.py:1157:19:</a> TRY003 Avoid specifying long messages outside the exception class
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1158'>corporate/lib/stripe.py:1158:17:</a> EM101 Exception must not use a string literal, assign to variable first
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1163'>corporate/lib/stripe.py:1163:13:</a> S101 Use of `assert` detected
- <a href='https://github.com/zulip/zulip/blob/6bc8651d22310e753e3676df5f897c7e7cd8604a/corporate/lib/stripe.py#L1164'>corporate/lib/stripe.py:1164:13:</a> S101 Use of `assert` detected
... 207 additional changes omitted for rule S101
... 1297 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (112 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| COM812 | 228 | 0 | 228 | 0 | 0 |
| S101 | 212 | 0 | 212 | 0 | 0 |
| ERA001 | 173 | 0 | 173 | 0 | 0 |
| D102 | 126 | 0 | 126 | 0 | 0 |
| E501 | 90 | 0 | 90 | 0 | 0 |
| D103 | 80 | 0 | 80 | 0 | 0 |
| D101 | 61 | 0 | 61 | 0 | 0 |
| FBT001 | 44 | 0 | 44 | 0 | 0 |
| TRY003 | 43 | 0 | 43 | 0 | 0 |
| D300 | 32 | 0 | 32 | 0 | 0 |
| PLR2004 | 31 | 0 | 31 | 0 | 0 |
| D210 | 31 | 0 | 31 | 0 | 0 |
| ANN001 | 29 | 0 | 29 | 0 | 0 |
| Q002 | 29 | 0 | 29 | 0 | 0 |
| D107 | 28 | 0 | 28 | 0 | 0 |
| FBT002 | 27 | 0 | 27 | 0 | 0 |
| EM101 | 27 | 0 | 27 | 0 | 0 |
| PLR6301 | 26 | 0 | 26 | 0 | 0 |
| E226 | 24 | 0 | 24 | 0 | 0 |
| EM102 | 19 | 0 | 19 | 0 | 0 |
| PLR6201 | 19 | 0 | 19 | 0 | 0 |
| RET505 | 18 | 0 | 18 | 0 | 0 |
| C901 | 18 | 0 | 18 | 0 | 0 |
| FIX002 | 17 | 0 | 17 | 0 | 0 |
| TD002 | 17 | 0 | 17 | 0 | 0 |
| TD003 | 17 | 0 | 17 | 0 | 0 |
| E265 | 16 | 0 | 16 | 0 | 0 |
| D205 | 15 | 0 | 15 | 0 | 0 |
| D212 | 14 | 0 | 14 | 0 | 0 |
| RET504 | 13 | 0 | 13 | 0 | 0 |
| PLR0912 | 13 | 0 | 13 | 0 | 0 |
| DOC201 | 12 | 0 | 12 | 0 | 0 |
| ANN201 | 11 | 0 | 11 | 0 | 0 |
| N806 | 11 | 0 | 11 | 0 | 0 |
| ARG001 | 10 | 0 | 10 | 0 | 0 |
| D400 | 10 | 0 | 10 | 0 | 0 |
| D415 | 10 | 0 | 10 | 0 | 0 |
| B904 | 10 | 0 | 10 | 0 | 0 |
| PLR0914 | 10 | 0 | 10 | 0 | 0 |
| PLR0913 | 9 | 0 | 9 | 0 | 0 |
| PLR0917 | 9 | 0 | 9 | 0 | 0 |
| PLR0915 | 9 | 0 | 9 | 0 | 0 |
| FIX004 | 9 | 0 | 9 | 0 | 0 |
| PLC1901 | 9 | 0 | 9 | 0 | 0 |
| RUF012 | 8 | 0 | 8 | 0 | 0 |
| E402 | 7 | 0 | 7 | 0 | 0 |
| CPY001 | 6 | 0 | 6 | 0 | 0 |
| SIM103 | 6 | 0 | 6 | 0 | 0 |
| FBT003 | 6 | 0 | 6 | 0 | 0 |
| PT018 | 6 | 0 | 6 | 0 | 0 |
| F841 | 6 | 0 | 6 | 0 | 0 |
| A001 | 6 | 0 | 6 | 0 | 0 |
| PTH118 | 5 | 0 | 5 | 0 | 0 |
| S105 | 5 | 0 | 5 | 0 | 0 |
| TCH002 | 5 | 0 | 5 | 0 | 0 |
| D200 | 5 | 0 | 5 | 0 | 0 |
| PLR0911 | 5 | 0 | 5 | 0 | 0 |
| PLC0415 | 5 | 0 | 5 | 0 | 0 |
| B008 | 5 | 0 | 5 | 0 | 0 |
| PLR0904 | 5 | 0 | 5 | 0 | 0 |
| D401 | 5 | 0 | 5 | 0 | 0 |
| D202 | 5 | 0 | 5 | 0 | 0 |
| BLE001 | 4 | 0 | 4 | 0 | 0 |
| DOC501 | 4 | 0 | 4 | 0 | 0 |
| TID252 | 4 | 0 | 4 | 0 | 0 |
| SIM114 | 4 | 0 | 4 | 0 | 0 |
| N802 | 3 | 0 | 3 | 0 | 0 |
| ANN401 | 3 | 0 | 3 | 0 | 0 |
| TCH001 | 3 | 0 | 3 | 0 | 0 |
| E741 | 3 | 0 | 3 | 0 | 0 |
| E302 | 3 | 0 | 3 | 0 | 0 |
| SIM108 | 3 | 0 | 3 | 0 | 0 |
| RET506 | 3 | 0 | 3 | 0 | 0 |
| D209 | 3 | 0 | 3 | 0 | 0 |
| B007 | 3 | 0 | 3 | 0 | 0 |
| PLW0603 | 3 | 0 | 3 | 0 | 0 |
| PT006 | 2 | 0 | 2 | 0 | 0 |
| PLW1514 | 2 | 0 | 2 | 0 | 0 |
| PTH123 | 2 | 0 | 2 | 0 | 0 |
| Q000 | 2 | 0 | 2 | 0 | 0 |
| ARG002 | 2 | 0 | 2 | 0 | 0 |
| TRY301 | 2 | 0 | 2 | 0 | 0 |
| D100 | 2 | 0 | 2 | 0 | 0 |
| TRY300 | 2 | 0 | 2 | 0 | 0 |
| DTZ001 | 1 | 0 | 1 | 0 | 0 |
| ANN202 | 1 | 0 | 1 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |
| TCH003 | 1 | 0 | 1 | 0 | 0 |
| UP035 | 1 | 0 | 1 | 0 | 0 |
| PLC2701 | 1 | 0 | 1 | 0 | 0 |
| ARG005 | 1 | 0 | 1 | 0 | 0 |
| PTH111 | 1 | 0 | 1 | 0 | 0 |
| E261 | 1 | 0 | 1 | 0 | 0 |
| FURB180 | 1 | 0 | 1 | 0 | 0 |
| D105 | 1 | 0 | 1 | 0 | 0 |
| TRY201 | 1 | 0 | 1 | 0 | 0 |
| SLF001 | 1 | 0 | 1 | 0 | 0 |
| RUF001 | 1 | 0 | 1 | 0 | 0 |
| PT017 | 1 | 0 | 1 | 0 | 0 |
| TRY400 | 1 | 0 | 1 | 0 | 0 |
| TD004 | 1 | 0 | 1 | 0 | 0 |
| PLR5501 | 1 | 0 | 1 | 0 | 0 |
| D413 | 1 | 0 | 1 | 0 | 0 |
| PLW2901 | 1 | 0 | 1 | 0 | 0 |
| FURB145 | 1 | 0 | 1 | 0 | 0 |
| S405 | 1 | 0 | 1 | 0 | 0 |
| PLR0916 | 1 | 0 | 1 | 0 | 0 |
| D104 | 1 | 0 | 1 | 0 | 0 |
| D404 | 1 | 0 | 1 | 0 | 0 |
| PLR1702 | 1 | 0 | 1 | 0 | 0 |
| RET508 | 1 | 0 | 1 | 0 | 0 |
| FURB118 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @zanieb on 2024-10-05 15:33_

The ecosystem changes have a mix of new false negatives:

https://github.com/zulip/zulip/blob/4b4b6c5ebe0880c7e8f79c659d23ebdce0dab309/zerver/actions/create_realm.py#L239

and fixed false positives:

https://github.com/zulip/zulip/blob/4b4b6c5ebe0880c7e8f79c659d23ebdce0dab309/zerver/views/realm_logo.py#L68

---

_Comment by @autinerd on 2024-10-05 19:18_

Hmm, the question is if "Hacky" is used as a tag, or just being a word. In that regard `# Hackers could compromise it` would be a todo-tag as well.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/directives.rs`:344 on 2024-10-07 09:09_

Nit: I would change the `from_str` implementation to do a direct match on the string (instead of selecting the substring) and instead split by word boundaries here:

https://github.com/astral-sh/ruff/blob/8f409285347fbed17c0764759635eac6b6935b13/crates/ruff_linter/src/directives.rs#L302

That should simplify the implementation

---

_Label `rule` added by @MichaReiser on 2024-10-07 09:14_

---

_@MichaReiser requested changes on 2024-10-07 09:19_

The change makes sense to me. Although our settings aren't very explicit about what "starting" means:

> Comments starting with these tags will be ignored by commented-out code detection (ERA), and skipped by line-length rules (E501) if [ignore-overlong-task-comments](https://docs.astral.sh/ruff/settings/#lint_pycodestyle_ignore-overlong-task-comments) is set to true.

https://docs.astral.sh/ruff/settings/#lint_task-tags

What surprised me is that this not only changes the behavior of `flake8-todos` but also `flake8-fixme`

Flake8-fixme only tests for the use of task tags at word boundaries. 

https://github.com/tommilligan/flake8-fixme/blob/master/flake8_fixme/__init__.py

I think we can simplify the implementation, see my inline comment.

---

_Review comment by @autinerd on `crates/ruff_linter/src/directives.rs`:344 on 2024-10-11 12:08_

I don't quite understand, sorry (Maybe my Rust knowledge is not good enough)

---

_@autinerd reviewed on 2024-10-11 12:09_

---

_@MichaReiser reviewed on 2024-10-11 14:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/directives.rs`:344 on 2024-10-11 14:22_

I'm proposing a small rewrite because I think it will simplify the overall code now that we make the change here. 

What I propose is that `FromStr` does a full match (matching the entire string and not just a prefix). So `from_str` becomes something like this:


```rust
            match s.to_lowercase().as_str() {
                "fixme" => {
                    Ok(TodoDirectiveKind::Fixme)
                }
                "hack" => {
                    Ok(TodoDirectiveKind::Hack)
                }
                "todo" => {
                    Ok(TodoDirectiveKind::Todo)
                }
                "xxx" => {
                    Ok(TodoDirectiveKind::Xxx)
                }
                _ =>Err(()),
		}
```

This does require that we change the one call site to implement the: split by word boundaries (whitespace) and we only pass the first word to `FromStr`

---

_@autinerd reviewed on 2024-10-12 19:35_

---

_Review comment by @autinerd on `crates/ruff_linter/src/directives.rs`:344 on 2024-10-12 19:35_

I have applied your suggestion.

---

_@MichaReiser approved on 2024-10-14 09:36_

Thanks

---

_Merged by @MichaReiser on 2024-10-14 10:21_

---

_Closed by @MichaReiser on 2024-10-14 10:21_

---
