```yaml
number: 12858
title: "[Ruff 0.6] Stabilise `PLR6104`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: ruff-0.6
head: stabilise-PLR6104
created_at: 2024-08-13T12:35:57Z
updated_at: 2024-08-14T10:33:00Z
url: https://github.com/astral-sh/ruff/pull/12858
synced_at: 2026-01-10T21:38:32Z
```

# [Ruff 0.6] Stabilise `PLR6104`

---

_Pull request opened by @AlexWaygood on 2024-08-13 12:35_

Split out from #12857 so that it can be considered in isolation.

## Summary

This PR stabilises the `non-augmented-assignment` pylint rule (`PLR6104`). I think this rule makes sense and is reasonably uncontroversial. It's been in preview for >3 months and there haven't been any significant issues opened about it in several months.

However, it's a stylistic rule that's somewhat pedantic, and it has a lot of ecosystem hits.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-13 12:35_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-08-13 12:35_

---

_Comment by @github-actions[bot] on 2024-08-13 12:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+358 -0 violations, +0 -0 fixes in 11 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+33 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/analysis/time_series/running_moments.py#L61'>src/plasmapy/analysis/time_series/running_moments.py:61:5:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L440'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:440:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L446'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:446:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L777'>src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:777:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/langmuir.py#L1398'>src/plasmapy/diagnostics/langmuir.py:1398:5:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/langmuir.py#L809'>src/plasmapy/diagnostics/langmuir.py:809:9:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/thomson.py#L199'>src/plasmapy/diagnostics/thomson.py:199:5:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/thomson.py#L553'>src/plasmapy/diagnostics/thomson.py:553:5:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/thomson.py#L554'>src/plasmapy/diagnostics/thomson.py:554:5:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/thomson.py#L933'>src/plasmapy/diagnostics/thomson.py:933:5:</a> PLR6104 Use `/=` to perform an augmented assignment directly
... 23 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+44 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/io/path.py#L292'>airflow/io/path.py:292:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/io/path.py#L294'>airflow/io/path.py:294:13:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/jobs/scheduler_job_runner.py#L869'>airflow/jobs/scheduler_job_runner.py:869:37:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py#L89'>airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py:89:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py#L90'>airflow/kubernetes/pre_7_4_0_compatibility/kube_client.py:90:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/providers/amazon/aws/hooks/eks.py#L546'>airflow/providers/amazon/aws/hooks/eks.py:546:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/providers/amazon/aws/hooks/eks.py#L549'>airflow/providers/amazon/aws/hooks/eks.py:549:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/providers/cncf/kubernetes/kube_client.py#L90'>airflow/providers/cncf/kubernetes/kube_client.py:90:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/providers/cncf/kubernetes/kube_client.py#L91'>airflow/providers/cncf/kubernetes/kube_client.py:91:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/airflow/blob/aeb5a52ba482422aef6fde354a82a1f2588c3e9e/airflow/providers/teradata/transfers/s3_to_teradata.py#L106'>airflow/providers/teradata/transfers/s3_to_teradata.py:106:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/scripts/build_docker.py#L284'>scripts/build_docker.py:284:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/advanced_data_type/plugins/internet_address.py#L93'>superset/advanced_data_type/plugins/internet_address.py:93:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/common/query_context_processor.py#L618'>superset/common/query_context_processor.py:618:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/db_engine_specs/base.py#L1156'>superset/db_engine_specs/base.py:1156:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/migrations/shared/security_converge.py#L259'>superset/migrations/shared/security_converge.py:259:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/migrations/shared/security_converge.py#L272'>superset/migrations/shared/security_converge.py:272:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py#L57'>superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py:57:17:</a> PLR6104 Use `*=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py#L58'>superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py:58:17:</a> PLR6104 Use `*=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py#L60'>superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py:60:17:</a> PLR6104 Use `*=` to perform an augmented assignment directly
+ <a href='https://github.com/apache/superset/blob/9f5eb899e87a1640887212b1942ed816a87cbec4/superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py#L79'>superset/migrations/versions/2018-02-13_08-07_e866bd2d4976_smaller_grid.py:79:17:</a> PLR6104 Use `/=` to perform an augmented assignment directly
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+128 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/commands/_utils/table_print.py#L46'>samcli/commands/_utils/table_print.py:46:9:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/commands/init/interactive_event_bridge_flow.py#L174'>samcli/commands/init/interactive_event_bridge_flow.py:174:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/build/bundler.py#L178'>samcli/lib/build/bundler.py:178:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/deploy/deployer.py#L468'>samcli/lib/deploy/deployer.py:468:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/deploy/deployer.py#L820'>samcli/lib/deploy/deployer.py:820:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/cli_paginator.py#L27'>samcli/lib/schemas/cli_paginator.py:27:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/cli_paginator.py#L34'>samcli/lib/schemas/cli_paginator.py:34:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/cli_paginator.py#L38'>samcli/lib/schemas/cli_paginator.py:38:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/cli_paginator.py#L42'>samcli/lib/schemas/cli_paginator.py:42:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/cli_paginator.py#L59'>samcli/lib/schemas/cli_paginator.py:59:9:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/schemas_aws_config.py#L52'>samcli/lib/schemas/schemas_aws_config.py:52:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/schemas_directory_hierarchy_builder.py#L14'>samcli/lib/schemas/schemas_directory_hierarchy_builder.py:14:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/schemas_directory_hierarchy_builder.py#L21'>samcli/lib/schemas/schemas_directory_hierarchy_builder.py:21:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/schemas/schemas_directory_hierarchy_builder.py#L22'>samcli/lib/schemas/schemas_directory_hierarchy_builder.py:22:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/utils/retry.py#L34'>samcli/lib/utils/retry.py:34:21:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/lib/utils/retry.py#L35'>samcli/lib/utils/retry.py:35:21:</a> PLR6104 Use `-=` to perform an augmented assignment directly
+ <a href='https://github.com/aws/aws-sam-cli/blob/d3bf78f60b47b19dae263f01127806176f5bb347/samcli/local/docker/lambda_container.py#L131'>samcli/local/docker/lambda_container.py:131:17:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 111 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L379'>src/bokeh/resources.py:379:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/util.py#L96'>src/bokeh/server/util.py:96:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/token.py#L308'>src/bokeh/util/token.py:308:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/e76017f79ab0754e5f36505fe73e9b5aa9dd7a1b/securedrop/tests/test_journalist.py#L1864'>securedrop/tests/test_journalist.py:1864:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/1920c8ddac0ebdd82b6ddaede360d113c48ef8bf/ibis/backends/bigquery/tests/unit/udf/test_usage.py#L25'>ibis/backends/bigquery/tests/unit/udf/test_usage.py:25:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/1920c8ddac0ebdd82b6ddaede360d113c48ef8bf/ibis/backends/pandas/executor.py#L718'>ibis/backends/pandas/executor.py:718:17:</a> PLR6104 Use `&=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/1920c8ddac0ebdd82b6ddaede360d113c48ef8bf/ibis/common/tests/test_numeric.py#L63'>ibis/common/tests/test_numeric.py:63:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/1920c8ddac0ebdd82b6ddaede360d113c48ef8bf/ibis/common/tests/test_numeric.py#L68'>ibis/common/tests/test_numeric.py:68:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/1920c8ddac0ebdd82b6ddaede360d113c48ef8bf/ibis/common/tests/test_numeric.py#L75'>ibis/common/tests/test_numeric.py:75:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/ibis-project/ibis/blob/1920c8ddac0ebdd82b6ddaede360d113c48ef8bf/ibis/expr/tests/test_api.py#L171'>ibis/expr/tests/test_api.py:171:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L142'>examples/example_bulkinsert_json.py:142:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L224'>examples/example_bulkinsert_json.py:224:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L247'>examples/example_bulkinsert_json.py:247:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L249'>examples/example_bulkinsert_json.py:249:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L251'>examples/example_bulkinsert_json.py:251:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L253'>examples/example_bulkinsert_json.py:253:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_json.py#L255'>examples/example_bulkinsert_json.py:255:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_numpy.py#L132'>examples/example_bulkinsert_numpy.py:132:5:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_numpy.py#L226'>examples/example_bulkinsert_numpy.py:226:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/milvus-io/pymilvus/blob/66c23629cca718f4dc685d9a9efa31a207ea2442/examples/example_bulkinsert_numpy.py#L249'>examples/example_bulkinsert_numpy.py:249:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+85 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/asv_bench/benchmarks/algos/isin.py#L136'>asv_bench/benchmarks/algos/isin.py:136:13:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/asv_bench/benchmarks/arithmetic.py#L141'>asv_bench/benchmarks/arithmetic.py:141:13:</a> PLR6104 Use `//=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/algorithms.py#L916'>pandas/core/algorithms.py:916:9:</a> PLR6104 Use `/=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/apply.py#L1868'>pandas/core/apply.py:1868:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/arrow/array.py#L2545'>pandas/core/arrays/arrow/array.py:2545:9:</a> PLR6104 Use `+=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/boolean.py#L232'>pandas/core/arrays/boolean.py:232:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/boolean.py#L239'>pandas/core/arrays/boolean.py:239:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/masked.py#L689'>pandas/core/arrays/masked.py:689:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/masked.py#L691'>pandas/core/arrays/masked.py:691:17:</a> PLR6104 Use `|=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/masked.py#L942'>pandas/core/arrays/masked.py:942:13:</a> PLR6104 Use `^=` to perform an augmented assignment directly
+ <a href='https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/period.py#L1226'>pandas/core/arrays/period.py:1226:9:</a> PLR6104 Use `*=` to perform an augmented assignment directly
... 74 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR6104 | 358 | 358 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2024-08-13 13:09_

---

_Comment by @MichaReiser on 2024-08-13 13:19_

Yeah, i"m not entirely sure about this one either. 

I do think it improves readability in some cases because it makes the precedence more clear. But the fact that the rule has no fix (according to the ecosystem results) makes the adoption fairly painful. 

To me it's not entirely clear if I would consider this an idiomatic rule or a stylistic rule. If it's the latter, then our rule acceptance guidelines would say that we should not accept the rule. If it's idiomatic, then I would probably go ahead

---

_Comment by @AlexWaygood on 2024-08-13 13:53_

> But the fact that the rule has no fix (according to the ecosystem results) makes the adoption fairly painful.

it seems like it does have an autofix, but that the autofix must be marked as unsafe in many situations :/

---

_Comment by @AlexWaygood on 2024-08-13 15:20_

Having discussed this offline, we'll merge this since:
- It's included as a builtin rule in the original pylint tool
- It's not enabled by default
- It's easy to switch off the rule if you don't like it
- Our pylint rules in general are somewhat more pedantic and opinionated than our other rules

---

_Merged by @AlexWaygood on 2024-08-13 15:21_

---

_Closed by @AlexWaygood on 2024-08-13 15:21_

---

_Branch deleted on 2024-08-13 15:21_

---

_Comment by @kdebrab on 2024-08-13 20:22_

@AlexWaygood I'm a bit concerned about this rule, because there are quite some cases where the application would unintentionally introduce nasty and hard-to-find bugs, in particular when it is applied on input arguments of a function. I'm afraid one could sometimes not be aware that applying the fix causes bugs for these cases. It is especially prevalent for data science libraries when working with numpy arrays or pandas Series / DataFrames, but it could also happen in case the argument is a dictionary or list. In my view, these are false positives which could and should be avoided by not applying the rule on variables that are function arguments.

Skimming through the examples above from the ecosystem, I found 10 such cases where applying the fix would lead to (not always evident) bugs:

[PlasmaPy/PlasmaPy](https://github.com/PlasmaPy/PlasmaPy)
+ [src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:440:13:](https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L440) PLR6104 Use `/=` to perform an augmented assignment directly
+ [src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py:446:13:](https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/charged_particle_radiography/synthetic_radiography.py#L446) PLR6104 Use `/=` to perform an augmented assignment directly
+ [src/plasmapy/diagnostics/langmuir.py:1398:5:](https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/langmuir.py#L1398) PLR6104 Use `/=` to perform an augmented assignment directly
+ [src/plasmapy/diagnostics/langmuir.py:809:9:](https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/langmuir.py#L809) PLR6104 Use `-=` to perform an augmented assignment directly
+ [src/plasmapy/diagnostics/thomson.py:553:5:](https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/thomson.py#L553) PLR6104 Use `/=` to perform an augmented assignment directly
+ [src/plasmapy/diagnostics/thomson.py:554:5:](https://github.com/PlasmaPy/PlasmaPy/blob/68f548c8614eb7e09d1137176c5c32a2ae7f55bc/src/plasmapy/diagnostics/thomson.py#L554) PLR6104 Use `/=` to perform an augmented assignment directly

[pandas-dev/pandas](https://github.com/pandas-dev/pandas)
+ [pandas/core/arrays/boolean.py:232:17:](https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/boolean.py#L232) PLR6104 Use `|=` to perform an augmented assignment directly
+ [pandas/core/arrays/boolean.py:239:17:](https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/boolean.py#L239) PLR6104 Use `|=` to perform an augmented assignment directly
+ [pandas/core/arrays/masked.py:689:17:](https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/masked.py#L689) PLR6104 Use `|=` to perform an augmented assignment directly
+ [pandas/core/arrays/masked.py:691:17:](https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/masked.py#L691) PLR6104 Use `|=` to perform an augmented assignment directly

Would it be possible to somehow avoid raising PLR6104 for variables that are function arguments?

---

_Comment by @zanieb on 2024-08-13 22:58_

By chance, I think the first one I clicked on ([pandas/core/arrays/boolean.py#L239](https://github.com/pandas-dev/pandas/blob/0fadaa9c9da39760e29e05012b14c8ce0782b5fe/pandas/core/arrays/boolean.py#L239)) is not a bug? I opened a couple others and you look correct though. I think that's a fair point, it does seem especially dangerous to apply this rule to variables bound to function inputs. I'm not sure if we have logic based on that already, it seems feasible?

What's the deal with this rule in Pylint? Does it have the same problems? i.e. does it report violations for the examples you selected?

---

_Comment by @AlexWaygood on 2024-08-13 23:02_

Thanks for speaking up @kdebrab, really appreciate it. I'll revert this PR tomorrow morning before we cut the release, so we can take our time to consider this more fully.

---

_Comment by @AlexWaygood on 2024-08-14 09:03_

Reverted in https://github.com/astral-sh/ruff/pull/12884

---

_Comment by @AlexWaygood on 2024-08-14 10:33_

I've opened #12890 to discuss if we can reduce the number of unsafe recommendations from this rule.

---
