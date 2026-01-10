```yaml
number: 18861
title: "[`ruff`] Fix `RUF056` false negative on shadowed bindings"
type: pull_request
state: closed
author: LaBatata101
labels: []
assignees: []
base: main
head: fix-RUF056
created_at: 2025-06-22T13:46:37Z
updated_at: 2025-06-24T06:12:15Z
url: https://github.com/astral-sh/ruff/pull/18861
synced_at: 2026-01-10T18:39:09Z
```

# [`ruff`] Fix `RUF056` false negative on shadowed bindings

---

_Pull request opened by @LaBatata101 on 2025-06-22 13:46_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR also affects `RUF051` and `SIM401` due to a change made to `is_known_to_be_of_type_dict` function.

Fixes #18855
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-22 13:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -24 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L763'>devel-common/src/docs/build_docs.py:763:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L763'>devel-common/src/docs/build_docs.py:763:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L765'>devel-common/src/docs/build_docs.py:765:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L765'>devel-common/src/docs/build_docs.py:765:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py#L150'>providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py:150:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L664'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:664:17:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L664'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:664:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L678'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:678:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L678'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:678:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/task-sdk/src/airflow/sdk/bases/operator.py#L1276'>task-sdk/src/airflow/sdk/bases/operator.py:1276:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/task-sdk/src/airflow/sdk/bases/operator.py#L1276'>task-sdk/src/airflow/sdk/bases/operator.py:1276:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/dashboards/schemas.py#L184'>superset/dashboards/schemas.py:184:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/dashboards/schemas.py#L184'>superset/dashboards/schemas.py:184:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/db_engine_specs/gsheets.py#L194'>superset/db_engine_specs/gsheets.py:194:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/db_engine_specs/gsheets.py#L194'>superset/db_engine_specs/gsheets.py:194:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/fits/diff.py#L1379'>astropy/io/fits/diff.py:1379:17:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/fits/diff.py#L1381'>astropy/io/fits/diff.py:1381:17:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/table/serialize.py#L222'>astropy/table/serialize.py:222:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF051 | 28 | 4 | 0 | 0 | 24 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -0 violations, +0 -24 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L763'>devel-common/src/docs/build_docs.py:763:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L763'>devel-common/src/docs/build_docs.py:763:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L765'>devel-common/src/docs/build_docs.py:765:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/devel-common/src/docs/build_docs.py#L765'>devel-common/src/docs/build_docs.py:765:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py#L150'>providers/databricks/src/airflow/providers/databricks/operators/databricks_sql.py:150:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L664'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:664:17:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L664'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:664:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L678'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:678:21:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/providers/google/src/airflow/providers/google/cloud/operators/dataproc.py#L678'>providers/google/src/airflow/providers/google/cloud/operators/dataproc.py:678:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/task-sdk/src/airflow/sdk/bases/operator.py#L1276'>task-sdk/src/airflow/sdk/bases/operator.py:1276:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/airflow/blob/3df86abc26bbd4d5ebeb80a38e7267ea1007754f/task-sdk/src/airflow/sdk/bases/operator.py#L1276'>task-sdk/src/airflow/sdk/bases/operator.py:1276:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -4 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/dashboards/schemas.py#L184'>superset/dashboards/schemas.py:184:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/dashboards/schemas.py#L184'>superset/dashboards/schemas.py:184:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/db_engine_specs/gsheets.py#L194'>superset/db_engine_specs/gsheets.py:194:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
- <a href='https://github.com/apache/superset/blob/cd3191bb79e13eb7098f3ab36b37d53da05b4fa5/superset/db_engine_specs/gsheets.py#L194'>superset/db_engine_specs/gsheets.py:194:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c406db2480d087e7d09e9b43e2fe118f0bd40933/zerver/lib/send_email.py#L219'>zerver/lib/send_email.py:219:51:</a> RUF056 [*] Avoid providing a falsy fallback to `dict.get()` in boolean test positions. The default fallback `None` is already falsy.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/fits/diff.py#L1379'>astropy/io/fits/diff.py:1379:17:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/io/fits/diff.py#L1381'>astropy/io/fits/diff.py:1381:17:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
+ <a href='https://github.com/astropy/astropy/blob/e48dd1c64b2433965363c7aa1d1ac19d70a93f28/astropy/table/serialize.py#L222'>astropy/table/serialize.py:222:13:</a> RUF051 Use `pop` instead of `key in dict` followed by `del dict[key]`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF051 | 28 | 4 | 0 | 0 | 24 |
| RUF056 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @LaBatata101 on 2025-06-22 19:46_

Ecosystem changes seem correct

---

_@MichaReiser reviewed on 2025-06-23 08:49_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:50 on 2025-06-23 08:49_

It seems that this function is also used by other rules. Did you check if the change is correct for those rules too?

For example, I don't think this cange is safe for `if_key_in_dict_del`:


```py
dictionary = {}
if key in dictionary:
		dictionary = {}
    del dictionary[key]
```

---

_@LaBatata101 reviewed on 2025-06-23 13:48_

---

_Review comment by @LaBatata101 on `crates/ruff_python_semantic/src/analyze/typing.rs`:50 on 2025-06-23 13:48_

It looks correct to me, all the changes in the ecosystem seems to be due to false negatives. For the example, you provided, `RUF051` is neither triggered in this branch nor in stable ([playground](https://play.ruff.rs/e81ce998-6703-475b-9950-e87ed5b73f18)).

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-23 13:49_

---

_@MichaReiser reviewed on 2025-06-23 14:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:50 on 2025-06-23 14:54_

@InSyncWithFoo do you remember what the reason was that you used `only_binding` over `resolve_name`?

---

_Comment by @MichaReiser on 2025-06-23 14:55_

We should update the summary to mention all affected rules. I'll try to do another review at a later point (in meetings now)

---

_Comment by @MichaReiser on 2025-06-23 15:55_

This is an example where Ruff now reports a false positive. 

```py
def demonstrate_false_positive():
    a = {}  # Initially a dict

    if True:
        a= set()
    else:
        a = {}


    # dict is now a string, should NOT trigger RUF056
    if a.get(key, False):  # But this DOES trigger - FALSE POSITIVE
        pass
```

I'm not quiet sure if this should prevent us from making this change because it's probably very unlikely in practice (because `get` will now also result in a runtime error) and I think most type checkers don't like redeclarations like this. 

If we make the change, then all fixes should be marked as unsafe at least.

---

_@InSyncWithFoo reviewed on 2025-06-23 17:07_

---

_Review comment by @InSyncWithFoo on `crates/ruff_python_semantic/src/analyze/typing.rs`:50 on 2025-06-23 17:07_

@MichaReiser If I recall correctly, I thought it would be safer to make a conclusion when there is only one binding.

---

_Comment by @LaBatata101 on 2025-06-23 21:54_

> This is an example where Ruff now reports a false positive.
> 
> ```python
> def demonstrate_false_positive():
>     a = {}  # Initially a dict
> 
>     if True:
>         a= set()
>     else:
>         a = {}
> 
> 
>     # dict is now a string, should NOT trigger RUF056
>     if a.get(key, False):  # But this DOES trigger - FALSE POSITIVE
>         pass
> ```
> 
> I'm not quiet sure if this should prevent us from making this change because it's probably very unlikely in practice (because `get` will now also result in a runtime error) and I think most type checkers don't like redeclarations like this.
> 
> If we make the change, then all fixes should be marked as unsafe at least.

I made the fixes unsafe

---

_Comment by @MichaReiser on 2025-06-24 06:12_

> I made the fixes unsafe

The idea wasn't to make all fixes unsafe. I'd argue that this is a step back compared to today. 

I think what we should do here instead is to either change `resolve_name` or change `only_binding` to allow redeclarations, but only if they are all in the same control flow path. I'm unsure how challenging this is or if it's even possible, as I'm not very familiar with Ruff's semantic model. While unlikely, I don't think that trading false positives with false negatives here is a good outcome. That's why I'll close this PR for now and raise this on the issue.

---

_Closed by @MichaReiser on 2025-06-24 06:12_

---
