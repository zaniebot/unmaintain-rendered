```yaml
number: 22772
title: "[`pyupgrade`] Make fix unsafe if it deletes comments (`UP045`, `UP007`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
assignees: []
merged: true
base: main
head: __UP
created_at: 2026-01-20T18:07:45Z
updated_at: 2026-01-21T15:38:13Z
url: https://github.com/astral-sh/ruff/pull/22772
synced_at: 2026-01-21T16:04:59Z
```

# [`pyupgrade`] Make fix unsafe if it deletes comments (`UP045`, `UP007`)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @astral-sh-bot[bot] on 2026-01-20 18:24_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -10 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/charts/client_processing.py#L285'>superset/charts/client_processing.py:285:17:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/charts/client_processing.py#L285'>superset/charts/client_processing.py:285:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L292'>superset/db_engine_specs/snowflake.py:292:26:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L292'>superset/db_engine_specs/snowflake.py:292:26:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L314'>superset/db_engine_specs/snowflake.py:314:26:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L314'>superset/db_engine_specs/snowflake.py:314:26:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L2634'>superset/models/helpers.py:2634:17:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L2634'>superset/models/helpers.py:2634:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L897'>superset/models/helpers.py:897:29:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L897'>superset/models/helpers.py:897:29:</a> UP045 [*] Use `X | None` for type annotations
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP045 | 10 | 0 | 0 | 0 | 10 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -10 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/charts/client_processing.py#L285'>superset/charts/client_processing.py:285:17:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/charts/client_processing.py#L285'>superset/charts/client_processing.py:285:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L292'>superset/db_engine_specs/snowflake.py:292:26:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L292'>superset/db_engine_specs/snowflake.py:292:26:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L314'>superset/db_engine_specs/snowflake.py:314:26:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/db_engine_specs/snowflake.py#L314'>superset/db_engine_specs/snowflake.py:314:26:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L2634'>superset/models/helpers.py:2634:17:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L2634'>superset/models/helpers.py:2634:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L897'>superset/models/helpers.py:897:29:</a> UP045 Use `X | None` for type annotations
- <a href='https://github.com/apache/superset/blob/238bebebeced9bd238d8281f7c966ab132e293f7/superset/models/helpers.py#L897'>superset/models/helpers.py:897:29:</a> UP045 [*] Use `X | None` for type annotations
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP045 | 10 | 0 | 0 | 0 | 10 |

</p>
</details>





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:157 on 2026-01-21 14:18_

We also need to update the documentation for both rules, right?

---

_@ntBre requested changes on 2026-01-21 14:18_

---

_Label `fixes` added by @ntBre on 2026-01-21 14:18_

---

_@chirizxc reviewed on 2026-01-21 14:31_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_annotation.rs`:157 on 2026-01-21 14:31_

Yes, I forget about it😅

---

_@ntBre approved on 2026-01-21 15:37_

Thank you!

---

_Merged by @ntBre on 2026-01-21 15:38_

---

_Closed by @ntBre on 2026-01-21 15:38_

---
