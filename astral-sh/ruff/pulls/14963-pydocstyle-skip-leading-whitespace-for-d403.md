```yaml
number: 14963
title: "[`pydocstyle`] Skip leading whitespace for `D403`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: docstring-capitalize
created_at: 2024-12-13T22:19:02Z
updated_at: 2024-12-16T15:09:28Z
url: https://github.com/astral-sh/ruff/pull/14963
synced_at: 2026-01-10T20:42:27Z
```

# [`pydocstyle`] Skip leading whitespace for `D403`

---

_Pull request opened by @dylwil3 on 2024-12-13 22:19_

This PR introduces three changes to `D403`, which has to do with capitalizing the first word in a docstring.

1. The diagnostic and fix now skip leading whitespace when determining what counts as "the first word".
2. The name has been changed to `first-word-uncapitalized` from `first-line-capitalized`, for both clarity and compliance with our rule naming policy.
3. The diagnostic message and documentation has been modified slightly to reflect this.

Closes #14890


---

_Comment by @dylwil3 on 2024-12-13 22:21_

Two questions for reviewers:

1. I used `trim_start` (which trims unicode-defined whitespace) rather than `trim_whitespace_start` (which trims "Python" whitespace) since we were inside a docstring. But let me know if I should change that.
2. Is this enough of a rule change that it should not be merged until a minor release? (Added later: Haven't looked at them all, but the ecosystem results look correct to me. There are a lot of new violations, so maybe that's a factor in answering this question.)

---

_Comment by @github-actions[bot] on 2024-12-13 22:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+48 -11 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+5 -4 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_broker.py#L309'>tests/core/test_broker.py:309:5:</a> D403 [*] First word of the docstring should be capitalized: `tests` -> `Tests`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L1025'>tests/nlu/classifiers/test_diet_classifier.py:1025:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L1025'>tests/nlu/classifiers/test_diet_classifier.py:1025:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L504'>tests/nlu/classifiers/test_diet_classifier.py:504:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L504'>tests/nlu/classifiers/test_diet_classifier.py:504:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L953'>tests/nlu/classifiers/test_diet_classifier.py:953:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L953'>tests/nlu/classifiers/test_diet_classifier.py:953:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L995'>tests/nlu/classifiers/test_diet_classifier.py:995:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L995'>tests/nlu/classifiers/test_diet_classifier.py:995:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/e8f7ada2fa9faa5e8fcdf904908160285f137124/src/plasmapy/diagnostics/thomson.py#L684'>src/plasmapy/diagnostics/thomson.py:684:5:</a> D403 [*] First word of the docstring should be capitalized: `lmfit` -> `Lmfit`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/e8f7ada2fa9faa5e8fcdf904908160285f137124/tests/simulation/test_particle_tracker.py#L568'>tests/simulation/test_particle_tracker.py:568:9:</a> D403 [*] First word of the docstring should be capitalized: `fsolve` -> `Fsolve`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9d6801353c48569cfe0756f4cd0f02f0630e8baf/providers/src/airflow/providers/weaviate/hooks/weaviate.py#L751'>providers/src/airflow/providers/weaviate/hooks/weaviate.py:751:9:</a> D403 [*] First word of the docstring should be capitalized: `create` -> `Create`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+29 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/commands/importers/v1/__init__.py#L115'>superset/commands/importers/v1/__init__.py:115:9:</a> D403 [*] First word of the docstring should be capitalized: `check` -> `Check`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/commands/importers/v1/__init__.py#L115'>superset/commands/importers/v1/__init__.py:115:9:</a> D403 [*] First word of the first line should be capitalized: `check` -> `Check`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/common/utils/query_cache_manager.py#L204'>superset/common/utils/query_cache_manager.py:204:9:</a> D403 [*] First word of the docstring should be capitalized: `set` -> `Set`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/common/utils/time_range_utils.py#L52'>superset/common/utils/time_range_utils.py:52:5:</a> D403 [*] First word of the docstring should be capitalized: `this` -> `This`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L105'>superset/daos/tag.py:105:9:</a> D403 [*] First word of the docstring should be capitalized: `deletes` -> `Deletes`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L121'>superset/daos/tag.py:121:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L136'>superset/daos/tag.py:136:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L146'>superset/daos/tag.py:146:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L171'>superset/daos/tag.py:171:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L82'>superset/daos/tag.py:82:9:</a> D403 [*] First word of the docstring should be capitalized: `deletes` -> `Deletes`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/models/helpers.py#L455'>superset/models/helpers.py:455:9:</a> D403 [*] First word of the docstring should be capitalized: `object` -> `Object`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/models/helpers.py#L455'>superset/models/helpers.py:455:9:</a> D403 [*] First word of the first line should be capitalized: `object` -> `Object`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/sql_lab.py#L158'>superset/sql_lab.py:158:5:</a> D403 [*] First word of the docstring should be capitalized: `attempts` -> `Attempts`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/sql_lab.py#L158'>superset/sql_lab.py:158:5:</a> D403 [*] First word of the first line should be capitalized: `attempts` -> `Attempts`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/sql_parse.py#L147'>superset/sql_parse.py:147:5:</a> D403 [*] First word of the docstring should be capitalized: `parse` -> `Parse`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/utils/json.py#L234'>superset/utils/json.py:234:5:</a> D403 [*] First word of the docstring should be capitalized: `deserializable` -> `Deserializable`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/utils/pandas_postprocessing/resample.py#L32'>superset/utils/pandas_postprocessing/resample.py:32:5:</a> D403 [*] First word of the docstring should be capitalized: `support` -> `Support`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/import_export_tests.py#L210'>tests/integration_tests/import_export_tests.py:210:9:</a> D403 [*] First word of the docstring should be capitalized: `only` -> `Only`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/import_export_tests.py#L210'>tests/integration_tests/import_export_tests.py:210:9:</a> D403 [*] First word of the first line should be capitalized: `only` -> `Only`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py#L80'>tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py:80:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py#L91'>tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py:91:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L175'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:175:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L184'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:184:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L197'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:197:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L206'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:206:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L216'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:216:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L225'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:225:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/info.py#L84'>src/bokeh/command/subcommands/info.py:84:5:</a> D403 [*] First word of the docstring should be capitalized: `helper` -> `Helper`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokehjs_content.py#L132'>src/bokeh/sphinxext/bokehjs_content.py:132:9:</a> D403 [*] First word of the docstring should be capitalized: `this` -> `This`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokehjs_content.py#L132'>src/bokeh/sphinxext/bokehjs_content.py:132:9:</a> D403 [*] First word of the first line should be capitalized: `this` -> `This`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/screenshot.py#L87'>tests/support/util/screenshot.py:87:5:</a> D403 [*] First word of the docstring should be capitalized: `wait` -> `Wait`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_events.py#L144'>tests/unit/bokeh/test_events.py:144:5:</a> D403 [*] First word of the docstring should be capitalized: `model` -> `Model`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/resources/tasks.py#L283'>src/latch/resources/tasks.py:283:5:</a> D403 [*] First word of the docstring should be capitalized: `any` -> `Any`
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/resources/tasks.py#L283'>src/latch/resources/tasks.py:283:5:</a> D403 [*] First word of the first line should be capitalized: `any` -> `Any`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/lib/markdown/fenced_code.py#L562'>zerver/lib/markdown/fenced_code.py:562:9:</a> D403 [*] First word of the docstring should be capitalized: `basic` -> `Basic`
- <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/lib/markdown/fenced_code.py#L562'>zerver/lib/markdown/fenced_code.py:562:9:</a> D403 [*] First word of the first line should be capitalized: `basic` -> `Basic`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/lib/message_cache.py#L354'>zerver/lib/message_cache.py:354:9:</a> D403 [*] First word of the docstring should be capitalized: `row` -> `Row`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/tests/test_decorators.py#L817'>zerver/tests/test_decorators.py:817:9:</a> D403 [*] First word of the docstring should be capitalized: `logging` -> `Logging`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/tests/test_decorators.py#L830'>zerver/tests/test_decorators.py:830:9:</a> D403 [*] First word of the docstring should be capitalized: `logging` -> `Logging`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/tests/test_message_fetch.py#L3894'>zerver/tests/test_message_fetch.py:3894:9:</a> D403 [*] First word of the docstring should be capitalized: `narrow` -> `Narrow`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zproject/backends.py#L3075'>zproject/backends.py:3075:5:</a> D403 [*] First word of the docstring should be capitalized: `wantMessagesSigned` -> `WantMessagesSigned`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D403 | 59 | 48 | 11 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+48 -11 violations, +0 -0 fixes in 7 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+5 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/core/test_broker.py#L309'>tests/core/test_broker.py:309:5:</a> D403 [*] First word of the docstring should be capitalized: `tests` -> `Tests`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L1025'>tests/nlu/classifiers/test_diet_classifier.py:1025:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L1025'>tests/nlu/classifiers/test_diet_classifier.py:1025:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L504'>tests/nlu/classifiers/test_diet_classifier.py:504:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L504'>tests/nlu/classifiers/test_diet_classifier.py:504:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L953'>tests/nlu/classifiers/test_diet_classifier.py:953:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L953'>tests/nlu/classifiers/test_diet_classifier.py:953:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L995'>tests/nlu/classifiers/test_diet_classifier.py:995:5:</a> D403 [*] First word of the docstring should be capitalized: `test` -> `Test`
- <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/classifiers/test_diet_classifier.py#L995'>tests/nlu/classifiers/test_diet_classifier.py:995:5:</a> D403 [*] First word of the first line should be capitalized: `test` -> `Test`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/e8f7ada2fa9faa5e8fcdf904908160285f137124/src/plasmapy/diagnostics/thomson.py#L684'>src/plasmapy/diagnostics/thomson.py:684:5:</a> D403 [*] First word of the docstring should be capitalized: `lmfit` -> `Lmfit`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/e8f7ada2fa9faa5e8fcdf904908160285f137124/tests/simulation/test_particle_tracker.py#L568'>tests/simulation/test_particle_tracker.py:568:9:</a> D403 [*] First word of the docstring should be capitalized: `fsolve` -> `Fsolve`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/9d6801353c48569cfe0756f4cd0f02f0630e8baf/providers/src/airflow/providers/weaviate/hooks/weaviate.py#L751'>providers/src/airflow/providers/weaviate/hooks/weaviate.py:751:9:</a> D403 [*] First word of the docstring should be capitalized: `create` -> `Create`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+29 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/commands/importers/v1/__init__.py#L115'>superset/commands/importers/v1/__init__.py:115:9:</a> D403 [*] First word of the docstring should be capitalized: `check` -> `Check`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/commands/importers/v1/__init__.py#L115'>superset/commands/importers/v1/__init__.py:115:9:</a> D403 [*] First word of the first line should be capitalized: `check` -> `Check`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/common/utils/query_cache_manager.py#L204'>superset/common/utils/query_cache_manager.py:204:9:</a> D403 [*] First word of the docstring should be capitalized: `set` -> `Set`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/common/utils/time_range_utils.py#L52'>superset/common/utils/time_range_utils.py:52:5:</a> D403 [*] First word of the docstring should be capitalized: `this` -> `This`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L105'>superset/daos/tag.py:105:9:</a> D403 [*] First word of the docstring should be capitalized: `deletes` -> `Deletes`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L121'>superset/daos/tag.py:121:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L136'>superset/daos/tag.py:136:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L146'>superset/daos/tag.py:146:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L171'>superset/daos/tag.py:171:9:</a> D403 [*] First word of the docstring should be capitalized: `returns` -> `Returns`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/daos/tag.py#L82'>superset/daos/tag.py:82:9:</a> D403 [*] First word of the docstring should be capitalized: `deletes` -> `Deletes`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/models/helpers.py#L455'>superset/models/helpers.py:455:9:</a> D403 [*] First word of the docstring should be capitalized: `object` -> `Object`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/models/helpers.py#L455'>superset/models/helpers.py:455:9:</a> D403 [*] First word of the first line should be capitalized: `object` -> `Object`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/sql_lab.py#L158'>superset/sql_lab.py:158:5:</a> D403 [*] First word of the docstring should be capitalized: `attempts` -> `Attempts`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/sql_lab.py#L158'>superset/sql_lab.py:158:5:</a> D403 [*] First word of the first line should be capitalized: `attempts` -> `Attempts`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/sql_parse.py#L147'>superset/sql_parse.py:147:5:</a> D403 [*] First word of the docstring should be capitalized: `parse` -> `Parse`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/utils/json.py#L234'>superset/utils/json.py:234:5:</a> D403 [*] First word of the docstring should be capitalized: `deserializable` -> `Deserializable`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/superset/utils/pandas_postprocessing/resample.py#L32'>superset/utils/pandas_postprocessing/resample.py:32:5:</a> D403 [*] First word of the docstring should be capitalized: `support` -> `Support`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/import_export_tests.py#L210'>tests/integration_tests/import_export_tests.py:210:9:</a> D403 [*] First word of the docstring should be capitalized: `only` -> `Only`
- <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/import_export_tests.py#L210'>tests/integration_tests/import_export_tests.py:210:9:</a> D403 [*] First word of the first line should be capitalized: `only` -> `Only`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py#L80'>tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py:80:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py#L91'>tests/integration_tests/migrations/f1410ed7ec95_migrate_native_filters_to_new_schema__tests.py:91:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L175'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:175:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L184'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:184:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L197'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:197:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L206'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:206:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L216'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:216:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
+ <a href='https://github.com/apache/superset/blob/04077ce9342412a701c764310a467917df99c3bd/tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py#L225'>tests/integration_tests/migrations/f84fde59123a_update_charts_with_old_time_comparison__test.py:225:5:</a> D403 [*] First word of the docstring should be capitalized: `ensure` -> `Ensure`
... 6 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/info.py#L84'>src/bokeh/command/subcommands/info.py:84:5:</a> D403 [*] First word of the docstring should be capitalized: `helper` -> `Helper`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokehjs_content.py#L132'>src/bokeh/sphinxext/bokehjs_content.py:132:9:</a> D403 [*] First word of the docstring should be capitalized: `this` -> `This`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/bokehjs_content.py#L132'>src/bokeh/sphinxext/bokehjs_content.py:132:9:</a> D403 [*] First word of the first line should be capitalized: `this` -> `This`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/screenshot.py#L87'>tests/support/util/screenshot.py:87:5:</a> D403 [*] First word of the docstring should be capitalized: `wait` -> `Wait`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/test_events.py#L144'>tests/unit/bokeh/test_events.py:144:5:</a> D403 [*] First word of the docstring should be capitalized: `model` -> `Model`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/resources/tasks.py#L283'>src/latch/resources/tasks.py:283:5:</a> D403 [*] First word of the docstring should be capitalized: `any` -> `Any`
- <a href='https://github.com/latchbio/latch/blob/f986b146bce5cd9d915721edaf608d84c53d8648/src/latch/resources/tasks.py#L283'>src/latch/resources/tasks.py:283:5:</a> D403 [*] First word of the first line should be capitalized: `any` -> `Any`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+6 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/lib/markdown/fenced_code.py#L562'>zerver/lib/markdown/fenced_code.py:562:9:</a> D403 [*] First word of the docstring should be capitalized: `basic` -> `Basic`
- <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/lib/markdown/fenced_code.py#L562'>zerver/lib/markdown/fenced_code.py:562:9:</a> D403 [*] First word of the first line should be capitalized: `basic` -> `Basic`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/lib/message_cache.py#L354'>zerver/lib/message_cache.py:354:9:</a> D403 [*] First word of the docstring should be capitalized: `row` -> `Row`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/tests/test_decorators.py#L817'>zerver/tests/test_decorators.py:817:9:</a> D403 [*] First word of the docstring should be capitalized: `logging` -> `Logging`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/tests/test_decorators.py#L830'>zerver/tests/test_decorators.py:830:9:</a> D403 [*] First word of the docstring should be capitalized: `logging` -> `Logging`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zerver/tests/test_message_fetch.py#L3894'>zerver/tests/test_message_fetch.py:3894:9:</a> D403 [*] First word of the docstring should be capitalized: `narrow` -> `Narrow`
+ <a href='https://github.com/zulip/zulip/blob/a1121b39ad89bd6704434aeefaef1c476880aa9c/zproject/backends.py#L3075'>zproject/backends.py:3075:5:</a> D403 [*] First word of the docstring should be capitalized: `wantMessagesSigned` -> `WantMessagesSigned`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D403 | 59 | 48 | 11 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @dylwil3 on 2024-12-13 23:07_

---

_Label `breaking` added by @dylwil3 on 2024-12-15 15:53_

---

_Comment by @MichaReiser on 2024-12-15 15:54_

The change from first-line to first-word makes sense to me. Would you mind doing a quick check if this is "aligned" with what other docstyle rules do? For example D400, D401 also only consider the first line. We might need to review the rules more holistically

I don't think we should change the diagnostic location. Or at least, I suggest to do it as a separate PR because it's technically a breaking change and can invalidate all noqa comments.



---

_Comment by @dylwil3 on 2024-12-16 03:41_

>  Would you mind doing a quick check if this is "aligned" with what other docstyle rules do? For example D400, D401 also only consider the first line. We might need to review the rules more holistically

Good call!

These seem like the rules that should ignore leading whitespace, and it looks like their implementations indeed do (except for the one under review):

- [x] `ends-in-period` (`D400`) 
- [x] `non-imperative-mood` (`D401`)
- [x] `no-signature` (`D402`)
- [ ] `first-line-capitalized` (`D403`) [covered in this PR]
- [x] `docstring-starts-with-this` (`D404`)
- [x] `ends-in-punctuation` (`D415`)

> I don't think we should change the diagnostic location. Or at least, I suggest to do it as a separate PR because it's technically a breaking change and can invalidate all noqa comments.

Reverted.

---

_Label `breaking` removed by @dylwil3 on 2024-12-16 03:42_

---

_@MichaReiser approved on 2024-12-16 07:30_

Thanks

---

_Merged by @dylwil3 on 2024-12-16 15:09_

---

_Closed by @dylwil3 on 2024-12-16 15:09_

---
