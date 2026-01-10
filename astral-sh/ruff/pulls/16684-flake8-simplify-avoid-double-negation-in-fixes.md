```yaml
number: 16684
title: "[`flake8-simplify`] Avoid double negation in fixes (`SIM103`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - fixes
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/sim103-fix-improvements
created_at: 2025-03-12T16:14:23Z
updated_at: 2025-03-13T07:45:53Z
url: https://github.com/astral-sh/ruff/pull/16684
synced_at: 2026-01-10T19:49:02Z
```

# [`flake8-simplify`] Avoid double negation in fixes (`SIM103`)

---

_Pull request opened by @MichaReiser on 2025-03-12 16:14_

## Summary

This PR stabilizes the fixes improvements made in https://github.com/astral-sh/ruff/pull/15562 (released with ruff 0.9.3 in mid January). 

There's no open issue or PR related to the changed fix behavior.

This is not a breaking change. The fix was only gated behind preview to get some more test coverage before releasing.




---

_Label `fixes` added by @MichaReiser on 2025-03-12 16:14_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 16:14_

---

_Renamed from "[`flake8-simplify`] Avoid double negation in proposed fixes (`SIM103`)" to "[`flake8-simplify`] Avoid double negation i fixes (`SIM103`)" by @MichaReiser on 2025-03-12 16:17_

---

_Renamed from "[`flake8-simplify`] Avoid double negation i fixes (`SIM103`)" to "[`flake8-simplify`] Avoid double negation in fixes (`SIM103`)" by @MichaReiser on 2025-03-12 16:17_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 16:17_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 16:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsim103-fix-improvements)

### Merging #16684 will **degrade performances by 4.61%**

<sub>Comparing <code>micha/sim103-fix-improvements</code> (fec9aa3) with <code>micha/ruff-0.10</code> (464ea4a)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsim103-fix-improvements)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.61% |


---

_Comment by @github-actions[bot] on 2025-03-12 16:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+13 -13 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/airflow/configuration.py#L1278'>airflow/configuration.py:1278:13:</a> SIM103 Return the condition `not value is None` directly
+ <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/airflow/configuration.py#L1278'>airflow/configuration.py:1278:13:</a> SIM103 Return the condition `value is not None` directly
+ <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/kubernetes_tests/test_base.py#L349'>kubernetes_tests/test_base.py:349:9:</a> SIM103 Return the condition `"(no change)" not in patch_configmap_result` directly
- <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/kubernetes_tests/test_base.py#L349'>kubernetes_tests/test_base.py:349:9:</a> SIM103 Return the condition `not "(no change)" in patch_configmap_result` directly
- <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/providers/amazon/src/airflow/providers/amazon/aws/sensors/athena.py#L94'>providers/amazon/src/airflow/providers/amazon/aws/sensors/athena.py:94:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/providers/amazon/src/airflow/providers/amazon/aws/sensors/athena.py#L94'>providers/amazon/src/airflow/providers/amazon/aws/sensors/athena.py:94:9:</a> SIM103 Return the condition `state not in self.INTERMEDIATE_STATES` directly
- <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/providers/amazon/src/airflow/providers/amazon/aws/sensors/emr.py#L316'>providers/amazon/src/airflow/providers/amazon/aws/sensors/emr.py:316:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/providers/amazon/src/airflow/providers/amazon/aws/sensors/emr.py#L316'>providers/amazon/src/airflow/providers/amazon/aws/sensors/emr.py:316:9:</a> SIM103 Return the condition `state not in self.INTERMEDIATE_STATES` directly
- <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/providers/amazon/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py#L110'>providers/amazon/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py:110:9:</a> SIM103 Return the condition `not state in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/providers/amazon/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py#L110'>providers/amazon/src/airflow/providers/amazon/aws/sensors/opensearch_serverless.py:110:9:</a> SIM103 Return the condition `state not in self.INTERMEDIATE_STATES` directly
+ <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `j not in ["Description", "Identifier"]` directly
- <a href='https://github.com/apache/airflow/blob/37b03c1d541a26bc90f9e6ed5ac3868867369824/scripts/ci/pre_commit/check_integrations_list.py#L113'>scripts/ci/pre_commit/check_integrations_list.py:113:9:</a> SIM103 Return the condition `not j in ["Description", "Identifier"]` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/commands/importers/v1/utils.py#L204'>superset/commands/importers/v1/utils.py:204:5:</a> SIM103 Return the condition `not path.suffix.lower() not in {".yaml", ".yml"}` directly
+ <a href='https://github.com/apache/superset/blob/c8f5089f7a2b0d987cd43d3ae53b326512c7c2c9/superset/commands/importers/v1/utils.py#L204'>superset/commands/importers/v1/utils.py:204:5:</a> SIM103 Return the condition `path.suffix.lower() in {".yaml", ".yml"}` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/bridge_skylark2.py#L9'>request_llms/bridge_skylark2.py:9:5:</a> SIM103 Return the condition `YUNQUE_SECRET_KEY != ''` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/bridge_skylark2.py#L9'>request_llms/bridge_skylark2.py:9:5:</a> SIM103 Return the condition `not YUNQUE_SECRET_KEY == ''` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/bridge_taichu.py#L12'>request_llms/bridge_taichu.py:12:5:</a> SIM103 Return the condition `TAICHU_API_KEY != ''` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/bridge_taichu.py#L12'>request_llms/bridge_taichu.py:12:5:</a> SIM103 Return the condition `not TAICHU_API_KEY == ''` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/bridge_zhipu.py#L12'>request_llms/bridge_zhipu.py:12:5:</a> SIM103 Return the condition `ZHIPUAI_API_KEY != ''` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/bridge_zhipu.py#L12'>request_llms/bridge_zhipu.py:12:5:</a> SIM103 Return the condition `not ZHIPUAI_API_KEY == ''` directly
+ <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/com_qwenapi.py#L16'>request_llms/com_qwenapi.py:16:13:</a> SIM103 Return the condition `DASHSCOPE_API_KEY != ''` directly
- <a href='https://github.com/binary-husky/gpt_academic/blob/045cdb15d86c262a55ede6dd9c84a5ab63dcc797/request_llms/com_qwenapi.py#L16'>request_llms/com_qwenapi.py:16:13:</a> SIM103 Return the condition `not DASHSCOPE_API_KEY == ''` directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f0e15cff77e7d9ed2dfb3f61d81a3d293ecfdcce/zerver/lib/message.py#L1683'>zerver/lib/message.py:1683:5:</a> SIM103 Return the condition `not user_profile.realm != message.get_realm()` directly
+ <a href='https://github.com/zulip/zulip/blob/f0e15cff77e7d9ed2dfb3f61d81a3d293ecfdcce/zerver/lib/message.py#L1683'>zerver/lib/message.py:1683:5:</a> SIM103 Return the condition `user_profile.realm == message.get_realm()` directly
+ <a href='https://github.com/zulip/zulip/blob/f0e15cff77e7d9ed2dfb3f61d81a3d293ecfdcce/zerver/views/custom_profile_fields.py#L82'>zerver/views/custom_profile_fields.py:82:5:</a> SIM103 Return the condition `field_data["subtype"] != "custom"` directly
- <a href='https://github.com/zulip/zulip/blob/f0e15cff77e7d9ed2dfb3f61d81a3d293ecfdcce/zerver/views/custom_profile_fields.py#L82'>zerver/views/custom_profile_fields.py:82:5:</a> SIM103 Return the condition `not field_data["subtype"] == "custom"` directly
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM103 | 26 | 13 | 13 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-03-12 16:28_

LGTM!

---

_@ntBre approved on 2025-03-12 16:30_

Nice, this looks like a much better fix

---

_Merged by @MichaReiser on 2025-03-13 07:45_

---

_Closed by @MichaReiser on 2025-03-13 07:45_

---

_Branch deleted on 2025-03-13 07:45_

---
