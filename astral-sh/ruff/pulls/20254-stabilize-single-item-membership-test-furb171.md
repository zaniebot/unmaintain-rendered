```yaml
number: 20254
title: "Stabilize `single-item-membership-test` (`FURB171`)"
type: pull_request
state: closed
author: ntBre
labels:
  - rule
assignees: []
draft: true
base: brent/0.13.0
head: brent/furb171
created_at: 2025-09-04T20:38:23Z
updated_at: 2025-09-04T23:30:13Z
url: https://github.com/astral-sh/ruff/pull/20254
synced_at: 2026-01-12T15:56:57Z
```

# Stabilize `single-item-membership-test` (`FURB171`)

---

_@ntBre_

This rule has been a stabilization candidate at least as far back as 0.6, but it
had a few issues that blocked stabilization each time. The last issue was before
the 0.12 release, so I think it's worth trying this one again.

The docs and tests looked good.


---

_Added to milestone `v0.13` by @ntBre on 2025-09-04 20:38_

---

_Label `rule` added by @ntBre on 2025-09-04 20:38_

---

_Comment by @github-actions[bot] on 2025-09-04 20:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+34 -0 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+22 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/src/airflow/utils/db.py#L1411'>airflow-core/src/airflow/utils/db.py:1411:8:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/tests/unit/cli/commands/test_variable_command.py#L516'>airflow-core/tests/unit/cli/commands/test_variable_command.py:516:16:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/airflow-core/tests/unit/cli/commands/test_variable_command.py#L533'>airflow-core/tests/unit/cli/commands/test_variable_command.py:533:16:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/dev/breeze/src/airflow_breeze/utils/run_tests.py#L420'>dev/breeze/src/airflow_breeze/utils/run_tests.py:420:8:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/dev/react-plugin-tools/bootstrap.py#L59'>dev/react-plugin-tools/bootstrap.py:59:10:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/dev/react-plugin-tools/bootstrap.py#L62'>dev/react-plugin-tools/bootstrap.py:62:10:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/devel-common/src/docs/provider_conf.py#L270'>devel-common/src/docs/provider_conf.py:270:4:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L140'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:140:103:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L164'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:164:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L186'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:186:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L187'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:187:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L209'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:209:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L210'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:210:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L267'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:267:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L268'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:268:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L278'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:278:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/common/sql/tests/unit/common/sql/sensors/test_sql.py#L279'>providers/common/sql/tests/unit/common/sql/sensors/test_sql.py:279:31:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/edge3/src/airflow/providers/edge3/plugins/edge_executor_plugin.py#L223'>providers/edge3/src/airflow/providers/edge3/plugins/edge_executor_plugin.py:223:51:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/fab/src/airflow/providers/fab/www/extensions/init_views.py#L112'>providers/fab/src/airflow/providers/fab/www/extensions/init_views.py:112:70:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/google/src/airflow/providers/google/cloud/triggers/cloud_build.py#L90'>providers/google/src/airflow/providers/google/cloud/triggers/cloud_build.py:90:20:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/providers/snowflake/src/airflow/providers/snowflake/triggers/snowflake_trigger.py#L80'>providers/snowflake/src/airflow/providers/snowflake/triggers/snowflake_trigger.py:80:24:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/airflow/blob/1d73c0124dddc1cee2ecd4fa0769e372ea44e081/scripts/ci/prek/mypy_folder.py#L114'>scripts/ci/prek/mypy_folder.py:114:14:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/7fb7ac8bef808deb57b6d441785e48c94325dea2/superset/connectors/sqla/models.py#L796'>superset/connectors/sqla/models.py:796:62:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/apache/superset/blob/7fb7ac8bef808deb57b6d441785e48c94325dea2/superset/models/helpers.py#L2120'>superset/models/helpers.py:2120:26:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/dd993fd5f0c5731e56ef8024f216b7f73ea2011f/reflex/utils/templates.py#L388'>reflex/utils/templates.py:388:34:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/24bd675a4a677e75324b61a358eee19c098f4c58/tests/test_settings_overrides.py#L441'>tests/test_settings_overrides.py:441:8:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/tools/lib/provision.py#L155'>tools/lib/provision.py:155:28:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/tools/lib/template_parser.py#L666'>tools/lib/template_parser.py:666:29:</a> FURB171 Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/tools/lib/template_parser.py#L674'>tools/lib/template_parser.py:674:29:</a> FURB171 Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/zerver/actions/presence.py#L87'>zerver/actions/presence.py:87:8:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/zerver/lib/emoji.py#L107'>zerver/lib/emoji.py:107:12:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/zerver/lib/narrow_predicate.py#L60'>zerver/lib/narrow_predicate.py:60:39:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/zerver/lib/url_decoding.py#L136'>zerver/lib/url_decoding.py:136:30:</a> FURB171 [*] Membership test against single-item container
+ <a href='https://github.com/zulip/zulip/blob/6615756437b00098043230bf6d2b9b3f448888dc/zerver/tests/test_subs.py#L2704'>zerver/tests/test_subs.py:2704:12:</a> FURB171 [*] Membership test against single-item container
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB171 | 34 | 34 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-09-04 23:30_

#20255

---

_Closed by @ntBre on 2025-09-04 23:30_

---
