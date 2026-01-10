```yaml
number: 15145
title: "[airflow]: extend moved names (AIR303)"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend-AIR303
created_at: 2024-12-26T08:19:02Z
updated_at: 2024-12-26T09:55:07Z
url: https://github.com/astral-sh/ruff/pull/15145
synced_at: 2026-01-10T20:42:27Z
```

# [airflow]: extend moved names (AIR303)

---

_Pull request opened by @Lee-W on 2024-12-26 08:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Many core Airflow features have been deprecated and moved to Airflow Providers since users might need to install an additional package (e.g., `apache-airflow-provider-fab==1.0.0`); a separate rule (AIR303) is created for this.

The following is the ones that has been moved to `apache-airflow-provider-celery==3.0.0`

* `airflow.config_templates.default_celery.DEFAULT_CELERY_CONFIG` → `airflow.providers.celery.executors.default_celery.DEFAULT_CELERY_CONFIG`


## Test Plan

<!-- How was it tested? -->

A test fixture has been included for the rule.

---

_Comment by @github-actions[bot] on 2024-12-26 08:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/excel/_odfreader.py#L11'>pandas/io/excel/_odfreader.py:11:5:</a> TC001 Move application import `pandas._typing.FilePath` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/excel/_odfreader.py#L12'>pandas/io/excel/_odfreader.py:12:5:</a> TC001 Move application import `pandas._typing.ReadBuffer` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/excel/_odfreader.py#L13'>pandas/io/excel/_odfreader.py:13:5:</a> TC001 Move application import `pandas._typing.Scalar` into a type-checking block
+ <a href='https://github.com/pandas-dev/pandas/blob/8a5344742c5165b2595f7ccca9e17d5eff7f7886/pandas/io/excel/_odfreader.py#L14'>pandas/io/excel/_odfreader.py:14:5:</a> TC001 Move application import `pandas._typing.StorageOptions` into a type-checking block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC001 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2024-12-26 09:55_

---

_Label `preview` added by @MichaReiser on 2024-12-26 09:55_

---

_Merged by @MichaReiser on 2024-12-26 09:55_

---

_Closed by @MichaReiser on 2024-12-26 09:55_

---
