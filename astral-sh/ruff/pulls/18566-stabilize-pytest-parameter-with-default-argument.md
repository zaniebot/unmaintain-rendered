```yaml
number: 18566
title: "Stabilize `pytest-parameter-with-default-argument` (`PT028`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-pt028
created_at: 2025-06-08T19:20:17Z
updated_at: 2025-06-09T19:09:13Z
url: https://github.com/astral-sh/ruff/pull/18566
synced_at: 2026-01-12T15:56:21Z
```

# Stabilize `pytest-parameter-with-default-argument` (`PT028`)

---

_@dylwil3_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-08 19:20_

---

_Label `rule` added by @dylwil3 on 2025-06-08 19:20_

---

_Comment by @github-actions[bot] on 2025-06-08 19:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+91 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/70387b845abfd92e12d722d5a7c7f28a75a6cd4d/tests/formulary/test_thermal_speed.py#L400'>tests/formulary/test_thermal_speed.py:400:43:</a> PT028 Test function parameter `kwargs` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+46 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L1912'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:1912:100:</a> PT028 Test function parameter `serialized` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/listeners/test_listeners.py#L108'>airflow-core/tests/unit/listeners/test_listeners.py:108:76:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/listeners/test_listeners.py#L133'>airflow-core/tests/unit/listeners/test_listeners.py:133:91:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/listeners/test_listeners.py#L148'>airflow-core/tests/unit/listeners/test_listeners.py:148:96:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/listeners/test_listeners.py#L162'>airflow-core/tests/unit/listeners/test_listeners.py:162:61:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/listeners/test_listeners.py#L72'>airflow-core/tests/unit/listeners/test_listeners.py:72:60:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/listeners/test_listeners.py#L87'>airflow-core/tests/unit/listeners/test_listeners.py:87:59:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/models/test_taskinstance.py#L1265'>airflow-core/tests/unit/models/test_taskinstance.py:1265:96:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/models/test_taskinstance.py#L2284'>airflow-core/tests/unit/models/test_taskinstance.py:2284:61:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/models/test_taskinstance.py#L406'>airflow-core/tests/unit/models/test_taskinstance.py:406:71:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/models/test_timestamp.py#L57'>airflow-core/tests/unit/models/test_timestamp.py:57:49:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/airflow-core/tests/unit/models/test_timestamp.py#L69'>airflow-core/tests/unit/models/test_timestamp.py:69:63:</a> PT028 Test function parameter `session` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L101'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:101:54:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L122'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:122:59:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L167'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:167:65:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L186'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:186:64:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L207'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:207:68:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L40'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:40:52:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py#L77'>helm-tests/tests/helm_tests/airflow_aux/test_container_lifecycle.py:77:45:</a> PT028 Test function parameter `hook_type` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py#L1030'>providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py:1030:66:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py#L244'>providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py:244:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py#L254'>providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py:254:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py#L264'>providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py:264:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py#L333'>providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py:333:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
+ <a href='https://github.com/apache/airflow/blob/bd2fbb1fe7b240258ba26f0253feabff6452ec9f/providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py#L351'>providers/amazon/tests/unit/amazon/aws/hooks/test_eks.py:351:58:</a> PT028 Test function parameter `initial_batch_size` has default argument
... 21 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/08655a7559718e10049fa16f83f40146402405fa/superset/cli/test_db.py#L141'>superset/cli/test_db.py:141:66:</a> PT028 Test function parameter `raw_engine_kwargs` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7d542b4b429b50f3c6df223275363cad63c9a153/asv_bench/benchmarks/gil.py#L39'>asv_bench/benchmarks/gil.py:39:31:</a> PT028 Test function parameter `num_threads` has default argument
+ <a href='https://github.com/pandas-dev/pandas/blob/7d542b4b429b50f3c6df223275363cad63c9a153/asv_bench/benchmarks/gil.py#L39'>asv_bench/benchmarks/gil.py:39:46:</a> PT028 Test function parameter `kwargs_list` has default argument
+ <a href='https://github.com/pandas-dev/pandas/blob/7d542b4b429b50f3c6df223275363cad63c9a153/pandas/util/_tester.py#L15'>pandas/util/_tester.py:15:41:</a> PT028 Test function parameter `extra_args` has default argument
+ <a href='https://github.com/pandas-dev/pandas/blob/7d542b4b429b50f3c6df223275363cad63c9a153/pandas/util/_tester.py#L15'>pandas/util/_tester.py:15:68:</a> PT028 Test function parameter `run_doctests` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/tools/lib/test_server.py#L56'>tools/lib/test_server.py:56:34:</a> PT028 Test function parameter `skip_provision_check` has default argument
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/tools/lib/test_server.py#L57'>tools/lib/test_server.py:57:26:</a> PT028 Test function parameter `external_host` has default argument
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/tools/lib/test_server.py#L58'>tools/lib/test_server.py:58:28:</a> PT028 Test function parameter `log_file` has default argument
+ <a href='https://github.com/zulip/zulip/blob/c41a96a9540a2c9b7f6dadf6eef02ed3080dff81/tools/lib/test_server.py#L59'>tools/lib/test_server.py:59:18:</a> PT028 Test function parameter `dots` has default argument
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/io/votable/tests/test_vo.py#L950'>astropy/io/votable/tests/test_vo.py:950:36:</a> PT028 Test function parameter `test_path_object` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/modeling/tests/test_models.py#L47'>astropy/modeling/tests/test_models.py:47:41:</a> PT028 Test function parameter `amplitude` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/modeling/tests/test_models.py#L47'>astropy/modeling/tests/test_models.py:47:54:</a> PT028 Test function parameter `frequency` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_bayesian_blocks.py#L10'>astropy/stats/tests/test_bayesian_blocks.py:10:36:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_bayesian_blocks.py#L170'>astropy/stats/tests/test_bayesian_blocks.py:170:35:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_bayesian_blocks.py#L20'>astropy/stats/tests/test_bayesian_blocks.py:20:33:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_bayesian_blocks.py#L35'>astropy/stats/tests/test_bayesian_blocks.py:35:47:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L101'>astropy/stats/tests/test_histogram.py:101:38:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L101'>astropy/stats/tests/test_histogram.py:101:50:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L112'>astropy/stats/tests/test_histogram.py:112:43:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L112'>astropy/stats/tests/test_histogram.py:112:55:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L172'>astropy/stats/tests/test_histogram.py:172:30:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L172'>astropy/stats/tests/test_histogram.py:172:42:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L17'>astropy/stats/tests/test_histogram.py:17:28:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L17'>astropy/stats/tests/test_histogram.py:17:41:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L31'>astropy/stats/tests/test_histogram.py:31:31:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L31'>astropy/stats/tests/test_histogram.py:31:44:</a> PT028 Test function parameter `rseed` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L60'>astropy/stats/tests/test_histogram.py:60:28:</a> PT028 Test function parameter `N` has default argument
+ <a href='https://github.com/astropy/astropy/blob/09795abdae6ebf71bc58b04be52faf79e198a1a2/astropy/stats/tests/test_histogram.py#L60'>astropy/stats/tests/test_histogram.py:60:41:</a> PT028 Test function parameter `rseed` has default argument
... 16 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT028 | 91 | 91 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @ntBre on 2025-06-08 19:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-08 20:27_

---

_@ntBre approved on 2025-06-09 19:05_

I think there are definitely some false positives here because of our [`is_likely_pytest_test`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs#L63) function, which is pretty naive (not to say that we can necessarily do any better), but I think that's okay. It seems like a very useful rule for people actually using pytest.

---

_Merged by @dylwil3 on 2025-06-09 19:07_

---

_Closed by @dylwil3 on 2025-06-09 19:07_

---

_Branch deleted on 2025-06-09 19:07_

---

_Comment by @dylwil3 on 2025-06-09 19:09_

I think generally a way to make these rules more accurate is for the user to only enable them in a `tests/` directory in the first place

---
