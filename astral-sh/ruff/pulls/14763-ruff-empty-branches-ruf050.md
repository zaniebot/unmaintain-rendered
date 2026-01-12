```yaml
number: 14763
title: "[`ruff`] Empty branches (`RUF050`)"
type: pull_request
state: closed
author: InSyncWithFoo
labels: []
assignees: []
base: main
head: RUF050
created_at: 2024-12-04T02:41:18Z
updated_at: 2024-12-16T12:12:01Z
url: https://github.com/astral-sh/ruff/pull/14763
synced_at: 2026-01-12T15:55:48Z
```

# [`ruff`] Empty branches (`RUF050`)

---

_@InSyncWithFoo_

## Summary

Resolves #13929 and #9472.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-04 02:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+286 -0 violations, +0 -0 fixes in 18 projects; 37 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/generator.py#L244'>rasa/shared/core/generator.py:244:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/story_reader/yaml_story_reader.py#L294'>rasa/shared/core/training_data/story_reader/yaml_story_reader.py:294:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/nlu/extractors/test_mitie_entity_extractor.py#L102'>tests/nlu/extractors/test_mitie_entity_extractor.py:102:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/tests/shared/nlu/training_data/test_features.py#L432'>tests/shared/nlu/training_data/test_features.py:432:9:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+30 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/api_connexion/endpoints/pool_endpoint.py#L104'>airflow/api_connexion/endpoints/pool_endpoint.py:104:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/api_fastapi/core_api/routes/public/pools.py#L131'>airflow/api_fastapi/core_api/routes/public/pools.py:131:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/models/dag.py#L1187'>airflow/models/dag.py:1187:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/models/dag.py#L1761'>airflow/models/dag.py:1761:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/models/dagrun.py#L1353'>airflow/models/dagrun.py:1353:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/serialization/serialized_objects.py#L1393'>airflow/serialization/serialized_objects.py:1393:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/airflow/serialization/serialized_objects.py#L1757'>airflow/serialization/serialized_objects.py:1757:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/dev/chart/build_changelog_annotations.py#L94'>dev/chart/build_changelog_annotations.py:94:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/providers/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py#L161'>providers/src/airflow/providers/amazon/aws/executors/ecs/ecs_executor.py:161:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/airflow/blob/b5c7057d3d586f3fdd8273b1b61557c2db44fa6c/providers/src/airflow/providers/amazon/aws/triggers/eks.py#L151'>providers/src/airflow/providers/amazon/aws/triggers/eks.py:151:17:</a> RUF050 Empty code branch
... 20 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/commands/dataset/importers/v0.py#L282'>superset/commands/dataset/importers/v0.py:282:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/daos/chart.py#L30'>superset/daos/chart.py:30:1:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/db_engine_specs/gsheets.py#L358'>superset/db_engine_specs/gsheets.py:358:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/models/helpers.py#L1764'>superset/models/helpers.py:1764:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/superset/utils/log.py#L37'>superset/utils/log.py:37:1:</a> RUF050 Empty code branch
+ <a href='https://github.com/apache/superset/blob/423a0fefa5e024db9fbf75301997c511b3ec992f/tests/integration_tests/datasource_tests.py#L119'>tests/integration_tests/datasource_tests.py:119:9:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L884'>src/bokeh/command/subcommands/serve.py:884:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L282'>src/bokeh/plotting/_renderer.py:282:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L284'>src/bokeh/plotting/_renderer.py:284:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L286'>src/bokeh/plotting/_renderer.py:286:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/plotting/_renderer.py#L288'>src/bokeh/plotting/_renderer.py:288:17:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/dddd251a5a23dfbb2a843c313126ee41406247a9/ibis/common/deferred.py#L393'>ibis/common/deferred.py:393:9:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L783'>lnbits/core/crud.py:783:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L791'>lnbits/core/crud.py:791:5:</a> RUF050 [*] Empty code branch
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L794'>lnbits/core/crud.py:794:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/core/crud.py#L800'>lnbits/core/crud.py:800:5:</a> RUF050 [*] Empty code branch
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/lnbits/wallets/nwc.py#L579'>lnbits/wallets/nwc.py:579:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/lnbits/lnbits/blob/51c9d294cdb40c777b1048bbee267b49cdaf7a34/tests/helpers.py#L52'>tests/helpers.py:52:5:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+109 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/frame_methods.py#L162'>asv_bench/benchmarks/frame_methods.py:162:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/frame_methods.py#L176'>asv_bench/benchmarks/frame_methods.py:176:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/frame_methods.py#L192'>asv_bench/benchmarks/frame_methods.py:192:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/frame_methods.py#L208'>asv_bench/benchmarks/frame_methods.py:208:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/frame_methods.py#L224'>asv_bench/benchmarks/frame_methods.py:224:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/frame_methods.py#L234'>asv_bench/benchmarks/frame_methods.py:234:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/index_object.py#L101'>asv_bench/benchmarks/index_object.py:101:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/index_object.py#L97'>asv_bench/benchmarks/index_object.py:97:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/io/csv.py#L497'>asv_bench/benchmarks/io/csv.py:497:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/series_methods.py#L370'>asv_bench/benchmarks/series_methods.py:370:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/strings.py#L283'>asv_bench/benchmarks/strings.py:283:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/asv_bench/benchmarks/timeseries.py#L138'>asv_bench/benchmarks/timeseries.py:138:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/_testing/asserters.py#L1009'>pandas/_testing/asserters.py:1009:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/_testing/asserters.py#L129'>pandas/_testing/asserters.py:129:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/_testing/asserters.py#L132'>pandas/_testing/asserters.py:132:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/_testing/asserters.py#L1406'>pandas/_testing/asserters.py:1406:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/core/arrays/arrow/array.py#L616'>pandas/core/arrays/arrow/array.py:616:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/core/arrays/datetimelike.py#L2002'>pandas/core/arrays/datetimelike.py:2002:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/pandas-dev/pandas/blob/719fc0fcbcda23a79156ccfc990228df0851452f/pandas/core/arrays/datetimelike.py#L2269'>pandas/core/arrays/datetimelike.py:2269:9:</a> RUF050 Empty code branch
... 90 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/9c75ea15c2f31a77e6043b80b1b7081372319d85/cibuildwheel/pyodide.py#L406'>cibuildwheel/pyodide.py:406:5:</a> RUF050 [*] Empty code branch
+ <a href='https://github.com/pypa/cibuildwheel/blob/9c75ea15c2f31a77e6043b80b1b7081372319d85/test/utils.py#L29'>test/utils.py:29:1:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/puzzle/provider.py#L240'>src/poetry/puzzle/provider.py:240:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/utils/shell.py#L129'>src/poetry/utils/shell.py:129:9:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/reflex-dev/reflex/blob/d75a708e6bf46ba49904fcce114fe84c42df2375/reflex/base.py#L35'>reflex/base.py:35:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/reflex-dev/reflex/blob/d75a708e6bf46ba49904fcce114fe84c42df2375/reflex/utils/processes.py#L298'>reflex/utils/processes.py:298:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/reflex-dev/reflex/blob/d75a708e6bf46ba49904fcce114fe84c42df2375/tests/units/test_app.py#L1195'>tests/units/test_app.py:1195:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/reflex-dev/reflex/blob/d75a708e6bf46ba49904fcce114fe84c42df2375/tests/units/test_state.py#L2076'>tests/units/test_state.py:2076:9:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/67b1dbfc379ecd0bd9a7d348ea3dedbd9c6528d6/rotkehlchen/data_import/importers/nexo.py#L155'>rotkehlchen/data_import/importers/nexo.py:155:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/rotki/rotki/blob/67b1dbfc379ecd0bd9a7d348ea3dedbd9c6528d6/rotkehlchen/icons.py#L32'>rotkehlchen/icons.py:32:1:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/3943920fa267dc83f9295279bea1c774c0916f13/tests/test_cmake_ast.py#L137'>tests/test_cmake_ast.py:137:5:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+35 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/analytics/views/stats.py#L67'>analytics/views/stats.py:67:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/tools/lib/provision_inner.py#L291'>tools/lib/provision_inner.py:291:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/data_import/slack_message_conversion.py#L241'>zerver/data_import/slack_message_conversion.py:241:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/decorator.py#L80'>zerver/decorator.py:80:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/lib/event_schema.py#L270'>zerver/lib/event_schema.py:270:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/lib/events.py#L1375'>zerver/lib/events.py:1375:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/lib/events.py#L1517'>zerver/lib/events.py:1517:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/lib/events.py#L1520'>zerver/lib/events.py:1520:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/lib/events.py#L1523'>zerver/lib/events.py:1523:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/zulip/zulip/blob/a59a93c4362e43b1635c37039672a236ce80dc0e/zerver/lib/events.py#L1526'>zerver/lib/events.py:1526:5:</a> RUF050 Empty code branch
... 25 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/cli/devserver.py#L231'>indico/cli/devserver.py:231:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/core/celery/controllers.py#L25'>indico/core/celery/controllers.py:25:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/modules/designer/pdf.py#L155'>indico/modules/designer/pdf.py:155:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/modules/events/api.py#L620'>indico/modules/events/api.py:620:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/modules/events/export.py#L384'>indico/modules/events/export.py:384:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/modules/logs/util.py#L54'>indico/modules/logs/util.py:54:9:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/testing/util.py#L99'>indico/testing/util.py:99:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/util/string.py#L87'>indico/util/string.py:87:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/indico/indico/blob/833dfeb5f544ba3de6e6a607306a9b33deb7ca93/indico/web/flask/app.py#L171'>indico/web/flask/app.py:171:5:</a> RUF050 Empty code branch
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+10 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_core/_io_kqueue.py#L198'>src/trio/_core/_io_kqueue.py:198:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_core/_io_windows.py#L570'>src/trio/_core/_io_windows.py:570:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_core/_io_windows.py#L632'>src/trio/_core/_io_windows.py:632:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_core/_run.py#L601'>src/trio/_core/_run.py:601:13:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_core/_tests/test_ki.py#L107'>src/trio/_core/_tests/test_ki.py:107:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_core/_tests/test_ki.py#L99'>src/trio/_core/_tests/test_ki.py:99:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_dtls.py#L793'>src/trio/_dtls.py:793:17:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_subprocess_platform/__init__.py#L82'>src/trio/_subprocess_platform/__init__.py:82:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/_tests/test_tracing.py#L36'>src/trio/_tests/test_tracing.py:36:5:</a> RUF050 Empty code branch
+ <a href='https://github.com/python-trio/trio/blob/c4c8ce41aba240439678d2e6e847721ad441f8ab/src/trio/testing/_fake_net.py#L202'>src/trio/testing/_fake_net.py:202:9:</a> RUF050 Empty code branch
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF050 | 286 | 286 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @InSyncWithFoo on 2024-12-04 03:08_

Currently this rule is a bit too overzealous. It will also flag empty `if`/`elif` followed by non-empty `elif`/`else`.

Personally, I'm in favor of keeping this behaviour, as it enforces refactoring/denesting. I understand that I'm exceedingly likely to be in the minority, however.

---

_Comment by @MichaReiser on 2024-12-04 07:40_

From a first skim through the ecosystem checks:

We should exclude branches with comments because it expresses that the branch is intentional:

* https://github.com/zulip/zulip/blob/7e92c2ad16c7f93a031227477c7d7e241482ac83/tools/lib/provision_inner.py#L291-L299
* https://github.com/indico/indico/blob/a111b446a3fda1a126df765df27562eec6e0a828/indico/cli/devserver.py#L231

I know this is quiet the opposite of what I said in the issue, but I just realised that clippy intentionally has multiple rules:

*  [empty-loop](https://rust-lang.github.io/rust-clippy/master/index.html#empty_loop) There are cases where an empty loop is intentional (e.g. a spin loop)
* [empty-enum](https://rust-lang.github.io/rust-clippy/master/index.html#empty_loop)
* [needless-else](https://rust-lang.github.io/rust-clippy/master/index.html#needless_else)
* [https://rust-lang.github.io/rust-clippy/master/index.html#needless_if](https://rust-lang.github.io/rust-clippy/master/index.html#needless_if)

Thinking this through more, this makes sense to me because a needless-if is just useless where an empty loop can be correct sometimes.

> Currently this rule is a bit too overzealous. It will also flag empty if/elif followed by non-empty elif/else.

I like using `if` or `elif` branches to skip early. It can help to avoid repeating the same condition in negated form for all following branches.

---

_Comment by @InSyncWithFoo on 2024-12-04 18:16_

> I like using `if` or `elif` branches to skip early. It can help to avoid repeating the same condition in negated form for all following branches.

In such cases, I would move the conditions to another function to take advantage of early returns, but that's just me.

----

As for splitting, I would like to use and <em>reserve</em> the `RUF06*` block, if that's alright with you. That way, it would be easier to select all of these closely related rules using a single selector.

---

_Comment by @MichaReiser on 2024-12-05 07:31_

> In such cases, I would move the conditions to another function to take advantage of early returns, but that's just me.

That's fair. I just think it's too opinionated for this rule. Such a branch isn't needless; it's more of a stylistic choice; you don't want any empty branches. 

> As for splitting, I would like to use and reserve the RUF06* block, if that's alright with you. That way, it would be easier to select all of these closely related rules using a single selector.

I don't think we want to block off an entire range for just a few rules. 

Let's also hear from @AlexWaygood to ensure we're aligned to avoid changing our approach mroe than once 

---

_Comment by @gpauloski on 2024-12-14 21:32_

> That's fair. I just think it's too opinionated for this rule. Such a branch isn't needless; it's more of a stylistic choice; you don't want any empty branches.

I agree. My intention when I opened the issue was just to detect what is most likely unintentional dead code (e.g., empty conditionals from other auto-fixes/formatters). I would not want deliberate style choices to get flagged, especially the empty branches with comments.

---

_Comment by @MichaReiser on 2024-12-16 11:05_

Thanks for keeping this PR up to date. I talked to @AlexWaygood, and we agree that we should split this rule into multiple smaller rules. I'll comment on the issue with a suggestion. I'm sorry to have lead you in the wrong direction :(

---

_Closed by @MichaReiser on 2024-12-16 11:05_

---

_Branch deleted on 2024-12-16 12:12_

---
