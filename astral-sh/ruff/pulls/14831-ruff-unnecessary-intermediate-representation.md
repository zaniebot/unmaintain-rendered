```yaml
number: 14831
title: "[`ruff`] Unnecessary intermediate representation (`RUF042`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: RUF042
created_at: 2024-12-08T00:40:36Z
updated_at: 2025-04-28T07:07:03Z
url: https://github.com/astral-sh/ruff/pull/14831
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Unnecessary intermediate representation (`RUF042`)

---

_@InSyncWithFoo_

## Summary

Resolves #13533, resolves #14518 as well as the follow-up to #14564 and #14580.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-08 00:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+293 -0 violations, +0 -0 fixes in 24 projects; 31 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/noxfile.py#L112'>noxfile.py:112:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/DisnakeDev/disnake/blob/d847f505441d7ccf1ec4b63645ba8eae2cdc8882/tests/test_utils.py#L213'>tests/test_utils.py:213:5:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/channels/twilio.py#L58'>rasa/core/channels/twilio.py:58:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L131'>rasa/core/migrate.py:131:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L143'>rasa/core/migrate.py:143:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L177'>rasa/core/migrate.py:177:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L179'>rasa/core/migrate.py:179:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L181'>rasa/core/migrate.py:181:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L185'>rasa/core/migrate.py:185:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1898'>rasa/shared/core/domain.py:1898:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L1915'>rasa/shared/core/domain.py:1915:25:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/domain.py#L393'>rasa/shared/core/domain.py:393:13:</a> RUF042 [*] Unnecessary intermediate representation
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+98 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/cli/commands/local_commands/webserver_command.py#L401'>airflow/cli/commands/local_commands/webserver_command.py:401:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/abstractoperator.py#L299'>airflow/models/abstractoperator.py:299:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/dag.py#L1471'>airflow/models/dag.py:1471:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/taskinstance.py#L2018'>airflow/models/taskinstance.py:2018:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/taskinstance.py#L2020'>airflow/models/taskinstance.py:2020:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/taskinstance.py#L2022'>airflow/models/taskinstance.py:2022:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/taskinstance.py#L2028'>airflow/models/taskinstance.py:2028:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/taskinstance.py#L2030'>airflow/models/taskinstance.py:2030:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/models/taskinstance.py#L2034'>airflow/models/taskinstance.py:2034:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/serialization/serialized_objects.py#L1221'>airflow/serialization/serialized_objects.py:1221:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/serialization/serialized_objects.py#L1275'>airflow/serialization/serialized_objects.py:1275:25:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/serialization/serialized_objects.py#L1532'>airflow/serialization/serialized_objects.py:1532:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/airflow/utils/file.py#L214'>airflow/utils/file.py:214:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L2991'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:2991:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L594'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:594:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/airflow/blob/ee785a89ba27a59246cdfcc0d83129b691a39f3e/dev/breeze/src/airflow_breeze/utils/docker_command_utils.py#L346'>dev/breeze/src/airflow_breeze/utils/docker_command_utils.py:346:5:</a> RUF042 [*] Unnecessary intermediate representation
... 82 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+13 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/commands/chart/data/get_data_command.py#L62'>superset/commands/chart/data/get_data_command.py:62:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/databases/utils.py#L80'>superset/databases/utils.py:80:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/models/helpers.py#L1965'>superset/models/helpers.py:1965:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/models/helpers.py#L2016'>superset/models/helpers.py:2016:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/sql_lab.py#L512'>superset/sql_lab.py:512:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/sql_lab.py#L538'>superset/sql_lab.py:538:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/tasks/cache.py#L227'>superset/tasks/cache.py:227:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/views/core.py#L692'>superset/views/core.py:692:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/views/utils.py#L227'>superset/views/utils.py:227:25:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/apache/superset/blob/8960db4132409d563483cadd7311a30bf3f7b6d7/superset/viz.py#L1665'>superset/viz.py:1665:13:</a> RUF042 [*] Unnecessary intermediate representation
... 3 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L242'>src/bokeh/models/util/structure.py:242:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/models/util/structure.py#L243'>src/bokeh/models/util/structure.py:243:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/screenshot.py#L92'>tests/support/util/screenshot.py:92:9:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/b17c28d3af1d4897ff71d25a505d4bf3a1d94041/ibis/backends/sql/compilers/bigquery/__init__.py#L288'>ibis/backends/sql/compilers/bigquery/__init__.py:288:9:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+17 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L102'>lnbits/core/views/api.py:102:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L103'>lnbits/core/views/api.py:103:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L107'>lnbits/core/views/api.py:107:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L132'>lnbits/core/views/api.py:132:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L143'>lnbits/core/views/api.py:143:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L144'>lnbits/core/views/api.py:144:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L153'>lnbits/core/views/api.py:153:21:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L159'>lnbits/core/views/api.py:159:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L161'>lnbits/core/views/api.py:161:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/views/api.py#L162'>lnbits/core/views/api.py:162:17:</a> RUF042 [*] Unnecessary intermediate representation
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/milvus-io/pymilvus/blob/02e8eeec6ce18e2468d1b11906f53c5797edde68/examples/orm/collection.py#L163'>examples/orm/collection.py:163:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/milvus-io/pymilvus/blob/02e8eeec6ce18e2468d1b11906f53c5797edde68/examples/orm/partition.py#L98'>examples/orm/partition.py:98:9:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+24 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/doc/make.py#L140'>doc/make.py:140:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/apply.py#L1546'>pandas/core/apply.py:1546:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/apply.py#L744'>pandas/core/apply.py:744:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/computation/expressions.py#L87'>pandas/core/computation/expressions.py:87:21:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/config_init.py#L862'>pandas/core/config_init.py:862:5:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/generic.py#L3537'>pandas/core/generic.py:3537:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/generic.py#L3539'>pandas/core/generic.py:3539:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/indexes/api.py#L308'>pandas/core/indexes/api.py:308:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/core/indexes/period.py#L59'>pandas/core/indexes/period.py:59:1:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pandas-dev/pandas/blob/a4e814954b6f1c41528c071b028df62def7765c0/pandas/io/formats/style_render.py#L329'>pandas/io/formats/style_render.py:329:9:</a> RUF042 [*] Unnecessary intermediate representation
... 14 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/build">pypa/build</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/build/blob/88ae8330150f253dd70170368c311d6374315632/src/build/env.py#L300'>src/build/env.py:300:13:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/a96545b729c8d97ec068d265d9861762288c4144/bin/run_tests.py#L33'>bin/run_tests.py:33:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pypa/cibuildwheel/blob/a96545b729c8d97ec068d265d9861762288c4144/bin/run_tests.py#L54'>bin/run_tests.py:54:9:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/pypa/cibuildwheel/blob/a96545b729c8d97ec068d265d9861762288c4144/cibuildwheel/macos.py#L170'>cibuildwheel/macos.py:170:17:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/c01e731dd1fb6d6ae3c9ef00e2f19a89ab1ffb24/scripts/stubsabot.py#L445'>scripts/stubsabot.py:445:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/python/typeshed/blob/c01e731dd1fb6d6ae3c9ef00e2f19a89ab1ffb24/tests/stubtest_stdlib.py#L37'>tests/stubtest_stdlib.py:37:9:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/console/commands/run.py#L79'>src/poetry/console/commands/run.py:79:9:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/4da32a122baeb47093cc1ea07e4c02ea42351084/reflex/utils/exec.py#L364'>reflex/utils/exec.py:364:5:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/reflex-dev/reflex/blob/4da32a122baeb47093cc1ea07e4c02ea42351084/tests/integration/test_dynamic_routes.py#L289'>tests/integration/test_dynamic_routes.py:289:9:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/2dab3db35815be4ea1964a7e82a00221db514cdc/rotkehlchen/api/rest.py#L1964'>rotkehlchen/api/rest.py:1964:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/rotki/rotki/blob/2dab3db35815be4ea1964a7e82a00221db514cdc/rotkehlchen/exchanges/bitstamp.py#L266'>rotkehlchen/exchanges/bitstamp.py:266:21:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/rotki/rotki/blob/2dab3db35815be4ea1964a7e82a00221db514cdc/rotkehlchen/exchanges/bitstamp.py#L440'>rotkehlchen/exchanges/bitstamp.py:440:17:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/rotki/rotki/blob/2dab3db35815be4ea1964a7e82a00221db514cdc/rotkehlchen/exchanges/poloniex.py#L195'>rotkehlchen/exchanges/poloniex.py:195:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/rotki/rotki/blob/2dab3db35815be4ea1964a7e82a00221db514cdc/rotkehlchen/tests/unit/accounting/test_settings.py#L209'>rotkehlchen/tests/unit/accounting/test_settings.py:209:5:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/rotki/rotki/blob/2dab3db35815be4ea1964a7e82a00221db514cdc/rotkehlchen/tests/unit/globaldb/test_globaldb.py#L871'>rotkehlchen/tests/unit/globaldb/test_globaldb.py:871:13:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build">scikit-build/scikit-build</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/tests/test_cmaker.py#L119'>tests/test_cmaker.py:119:13:</a> RUF042 [*] Unnecessary intermediate representation
+ <a href='https://github.com/scikit-build/scikit-build/blob/9e20ec612893541d253674e688269e2495be0a12/tests/test_setup.py#L856'>tests/test_setup.py:856:13:</a> RUF042 [*] Unnecessary intermediate representation
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF042 | 293 | 293 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2024-12-08 01:01_

This is a 12-in-1 rule. All the patterns are needed to preserve consistency with [the reframing in #14564](https://github.com/astral-sh/ruff/pull/14564#pullrequestreview-2465057259). I'm sorry it couldn't have been made as small as a weekend PR should be.

---

_Label `rule` added by @MichaReiser on 2024-12-16 07:46_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-16 07:46_

---

_Comment by @MichaReiser on 2025-04-28 07:07_

> This is a 12-in-1 rule. All the patterns are needed to preserve consistency with [the reframing in #14564](https://github.com/astral-sh/ruff/pull/14564#pullrequestreview-2465057259). I'm sorry it couldn't have been made as small as a weekend PR should be.

Agree. I think we need to take a step back and think through how to best split the checks. 

---

_Closed by @MichaReiser on 2025-04-28 07:07_

---
