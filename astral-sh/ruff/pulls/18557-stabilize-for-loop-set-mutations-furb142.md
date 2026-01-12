```yaml
number: 18557
title: "Stabilize `for-loop-set-mutations` (`FURB142`)"
type: pull_request
state: closed
author: dylwil3
labels:
  - rule
assignees: []
base: brent/release-0.12.0
head: dylan/stabilize-furb142
created_at: 2025-06-08T19:05:09Z
updated_at: 2025-06-09T21:09:45Z
url: https://github.com/astral-sh/ruff/pull/18557
synced_at: 2026-01-12T15:56:21Z
```

# Stabilize `for-loop-set-mutations` (`FURB142`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:05_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:05_

---

_Comment by @github-actions[bot] on 2025-06-08 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+25 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/src/airflow/utils/dag_edges.py#L74'>airflow-core/src/airflow/utils/dag_edges.py:74:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L525'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:525:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/utils/provider_dependencies.py#L53'>dev/breeze/src/airflow_breeze/utils/provider_dependencies.py:53:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L952'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:952:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L958'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:958:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/breeze/src/airflow_breeze/utils/selective_checks.py#L986'>dev/breeze/src/airflow_breeze/utils/selective_checks.py:986:17:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/stats/calculate_statistics_provider_testing_issues.py#L110'>dev/stats/calculate_statistics_provider_testing_issues.py:110:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/dev/stats/calculate_statistics_provider_testing_issues.py#L117'>dev/stats/calculate_statistics_provider_testing_issues.py:117:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/src/airflow/providers/amazon/aws/hooks/glue_catalog.py#L124'>providers/amazon/src/airflow/providers/amazon/aws/hooks/glue_catalog.py:124:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/src/airflow/providers/amazon/aws/hooks/glue_catalog.py#L82'>providers/amazon/src/airflow/providers/amazon/aws/hooks/glue_catalog.py:82:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/scripts/tools/list-integrations.py#L67'>scripts/tools/list-integrations.py:67:9:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/extensions/__init__.py#L81'>superset/extensions/__init__.py:81:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/utils/dashboard_import_export.py#L30'>superset/utils/dashboard_import_export.py:30:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/utils/pandas_postprocessing/pivot.py#L89'>superset/utils/pandas_postprocessing/pivot.py:89:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/viz.py#L1662'>superset/viz.py:1662:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/tests/integration_tests/security_tests.py#L1504'>tests/integration_tests/security_tests.py:1504:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/tests/integration_tests/security_tests.py#L86'>tests/integration_tests/security_tests.py:86:5:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/document/test_models.py#L113'>tests/unit/bokeh/document/test_models.py:113:9:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/export.py#L1852'>zerver/lib/export.py:1852:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/streams.py#L1668'>zerver/lib/streams.py:1668:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/subscription_info.py#L690'>zerver/lib/subscription_info.py:690:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/thumbnail.py#L295'>zerver/lib/thumbnail.py:295:5:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/lib/user_groups.py#L676'>zerver/lib/user_groups.py:676:13:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/tests/test_docs.py#L335'>zerver/tests/test_docs.py:335:9:</a> FURB142 [*] Use of `set.add()` in a for loop
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/zerver/tests/test_docs.py#L342'>zerver/tests/test_docs.py:342:13:</a> FURB142 [*] Use of `set.add()` in a for loop
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB142 | 25 | 25 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:30_

---

_Comment by @ntBre on 2025-06-09 00:14_

Not sure if it's a blocker, but there's a new issue on this one: https://github.com/astral-sh/ruff/issues/18575

---

_Comment by @dylwil3 on 2025-06-09 21:09_

Hmm... I think it _could_ be a blocker, actually. The fix would be to check whether the loop variable is used outside of the loop (this is already done in `for-loop-writes` so we can steal another helper function).

But I think this ends up being a fairly significant change if someone has a lot of violations in the same scope that re-use the loop variable name. (For example - our test fixture fails to converge after 10 iterations if we make this behavior change).

Not sure how much of a big deal that is in practice, but probably worth delaying stabilization until that behavior has sat in preview for a round.

---

_Closed by @dylwil3 on 2025-06-09 21:09_

---
