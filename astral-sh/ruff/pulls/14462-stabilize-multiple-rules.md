```yaml
number: 14462
title: Stabilize multiple rules
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: ruff-0.8
head: micha/ruff-0.8-stabilize-rules
created_at: 2024-11-19T14:39:32Z
updated_at: 2024-11-20T15:42:18Z
url: https://github.com/astral-sh/ruff/pull/14462
synced_at: 2026-01-10T20:50:57Z
```

# Stabilize multiple rules

---

_Pull request opened by @MichaReiser on 2024-11-19 14:39_

- **Stabilize `PLC0206`**
- **Stabilize `B039`**
- **Stabilize `UP043`**
- **Stabilize `PYI063`**
- **Stabilize `PYI064`**
- **Stabilize `PYI066`**
- **Stabilize `FAST001` and `FAST002`**
- **Stabilize `RUF030`**



---

_Review requested from @carljm by @MichaReiser on 2024-11-19 14:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-19 14:39_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-19 14:39_

---

_Label `rule` added by @MichaReiser on 2024-11-19 14:39_

---

_Review request for @carljm removed by @AlexWaygood on 2024-11-19 14:52_

---

_Review request for @sharkdp removed by @AlexWaygood on 2024-11-19 14:52_

---

_Comment by @github-actions[bot] on 2024-11-19 14:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+54 -0 violations, +0 -0 fixes in 15 projects; 39 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/ac5a936d187ff89d6756e5f33bf0b2965e7b7685/disnake/webhook/async_.py#L645'>disnake/webhook/async_.py:645:38:</a> B039 Do not use mutable data structures for `ContextVar` defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/particles/_special_particles.py#L260'>src/plasmapy/particles/_special_particles.py:260:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/particles/decorators.py#L677'>src/plasmapy/particles/decorators.py:677:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/particles/ionization_state_collection.py#L479'>src/plasmapy/particles/ionization_state_collection.py:479:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/0b8f11cb46078eacb8cf33a5758fbe8f74b178ff/src/plasmapy/plasma/grids.py#L769'>src/plasmapy/plasma/grids.py:769:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/airflow/utils/hashlib_wrapper.py#L27'>airflow/utils/hashlib_wrapper.py:27:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/airflow/www/views.py#L2228'>airflow/www/views.py:2228:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/docs/conf.py#L468'>docs/conf.py:468:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/performance/src/performance_dags/performance_dag/performance_dag_utils.py#L137'>performance/src/performance_dags/performance_dag/performance_dag_utils.py:137:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/providers/tests/amazon/aws/executors/batch/test_batch_executor.py#L667'>providers/tests/amazon/aws/executors/batch/test_batch_executor.py:667:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py#L1234'>providers/tests/amazon/aws/executors/ecs/test_ecs_executor.py:1234:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/airflow/blob/e8fe1bddf1bf8ed4c4ce94fa6efd713be02fb6bc/tests/api_connexion/endpoints/test_task_instance_endpoint.py#L654'>tests/api_connexion/endpoints/test_task_instance_endpoint.py:654:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 4 additional changes omitted for rule PLC0206
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/RELEASING/changelog.py#L227'>RELEASING/changelog.py:227:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/superset/jinja_context.py#L483'>superset/jinja_context.py:483:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/apache/superset/blob/d22b7860a47de0950df82de7a67ec7d3dbd324ff/tests/integration_tests/reports/api_tests.py#L303'>tests/integration_tests/reports/api_tests.py:303:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/aws/aws-sam-cli">aws/aws-sam-cli</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/aws/aws-sam-cli/blob/4d0e244565c253002cbad8232679b1d1573a8276/tests/integration/pipeline/test_bootstrap_command.py#L229'>tests/integration/pipeline/test_bootstrap_command.py:229:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code.py#L201'>src/bokeh/application/handlers/code.py:201:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/property/wrappers.py#L470'>src/bokeh/core/property/wrappers.py:470:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/5599a0a537802a2b9083ee7d7e50311167b42f33/libs/core/langchain_core/runnables/config.py#L114'>libs/core/langchain_core/runnables/config.py:114:38:</a> B039 Do not use mutable data structures for `ContextVar` defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L183'>src/latch/functions/operators.py:183:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L37'>src/latch/functions/operators.py:37:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L48'>src/latch/functions/operators.py:48:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L59'>src/latch/functions/operators.py:59:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L68'>src/latch/functions/operators.py:68:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/functions/operators.py#L73'>src/latch/functions/operators.py:73:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/requestvars.py#L5'>lnbits/requestvars.py:5:31:</a> B039 Do not use mutable data structures for `ContextVar` defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/24cba21ea49f75bbcfd3e85a261412329345e8ea/pymilvus/bulk_writer/buffer.py#L96'>pymilvus/bulk_writer/buffer.py:96:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/_typing.py#L276'>pandas/_typing.py:276:20:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/_typing.py#L291'>pandas/_typing.py:291:20:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/_typing.py#L297'>pandas/_typing.py:297:21:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/6a7685faf104f8582e0e75f1fae58e09ae97e2fe/pandas/io/stata.py#L2209'>pandas/io/stata.py:2209:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/tiangolo/fastapi/blob/1f7629d3e421493ef9539ea7288564584c0182be/docs_src/sql_databases_peewee/sql_app/database.py#L7'>docs_src/sql_databases_peewee/sql_app/database.py:7:43:</a> B039 Do not use mutable data structures for `ContextVar` defaults
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+16 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/corporate/views/installation_activity.py#L82'>corporate/views/installation_activity.py:82:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/tools/setup/emoji/emoji_setup_utils.py#L90'>tools/setup/emoji/emoji_setup_utils.py:90:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/actions/realm_settings.py#L268'>zerver/actions/realm_settings.py:268:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/data_import/mattermost.py#L163'>zerver/data_import/mattermost.py:163:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/data_import/mattermost.py#L847'>zerver/data_import/mattermost.py:847:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/data_import/rocketchat.py#L169'>zerver/data_import/rocketchat.py:169:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/data_import/rocketchat.py#L217'>zerver/data_import/rocketchat.py:217:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/data_import/rocketchat.py#L252'>zerver/data_import/rocketchat.py:252:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/data_import/rocketchat.py#L67'>zerver/data_import/rocketchat.py:67:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/lib/cache.py#L241'>zerver/lib/cache.py:241:5:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/lib/markdown/api_return_values_table_generator.py#L187'>zerver/lib/markdown/api_return_values_table_generator.py:187:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/migrations/0382_create_role_based_system_groups.py#L59'>zerver/migrations/0382_create_role_based_system_groups.py:59:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/migrations/0403_create_role_based_groups_for_internal_realms.py#L68'>zerver/migrations/0403_create_role_based_groups_for_internal_realms.py:68:9:</a> PLC0206 Extracting value from dictionary without calling `.items()`
+ <a href='https://github.com/zulip/zulip/blob/666c1e1d951e995cbf2432860840cfcb30284b36/zerver/tests/test_events.py#L482'>zerver/tests/test_events.py:482:13:</a> PLC0206 Extracting value from dictionary without calling `.items()`
... 2 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/f724be58edaf1fd5b0671ba0da8292ff137c0656/src/trio/testing/_fake_net.py#L497'>src/trio/testing/_fake_net.py:497:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/f724be58edaf1fd5b0671ba0da8292ff137c0656/src/trio/testing/_fake_net.py#L505'>src/trio/testing/_fake_net.py:505:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/72f17d10d76e5260906c17f279c5fa6bd29b5a8d/testing/example_scripts/dataclasses/test_compare_dataclasses_with_custom_eq.py#L13'>testing/example_scripts/dataclasses/test_compare_dataclasses_with_custom_eq.py:13:26:</a> PYI063 Use PEP 570 syntax for positional-only arguments
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLC0206 | 43 | 43 | 0 | 0 | 0 |
| PYI063 | 7 | 7 | 0 | 0 | 0 |
| B039 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-11-19 15:01_

I skimmed through the ecosystem report and couldn't see any false positives or places where the rules were recommending something obviously undesirable. These have all been in preview for a while, and there are no open issues about them.

---

_Merged by @AlexWaygood on 2024-11-19 15:03_

---

_Closed by @AlexWaygood on 2024-11-19 15:03_

---

_Branch deleted on 2024-11-19 15:03_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-20 15:42_

---
