```yaml
number: 15562
title: "[`flake8-simplify`] Avoid double negations (`SIM103`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: SIM103
created_at: 2025-01-18T00:07:49Z
updated_at: 2025-01-18T17:09:25Z
url: https://github.com/astral-sh/ruff/pull/15562
synced_at: 2026-01-10T20:05:43Z
```

# [`flake8-simplify`] Avoid double negations (`SIM103`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-18 00:07_

## Summary

Related to [this comment](https://github.com/astral-sh/ruff/issues/6184#issuecomment-2578673788) at #6184.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-18 00:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -8 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+5 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/airflow/configuration.py#L1259'>airflow/configuration.py:1259:13:</a> SIM103 Return the condition `not value is None` directly
+ <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/airflow/configuration.py#L1259'>airflow/configuration.py:1259:13:</a> SIM103 Return the condition `value is not None` directly
- <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/providers/src/airflow/providers/amazon/aws/sensors/athena.py#L94'>providers/src/airflow/providers/amazon/aws/sensors/athena.py:94:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/providers/src/airflow/providers/amazon/aws/sensors/athena.py#L94'>providers/src/airflow/providers/amazon/aws/sensors/athena.py:94:9:</a> SIM103 Return the condition `state not in self.INTERMEDIATE_STATES` directly
- <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/providers/src/airflow/providers/amazon/aws/sensors/emr.py#L316'>providers/src/airflow/providers/amazon/aws/sensors/emr.py:316:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/providers/src/airflow/providers/amazon/aws/sensors/emr.py#L316'>providers/src/airflow/providers/amazon/aws/sensors/emr.py:316:9:</a> SIM103 Return the condition `state not in self.INTERMEDIATE_STATES` directly
- <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/providers/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py#L110'>providers/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py:110:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/providers/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py#L110'>providers/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py:110:9:</a> SIM103 Return the condition `state not in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `j not in ["Description", "Identifier"]` directly
- <a href='https://github.com/apache/airflow/blob/3e8449fbcfa97c267b58447d1eb72b17c5fda76b/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `not j in ["Description", "Identifier"]` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/commands/importers/v1/utils.py#L204'>superset/commands/importers/v1/utils.py:204:5:</a> SIM103 Return the condition `not path.suffix.lower() not in {".yaml", ".yml"}` directly
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/commands/importers/v1/utils.py#L204'>superset/commands/importers/v1/utils.py:204:5:</a> SIM103 Return the condition `path.suffix.lower() in {".yaml", ".yml"}` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/264e49e7e8ede23be94089b5dd2237f577ad8518/zerver/lib/message.py#L1539'>zerver/lib/message.py:1539:5:</a> SIM103 Return the condition `not user_profile.realm != message.get_realm()` directly
+ <a href='https://github.com/zulip/zulip/blob/264e49e7e8ede23be94089b5dd2237f577ad8518/zerver/lib/message.py#L1539'>zerver/lib/message.py:1539:5:</a> SIM103 Return the condition `user_profile.realm == message.get_realm()` directly
+ <a href='https://github.com/zulip/zulip/blob/264e49e7e8ede23be94089b5dd2237f577ad8518/zerver/views/custom_profile_fields.py#L82'>zerver/views/custom_profile_fields.py:82:5:</a> SIM103 Return the condition `field_data["subtype"] != "custom"` directly
- <a href='https://github.com/zulip/zulip/blob/264e49e7e8ede23be94089b5dd2237f577ad8518/zerver/views/custom_profile_fields.py#L82'>zerver/views/custom_profile_fields.py:82:5:</a> SIM103 Return the condition `not field_data["subtype"] == "custom"` directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 16 | 8 | 8 | 0 | 0 |

</p>
</details>




---

_@dylwil3 requested changes on 2025-01-18 16:29_

Thanks! This looks great, but let's gate it behind `preview` in case there's something we're missing. After doing so, you'll have to add a note to the documentation about the fix behavior under preview.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/needless_bool.rs`:47 on 2025-01-18 17:01_

```suggestion
/// will be simplified to `a == b`, `a in b` and `a is b`, respectively.
```

---

_@dylwil3 approved on 2025-01-18 17:01_

Great, thank you!

---

_Merged by @dylwil3 on 2025-01-18 17:06_

---

_Closed by @dylwil3 on 2025-01-18 17:06_

---

_Branch deleted on 2025-01-18 17:09_

---

_Label `fixes` added by @dylwil3 on 2025-01-18 17:09_

---

_Label `preview` added by @dylwil3 on 2025-01-18 17:09_

---
