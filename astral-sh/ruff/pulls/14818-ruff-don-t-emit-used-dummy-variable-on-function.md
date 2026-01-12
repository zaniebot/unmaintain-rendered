```yaml
number: 14818
title: "[`ruff`] Don't emit `used-dummy-variable` on function parameters (`RUF052`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: alex/ruf052-parameters
created_at: 2024-12-06T13:56:09Z
updated_at: 2024-12-06T14:54:45Z
url: https://github.com/astral-sh/ruff/pull/14818
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Don't emit `used-dummy-variable` on function parameters (`RUF052`)

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/14796.
Fixes https://github.com/astral-sh/ruff/issues/14790.
Fixes https://github.com/astral-sh/ruff/issues/14799.

There isn't nearly as much community consensus around the idea that using a leading underscore for a function parameter indicates the binding is meant to be "unused"; many people use this to indicate "private" parameters. Even if we decide that we don't believe that private parameters should be recognised as a concept by Ruff, I think emitting this lint on function parameters would still need to be behind a configuration flag (or maybe even a separate rule altogether), as it's just a lot more controversial than the other changes this rule makes IMO.

I agree with @zanieb's comment in https://github.com/astral-sh/ruff/issues/14796#issuecomment-2521618519 that the logical conclusion of this argument is that we should probably look into whether we should change the behaviour for `ARG001`. Unfortunately we'll only be able to do that in the next minor release, however, as it would be an increase in the scope of `ARG001`. I can open an issue for that after this PR is merged, so that we don't forget.

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `bug` added by @AlexWaygood on 2024-12-06 13:56_

---

_Label `preview` added by @AlexWaygood on 2024-12-06 13:56_

---

_Renamed from "[`ruff`] Dont emit RUF052 on function parameters" to "[`ruff`] Dont emit `used-dummy-variable` on function parameters (`RUF052`)" by @AlexWaygood on 2024-12-06 13:56_

---

_Comment by @github-actions[bot] on 2024-12-06 14:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -256 violations, +0 -0 fixes in 15 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -5 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/converter.py#L1189'>disnake/ext/commands/converter.py:1189:33:</a> RUF052 [*] Local dummy variable `_GenericAlias` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/help.py#L211'>disnake/ext/commands/help.py:211:37:</a> RUF052 [*] Local dummy variable `_original` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/help.py#L217'>disnake/ext/commands/help.py:217:38:</a> RUF052 [*] Local dummy variable `_original` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/utils.py#L691'>disnake/utils.py:691:35:</a> RUF052 [*] Local dummy variable `_IS_ASCII` is accessed
- <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/scripts/codemods/base.py#L31'>scripts/codemods/base.py:31:25:</a> RUF052 [*] Local dummy variable `_orig` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -28 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/training/interactive.py#L524'>rasa/core/training/interactive.py:524:19:</a> RUF052 [*] Local dummy variable `_table` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/training/interactive.py#L527'>rasa/core/training/interactive.py:527:20:</a> RUF052 [*] Local dummy variable `_table` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/importers/multi_project.py#L80'>rasa/shared/importers/multi_project.py:80:31:</a> RUF052 [*] Local dummy variable `_dict` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/training_data.py#L598'>rasa/shared/nlu/training_data/training_data.py:598:13:</a> RUF052 [*] Local dummy variable `_examples` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/training_data.py#L598'>rasa/shared/nlu/training_data/training_data.py:598:39:</a> RUF052 [*] Local dummy variable `_running_count` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/nlu/training_data/training_data.py#L598'>rasa/shared/nlu/training_data/training_data.py:598:60:</a> RUF052 [*] Local dummy variable `_running_train_count` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/telemetry.py#L601'>rasa/telemetry.py:601:29:</a> RUF052 [*] Local dummy variable `_unused_hint` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/telemetry.py#L910'>rasa/telemetry.py:910:9:</a> RUF052 [*] Local dummy variable `_model_directory` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/crf.py#L118'>rasa/utils/tensorflow/crf.py:118:18:</a> RUF052 [*] Local dummy variable `_state` is accessed
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/utils/tensorflow/crf.py#L118'>rasa/utils/tensorflow/crf.py:118:38:</a> RUF052 [*] Local dummy variable `_inputs` is accessed
... 18 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -77 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/api_fastapi/common/parameters.py#L284'>airflow/api_fastapi/common/parameters.py:284:5:</a> RUF052 [*] Local dummy variable `_type` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/configuration.py#L887'>airflow/configuration.py:887:9:</a> RUF052 [*] Local dummy variable `_extra_stacklevel` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/decorators/setup_teardown.py#L42'>airflow/decorators/setup_teardown.py:42:19:</a> RUF052 [*] Local dummy variable `_func` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/sentry.py#L167'>airflow/sentry.py:167:25:</a> RUF052 [*] Local dummy variable `_self` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/settings.py#L383'>airflow/settings.py:383:28:</a> RUF052 [*] Local dummy variable `_engine` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/utils/hashlib_wrapper.py#L27'>airflow/utils/hashlib_wrapper.py:27:9:</a> RUF052 [*] Local dummy variable `__string` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/airflow/utils/retries.py#L59'>airflow/utils/retries.py:59:26:</a> RUF052 [*] Local dummy variable `_func` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L391'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:391:5:</a> RUF052 [*] Local dummy variable `_dir_exists_warn_and_should_skip` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/dev/breeze/src/airflow_breeze/utils/run_utils.py#L103'>dev/breeze/src/airflow_breeze/utils/run_utils.py:103:25:</a> RUF052 [*] Local dummy variable `_index` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/dev/breeze/src/airflow_breeze/utils/run_utils.py#L103'>dev/breeze/src/airflow_breeze/utils/run_utils.py:103:38:</a> RUF052 [*] Local dummy variable `_arg` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/dev/breeze/src/airflow_breeze/utils/run_utils.py#L120'>dev/breeze/src/airflow_breeze/utils/run_utils.py:120:38:</a> RUF052 [*] Local dummy variable `_argument` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/dev/example_dags/update_example_dags_paths.py#L93'>dev/example_dags/update_example_dags_paths.py:93:18:</a> RUF052 [*] Local dummy variable `_file` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/docs/exts/operators_and_hooks_ref.py#L257'>docs/exts/operators_and_hooks_ref.py:257:34:</a> RUF052 [*] Local dummy variable `_file_path` is accessed
- <a href='https://github.com/apache/airflow/blob/3d421f78d7046474c5684580a744f87160378935/docs/exts/operators_and_hooks_ref.py#L257'>docs/exts/operators_and_hooks_ref.py:257:59:</a> RUF052 [*] Local dummy variable `_class_name` is accessed
... 63 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/2816a70af3ae0675110c8738246e97ce99c6f9be/superset/common/query_object_factory.py#L47'>superset/common/query_object_factory.py:47:9:</a> RUF052 [*] Local dummy variable `_datasource_dao` is accessed
- <a href='https://github.com/apache/superset/blob/2816a70af3ae0675110c8738246e97ce99c6f9be/superset/views/utils.py#L426'>superset/views/utils.py:426:31:</a> RUF052 [*] Local dummy variable `_self` is accessed
- <a href='https://github.com/apache/superset/blob/2816a70af3ae0675110c8738246e97ce99c6f9be/tests/example_data/data_loading/pandas/pands_data_loading_conf.py#L57'>tests/example_data/data_loading/pandas/pands_data_loading_conf.py:57:29:</a> RUF052 [*] Local dummy variable `_dict` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/has_props.py#L521'>src/bokeh/core/has_props.py:521:28:</a> RUF052 [*] Local dummy variable `_with_props` is accessed
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/standalone.py#L302'>src/bokeh/embed/standalone.py:302:15:</a> RUF052 [*] Local dummy variable `_always_new` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+0 -24 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L699'>ibis/backends/datafusion/__init__.py:699:31:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L732'>ibis/backends/datafusion/__init__.py:732:33:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L739'>ibis/backends/datafusion/__init__.py:739:33:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L746'>ibis/backends/datafusion/__init__.py:746:33:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L753'>ibis/backends/datafusion/__init__.py:753:40:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L760'>ibis/backends/datafusion/__init__.py:760:38:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L767'>ibis/backends/datafusion/__init__.py:767:37:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L774'>ibis/backends/datafusion/__init__.py:774:37:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/datafusion/__init__.py#L781'>ibis/backends/datafusion/__init__.py:781:47:</a> RUF052 [*] Local dummy variable `_conn` is accessed
- <a href='https://github.com/ibis-project/ibis/blob/51d90a707698baea49dd086d892c4afc2d8a7dba/ibis/backends/duckdb/__init__.py#L1753'>ibis/backends/duckdb/__init__.py:1753:51:</a> RUF052 [*] Local dummy variable `_conn` is accessed
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/wallets/helpers.py#L80'>tests/wallets/helpers.py:80:36:</a> RUF052 [*] Local dummy variable `_test_data` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/0c27f7ed28dc3ac6c51e0a75867ffb6e8d810e1e/rotkehlchen/serialization/schemas.py#L151'>rotkehlchen/serialization/schemas.py:151:15:</a> RUF052 [*] Local dummy variable `_kwargs` is accessed
- <a href='https://github.com/rotki/rotki/blob/0c27f7ed28dc3ac6c51e0a75867ffb6e8d810e1e/rotkehlchen/serialization/schemas.py#L213'>rotkehlchen/serialization/schemas.py:213:15:</a> RUF052 [*] Local dummy variable `_kwargs` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/skbuild/platform_specifics/abstract.py#L215'>skbuild/platform_specifics/abstract.py:215:45:</a> RUF052 [*] Local dummy variable `_generator` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L138'>src/scikit_build_core/settings/sources.py:138:21:</a> RUF052 [*] Local dummy variable `__target` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L155'>src/scikit_build_core/settings/sources.py:155:5:</a> RUF052 [*] Local dummy variable `__target` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L70'>src/scikit_build_core/settings/sources.py:70:17:</a> RUF052 [*] Local dummy variable `__dict` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L80'>src/scikit_build_core/settings/sources.py:80:21:</a> RUF052 [*] Local dummy variable `__dict` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/src/scikit_build_core/settings/sources.py#L86'>src/scikit_build_core/settings/sources.py:86:17:</a> RUF052 [*] Local dummy variable `__opt` is accessed
- <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/tests/test_dynamic_metadata.py#L58'>tests/test_dynamic_metadata.py:58:5:</a> RUF052 [*] Local dummy variable `_field` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/1847086044d484bb336874ac375b5468d7a1c021/zerver/actions/users.py#L478'>zerver/actions/users.py:478:32:</a> RUF052 [*] Local dummy variable `_cascade` is accessed
- <a href='https://github.com/zulip/zulip/blob/1847086044d484bb336874ac375b5468d7a1c021/zerver/lib/validator.py#L232'>zerver/lib/validator.py:232:5:</a> RUF052 [*] Local dummy variable `_allow_only_listed_keys` is accessed
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+0 -37 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/core/emails.py#L93'>indico/core/emails.py:93:42:</a> RUF052 [*] Local dummy variable `_from_task` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/core/marshmallow.py#L54'>indico/core/marshmallow.py:54:52:</a> RUF052 [*] Local dummy variable `_mro` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/core/permissions.py#L139'>indico/core/permissions.py:139:42:</a> RUF052 [*] Local dummy variable `_type` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/core/permissions.py#L62'>indico/core/permissions.py:62:26:</a> RUF052 [*] Local dummy variable `_type` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/modules/categories/operations.py#L57'>indico/modules/categories/operations.py:57:49:</a> RUF052 [*] Local dummy variable `_extra_log_fields` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/modules/categories/operations.py#L66'>indico/modules/categories/operations.py:66:51:</a> RUF052 [*] Local dummy variable `_extra_log_fields` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/modules/events/contributions/controllers/compat.py#L17'>indico/modules/events/contributions/controllers/compat.py:17:25:</a> RUF052 [*] Local dummy variable `_endpoint` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/modules/events/export.py#L431'>indico/modules/events/export.py:431:34:</a> RUF052 [*] Local dummy variable `_re` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/modules/events/models/events.py#L860'>indico/modules/events/models/events.py:860:35:</a> RUF052 [*] Local dummy variable `_for_sending` is accessed
- <a href='https://github.com/indico/indico/blob/aef5c498ca38e93496085cd601eea692ac1c2dbb/indico/modules/events/operations.py#L144'>indico/modules/events/operations.py:144:52:</a> RUF052 [*] Local dummy variable `_extra_log_fields` is accessed
... 27 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -40 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/_code/code.py#L443'>src/_pytest/_code/code.py:443:9:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/cacheprovider.py#L101'>src/_pytest/cacheprovider.py:101:50:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/cacheprovider.py#L109'>src/_pytest/cacheprovider.py:109:33:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/cacheprovider.py#L70'>src/_pytest/cacheprovider.py:70:50:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/cacheprovider.py#L77'>src/_pytest/cacheprovider.py:77:44:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/cacheprovider.py#L89'>src/_pytest/cacheprovider.py:89:42:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/capture.py#L907'>src/_pytest/capture.py:907:9:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/config/argparsing.py#L354'>src/_pytest/config/argparsing.py:354:9:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/config/argparsing.py#L46'>src/_pytest/config/argparsing.py:46:9:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
- <a href='https://github.com/pytest-dev/pytest/blob/80da4274398617274ff9e576339184ce77893d9b/src/_pytest/fixtures.py#L1220'>src/_pytest/fixtures.py:1220:29:</a> RUF052 [*] Local dummy variable `_ispytest` is accessed
... 30 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pdm-project/pdm">pdm-project/pdm</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pdm-project/pdm/blob/e4c5b2f57c1563cb1f82f52eaba667cd81d24d75/src/pdm/models/backends.py#L78'>src/pdm/models/backends.py:78:26:</a> RUF052 [*] Local dummy variable `__format_spec` is accessed
- <a href='https://github.com/pdm-project/pdm/blob/e4c5b2f57c1563cb1f82f52eaba667cd81d24d75/src/pdm/models/backends.py#L92'>src/pdm/models/backends.py:92:26:</a> RUF052 [*] Local dummy variable `__format_spec` is accessed
- <a href='https://github.com/pdm-project/pdm/blob/e4c5b2f57c1563cb1f82f52eaba667cd81d24d75/src/pdm/project/core.py#L609'>src/pdm/project/core.py:609:96:</a> RUF052 [*] Local dummy variable `_kwds` is accessed
- <a href='https://github.com/pdm-project/pdm/blob/e4c5b2f57c1563cb1f82f52eaba667cd81d24d75/src/pdm/pytest.py#L214'>src/pdm/pytest.py:214:27:</a> RUF052 [*] Local dummy variable `__key` is accessed
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF052 | 255 | 0 | 255 | 0 | 0 |
| DOC501 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Renamed from "[`ruff`] Dont emit `used-dummy-variable` on function parameters (`RUF052`)" to "[`ruff`] Don't emit `used-dummy-variable` on function parameters (`RUF052`)" by @AlexWaygood on 2024-12-06 14:03_

---

_@zanieb approved on 2024-12-06 14:44_

---

_Merged by @AlexWaygood on 2024-12-06 14:54_

---

_Closed by @AlexWaygood on 2024-12-06 14:54_

---

_Branch deleted on 2024-12-06 14:54_

---
