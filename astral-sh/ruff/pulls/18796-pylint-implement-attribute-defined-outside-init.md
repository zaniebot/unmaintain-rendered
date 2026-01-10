```yaml
number: 18796
title: "[pylint] Implement attribute-defined-outside-init (PLW0201)"
type: pull_request
state: closed
author: jack-mcivor
labels: []
assignees: []
draft: true
base: main
head: attr-outside-init
created_at: 2025-06-19T17:22:10Z
updated_at: 2025-12-10T18:08:20Z
url: https://github.com/astral-sh/ruff/pull/18796
synced_at: 2026-01-10T16:42:11Z
```

# [pylint] Implement attribute-defined-outside-init (PLW0201)

---

_Pull request opened by @jack-mcivor on 2025-06-19 17:22_

## Summary
- Add PLW0201 attribute-defined-outside-init, based on https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/attribute-defined-outside-init.html
- Unchecked from https://github.com/astral-sh/ruff/issues/970
- For now, cover basic cases - intentionally be more lenient than pylint. Skip cases where:
  - The class inherits from another class (which may inject attributes)
  - The class is decorated (which may inject attributes)
  - The class does not have any defining-attr-methods (eg. no `__init__` and no `__post_init__`, ...)
    - this reduces the number of violations on the ecosystem (roughly 1150 -> 109). ~700 of these are from pandas benchmarking code that defers setup - eg. https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/asv_bench/benchmarks/algorithms.py#L198-L207
    - I'm unsure if we should do this or not. Ignoring these classes does mean the most basic case in the pylint docs isn't caught! https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/attribute-defined-outside-init.html
  - The class has a metaclass (which may inject attributes)
  - `setattr` is used to define an attribute
  - The method is a `property.setter`

- Define new configuration `pylint.defining-attr-methods` based on https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#defining-attr-methods, with default `["__init__", "__new__", "setUp", "asyncSetUp", "__post_init__", "setup_class", "setup_method", "__enter__", "__aenter__"]`
  - `["setup_class", "setup_method", "__enter__", "__aenter__"]` are additional to the pylint defaults
  - Walk all defining-attr-methods, and consider any method call also a defining-attr-method. Track assignments made in branches in any defining-attr-method, even if that branch isn't actually run

## Test Plan
- Snapshot tests added


---

_@jack-mcivor reviewed on 2025-06-19 17:30_

---

_Review comment by @jack-mcivor on `crates/ruff_linter/resources/test/fixtures/pylint/attribute_defined_outside_init.py`:100 on 2025-06-19 17:30_

pylint does trigger on this. It is difficult to know if the decorator sets the attribute, eg:
```python
def custom_decorator(cls):
    def d(cls):
        cls.attr = None
        return cls
    return d
```

---

_Comment by @github-actions[bot] on 2025-06-19 17:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+109 -0 violations, +0 -0 fixes in 14 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2ab0c535b021dd120b5a6aff67dc1c195b9913ae/disnake/state.py#L760'>disnake/state.py:760:9:</a> PLW0201 Attribute `_ready_state` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/particles/ionization_state.py#L520'>src/plasmapy/particles/ionization_state.py:520:9:</a> PLW0201 Attribute `_ionic_fractions` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L1045'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:1045:9:</a> PLW0201 Attribute `iteration_number` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L1074'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:1074:9:</a> PLW0201 Attribute `ever_entered_any_grid` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L405'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:405:9:</a> PLW0201 Attribute `q` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L406'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:406:9:</a> PLW0201 Attribute `m` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L407'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:407:9:</a> PLW0201 Attribute `_particle` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L421'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:421:9:</a> PLW0201 Attribute `x` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L422'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:422:9:</a> PLW0201 Attribute `v` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L544'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:544:9:</a> PLW0201 Attribute `_stopping_method` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L545'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:545:9:</a> PLW0201 Attribute `_stopping_power_interpolators` defined outside `__init__`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/701128f79c0e40b7011945c3d0ac0049c1c6963d/src/plasmapy/simulation/particle_tracker/particle_tracker.py#L600'>src/plasmapy/simulation/particle_tracker/particle_tracker.py:600:9:</a> PLW0201 Attribute `_grid_resolutions` defined outside `__init__`
... 13 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L77'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:77:9:</a> PLW0201 Attribute `default_time` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L78'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:78:9:</a> PLW0201 Attribute `ti_init` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L82'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:82:9:</a> PLW0201 Attribute `ti_extras` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py#L92'>airflow-core/tests/unit/api_fastapi/core_api/routes/public/test_task_instances.py:92:9:</a> PLW0201 Attribute `dagbag` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/airflow-core/tests/unit/models/test_dagwarning.py#L65'>airflow-core/tests/unit/models/test_dagwarning.py:65:9:</a> PLW0201 Attribute `session_mock` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py#L56'>providers/alibaba/tests/unit/alibaba/cloud/log/test_oss_task_handler.py:56:9:</a> PLW0201 Attribute `ti` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/amazon/tests/system/amazon/aws/utils/__init__.py#L170'>providers/amazon/tests/system/amazon/aws/utils/__init__.py:170:9:</a> PLW0201 Attribute `default_value` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/amazon/tests/unit/amazon/aws/operators/test_comprehend.py#L149'>providers/amazon/tests/unit/amazon/aws/operators/test_comprehend.py:149:9:</a> PLW0201 Attribute `op` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/amazon/tests/unit/amazon/aws/operators/test_comprehend.py#L249'>providers/amazon/tests/unit/amazon/aws/operators/test_comprehend.py:249:9:</a> PLW0201 Attribute `op` defined outside `__init__`
+ <a href='https://github.com/apache/airflow/blob/d8b58471f9812127ea1955c8163af3747f1cbe25/providers/amazon/tests/unit/amazon/aws/operators/test_glue.py#L619'>providers/amazon/tests/unit/amazon/aws/operators/test_glue.py:619:9:</a> PLW0201 Attribute `op` defined outside `__init__`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/a5282b6b8d7a64c5a9b85cc54223af22b58ea883/samcli/lib/sync/watch_manager.py#L196'>samcli/lib/sync/watch_manager.py:196:9:</a> PLW0201 Attribute `_infra_sync_executor` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/state.py#L191'>src/bokeh/io/state.py:191:9:</a> PLW0201 Attribute `notebook_type` defined outside `__init__`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_autocomplete_input.py#L396'>tests/integration/widgets/test_autocomplete_input.py:396:17:</a> PLW0201 Attribute `new` defined outside `__init__`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/embed/test_util__embed.py#L91'>tests/unit/bokeh/embed/test_util__embed.py:91:9:</a> PLW0201 Attribute `event` defined outside `__init__`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_callback_manager.py#L81'>tests/unit/bokeh/util/test_callback_manager.py:81:9:</a> PLW0201 Attribute `event` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/freedomofpress/securedrop">freedomofpress/securedrop</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/freedomofpress/securedrop/blob/a7f63b0431acc9f0b1ffab3ce833d592b6cf9003/securedrop/tests/migrations/migration_b58139cfdc8c.py#L50'>securedrop/tests/migrations/migration_b58139cfdc8c.py:50:9:</a> PLW0201 Attribute `source_filesystem_id` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L100'>ibis/backends/impala/metadata.py:100:9:</a> PLW0201 Attribute `info` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L101'>ibis/backends/impala/metadata.py:101:9:</a> PLW0201 Attribute `storage` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L108'>ibis/backends/impala/metadata.py:108:9:</a> PLW0201 Attribute `pos` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L120'>ibis/backends/impala/metadata.py:120:9:</a> PLW0201 Attribute `schema` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L129'>ibis/backends/impala/metadata.py:129:9:</a> PLW0201 Attribute `partitions` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L156'>ibis/backends/impala/metadata.py:156:9:</a> PLW0201 Attribute `info` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L205'>ibis/backends/impala/metadata.py:205:9:</a> PLW0201 Attribute `storage` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L97'>ibis/backends/impala/metadata.py:97:9:</a> PLW0201 Attribute `pos` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L98'>ibis/backends/impala/metadata.py:98:9:</a> PLW0201 Attribute `schema` defined outside `__init__`
+ <a href='https://github.com/ibis-project/ibis/blob/fc1b6a73820b98768568a65555d5a88748e32f1b/ibis/backends/impala/metadata.py#L99'>ibis/backends/impala/metadata.py:99:9:</a> PLW0201 Attribute `partitions` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/00248d343ae405296ce56ac476364ce4fc11a489/src/latch_cli/centromere/utils.py#L289'>src/latch_cli/centromere/utils.py:289:9:</a> PLW0201 Attribute `_tempdir` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/cd611d70d9a0b6a676369b6133c1caddc7a2dfe7/pymilvus/client/grpc_handler.py#L269'>pymilvus/client/grpc_handler.py:269:9:</a> PLW0201 Attribute `_identifier` defined outside `__init__`
+ <a href='https://github.com/milvus-io/pymilvus/blob/cd611d70d9a0b6a676369b6133c1caddc7a2dfe7/pymilvus/client/grpc_handler.py#L270'>pymilvus/client/grpc_handler.py:270:9:</a> PLW0201 Attribute `_identifier_interceptor` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/pandas/io/formats/style_render.py#L341'>pandas/io/formats/style_render.py:341:9:</a> PLW0201 Attribute `cellstyle_map_columns` defined outside `__init__`
+ <a href='https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/pandas/io/formats/style_render.py#L353'>pandas/io/formats/style_render.py:353:9:</a> PLW0201 Attribute `cellstyle_map` defined outside `__init__`
+ <a href='https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/pandas/io/formats/style_render.py#L356'>pandas/io/formats/style_render.py:356:9:</a> PLW0201 Attribute `cellstyle_map_index` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/data_handler.py#L111'>rotkehlchen/data_handler.py:111:9:</a> PLW0201 Attribute `db` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/data_handler.py#L119'>rotkehlchen/data_handler.py:119:9:</a> PLW0201 Attribute `user_data_dir` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/data_migrations/manager.py#L58'>rotkehlchen/data_migrations/manager.py:58:9:</a> PLW0201 Attribute `progress_handler` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/exchanges/manager.py#L285'>rotkehlchen/exchanges/manager.py:285:9:</a> PLW0201 Attribute `database` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/rotkehlchen.py#L309'>rotkehlchen/rotkehlchen.py:309:9:</a> PLW0201 Attribute `user_directory` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/rotkehlchen.py#L319'>rotkehlchen/rotkehlchen.py:319:9:</a> PLW0201 Attribute `data_importer` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/rotkehlchen.py#L320'>rotkehlchen/rotkehlchen.py:320:9:</a> PLW0201 Attribute `premium_sync_manager` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/rotkehlchen.py#L379'>rotkehlchen/rotkehlchen.py:379:9:</a> PLW0201 Attribute `chains_aggregator` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/rotkehlchen.py#L480'>rotkehlchen/rotkehlchen.py:480:9:</a> PLW0201 Attribute `accountant` defined outside `__init__`
+ <a href='https://github.com/rotki/rotki/blob/cf638ac12eab573fff2486aa24e04b6e00cabf4e/rotkehlchen/rotkehlchen.py#L486'>rotkehlchen/rotkehlchen.py:486:9:</a> PLW0201 Attribute `history_querying_manager` defined outside `__init__`
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/modules/events/util.py#L350'>indico/modules/events/util.py:350:9:</a> PLW0201 Attribute `list_config` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L101'>indico/web/http_api/hooks/base.py:101:9:</a> PLW0201 Attribute `_offset` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L104'>indico/web/http_api/hooks/base.py:104:9:</a> PLW0201 Attribute `_orderBy` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L105'>indico/web/http_api/hooks/base.py:105:9:</a> PLW0201 Attribute `_descending` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L106'>indico/web/http_api/hooks/base.py:106:9:</a> PLW0201 Attribute `_detail` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L116'>indico/web/http_api/hooks/base.py:116:9:</a> PLW0201 Attribute `_userLimit` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L122'>indico/web/http_api/hooks/base.py:122:9:</a> PLW0201 Attribute `_limit` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L133'>indico/web/http_api/hooks/base.py:133:9:</a> PLW0201 Attribute `_fromDT` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/hooks/base.py#L134'>indico/web/http_api/hooks/base.py:134:9:</a> PLW0201 Attribute `_toDT` defined outside `__init__`
+ <a href='https://github.com/indico/indico/blob/bc983ee0e4c6cf6b6aee3a4f4346fbd858ebf093/indico/web/http_api/metadata/serializer.py#L48'>indico/web/http_api/metadata/serializer.py:48:9:</a> PLW0201 Attribute `_obj` defined outside `__init__`
... 1 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9f9b7f45df8bec61374e92436fade632cb47252d/src/_pytest/junitxml.py#L258'>src/_pytest/junitxml.py:258:9:</a> PLW0201 Attribute `to_xml` defined outside `__init__`
+ <a href='https://github.com/pytest-dev/pytest/blob/9f9b7f45df8bec61374e92436fade632cb47252d/src/_pytest/junitxml.py#L637'>src/_pytest/junitxml.py:637:9:</a> PLW0201 Attribute `suite_start` defined outside `__init__`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/convolution/core.py#L125'>astropy/convolution/core.py:125:9:</a> PLW0201 Attribute `_kernel_sum` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/io/ascii/core.py#L593'>astropy/io/ascii/core.py:593:9:</a> PLW0201 Attribute `cols` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/io/ascii/core.py#L690'>astropy/io/ascii/core.py:690:9:</a> PLW0201 Attribute `cols` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/samp/hub.py#L263'>astropy/samp/hub.py:263:9:</a> PLW0201 Attribute `_server` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/samp/hub.py#L273'>astropy/samp/hub.py:273:9:</a> PLW0201 Attribute `_url` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/samp/hub.py#L389'>astropy/samp/hub.py:389:9:</a> PLW0201 Attribute `_hub_private_key` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/samp/hub_proxy.py#L50'>astropy/samp/hub_proxy.py:50:9:</a> PLW0201 Attribute `lockfile` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/samp/hub_proxy.py#L93'>astropy/samp/hub_proxy.py:93:9:</a> PLW0201 Attribute `lockfile` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/time/tests/test_methods.py#L70'>astropy/time/tests/test_methods.py:70:9:</a> PLW0201 Attribute `t0` defined outside `__init__`
+ <a href='https://github.com/astropy/astropy/blob/952729eb6176e328d76e2ed162ed2e778dd84b76/astropy/time/tests/test_methods.py#L71'>astropy/time/tests/test_methods.py:71:9:</a> PLW0201 Attribute `t1` defined outside `__init__`
... 8 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0201 | 109 | 109 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @jack-mcivor on `crates/ruff_linter/resources/test/fixtures/pylint/attribute_defined_outside_init.py`:116 on 2025-06-19 17:31_

pylint does trigger on this. We could as well, but I'm not sure where we should draw the line in what is allowed to be inherited from (built-ins maybe?)

---

_@jack-mcivor reviewed on 2025-06-19 17:31_

---

_Comment by @jack-mcivor on 2025-06-19 17:38_

Not sure if we should follow calls inside the init and allow these. Pylint does allow this, so I guess so. For example:
```python
class Student:
    def __init__(self):
        self._setup()
    
    def _setup(self):
        self.is_registered = False  # allowed, this is called in init

    def register(self):
        self.is_registered = True  # not allowed
```

---

_Comment by @jack-mcivor on 2025-06-19 17:47_

Ah 

- [x] need to allow a few methods to do setting aside from init - https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#defining-attr-methods
- [x] need to allow property setters
- [x] follow method calls in init and allow setting

---

_Converted to draft by @jack-mcivor on 2025-06-19 18:40_

---

_Comment by @jack-mcivor on 2025-06-19 19:34_

- [x]  TODO: this is triggering and probably shouldn't be. pylint definitely triggers on these, but I'm a bit scared of the 2k violations - think maybe it's best to start quite lenient
 
```python
class Student:
    def __init__(self):
        self._setup()

    def _setup(self, reset=True):
        if reset:
            self.is_registered = False  # ? make allowed

    def register(self):
        self.is_registered = True  # ? make allowed
```
edit: now walk any method called from a defining-attr-method, and track assignments in branches (even if the branch isn't run)

---

_@jack-mcivor reviewed on 2025-06-20 21:03_

---

_Review comment by @jack-mcivor on `crates/ruff_linter/resources/test/fixtures/pylint/attribute_defined_outside_init.py`:1 on 2025-06-20 21:03_

TODO: clean-up tests, so it's clear what this covers vs. what pylint covers

---

_@mangolemur reviewed on 2025-11-20 16:59_

---

_Review comment by @mangolemur on `crates/ruff_linter/resources/test/fixtures/pylint/attribute_defined_outside_init.py`:116 on 2025-11-20 16:59_

Does this pass all cases where the class has a parent? Is it possible to look at the parent class to see if its defined? It seems that pylint looks at the parent class.

---

_Comment by @MichaReiser on 2025-12-10 17:34_

I'll close this PR as it's pretty old. @jack-mcivor feel free to open a new PR once rebased if you plan to keep working on this.

---

_Closed by @MichaReiser on 2025-12-10 17:34_

---

_Comment by @jack-mcivor on 2025-12-10 18:08_

@MichaReiser is it worthwhile continuing on this? My plan was to be more lenient than the pylint rule for now. In particular:

- ignore classes with inheritance
- ignore classes that are decorated
- ignore classes that don't have a init method (actually any "defining-attr-methods"). This is easy to support, but leads to a large number of violations in the ecosystem where setup code is deferred (eg. https://github.com/pandas-dev/pandas/blob/592a41a1a504af38f235557b2a1b16228d8eda0a/asv_bench/benchmarks/algorithms.py#L198-L207)
- support a larger set of defining-attr-methods by default (["__init__", "__new__", "setUp", "asyncSetUp", "__post_init__", "setup_class", "setup_method", "__enter__", "__aenter__"])

Is there any interest in implementing this with the conditions above? Or are there any suggestions?

---
