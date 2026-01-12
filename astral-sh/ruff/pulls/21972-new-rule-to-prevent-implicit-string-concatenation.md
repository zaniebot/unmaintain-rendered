```yaml
number: 21972
title: New rule to prevent implicit string concatenation in collections
type: pull_request
state: merged
author: hauntsaninja
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: ruffisc
created_at: 2025-12-14T03:22:17Z
updated_at: 2025-12-17T22:37:01Z
url: https://github.com/astral-sh/ruff/pull/21972
synced_at: 2026-01-12T15:57:38Z
```

# New rule to prevent implicit string concatenation in collections

---

_@hauntsaninja_

This is a common footgun, see the example in https://github.com/astral-sh/ruff/issues/13014#issuecomment-3411496519

Fixes #13014 , fixes #13031

---

_Comment by @astral-sh-bot[bot] on 2025-12-14 03:31_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+342 -0 violations, +0 -0 fixes in 14 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+9 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/formulary/test_dielectric.py#L191'>tests/formulary/test_dielectric.py:191:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/formulary/test_dielectric.py#L221'>tests/formulary/test_dielectric.py:221:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/particles/test_particle_class.py#L1165'>tests/particles/test_particle_class.py:1165:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/decorators/test_checks.py#L199'>tests/utils/decorators/test_checks.py:199:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/decorators/test_checks.py#L221'>tests/utils/decorators/test_checks.py:221:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/decorators/test_checks.py#L375'>tests/utils/decorators/test_checks.py:375:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/decorators/test_checks.py#L400'>tests/utils/decorators/test_checks.py:400:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/decorators/test_checks.py#L413'>tests/utils/decorators/test_checks.py:413:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/decorators/test_checks.py#L427'>tests/utils/decorators/test_checks.py:427:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+94 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/src/airflow/cli/commands/connection_command.py#L288'>airflow-core/src/airflow/cli/commands/connection_command.py:288:25:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L413'>airflow-core/tests/unit/always/test_project_structure.py:413:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L415'>airflow-core/tests/unit/always/test_project_structure.py:415:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L518'>airflow-core/tests/unit/always/test_project_structure.py:518:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L523'>airflow-core/tests/unit/always/test_project_structure.py:523:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L525'>airflow-core/tests/unit/always/test_project_structure.py:525:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L527'>airflow-core/tests/unit/always/test_project_structure.py:527:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L529'>airflow-core/tests/unit/always/test_project_structure.py:529:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L531'>airflow-core/tests/unit/always/test_project_structure.py:531:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L577'>airflow-core/tests/unit/always/test_project_structure.py:577:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/always/test_project_structure.py#L579'>airflow-core/tests/unit/always/test_project_structure.py:579:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/cli/commands/test_connection_command.py#L245'>airflow-core/tests/unit/cli/commands/test_connection_command.py:245:21:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/airflow/blob/55a4ecd6be6cec0d7f7850258fbf40f578841785/airflow-core/tests/unit/cli/commands/test_connection_command.py#L248'>airflow-core/tests/unit/cli/commands/test_connection_command.py:248:21:</a> ISC004 Unparenthesized implicit string concatenation in collection
... 81 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+29 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/db_engine_specs/denodo.py#L103'>superset/db_engine_specs/denodo.py:103:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/db_engine_specs/denodo.py#L109'>superset/db_engine_specs/denodo.py:109:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/db_engine_specs/denodo.py#L125'>superset/db_engine_specs/denodo.py:125:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/tool/get_chart_data.py#L312'>superset/mcp_service/chart/tool/get_chart_data.py:312:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/validation/runtime/cardinality_validator.py#L146'>superset/mcp_service/chart/validation/runtime/cardinality_validator.py:146:25:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/validation/runtime/cardinality_validator.py#L154'>superset/mcp_service/chart/validation/runtime/cardinality_validator.py:154:25:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/validation/runtime/cardinality_validator.py#L175'>superset/mcp_service/chart/validation/runtime/cardinality_validator.py:175:21:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/validation/runtime/chart_type_suggester.py#L271'>superset/mcp_service/chart/validation/runtime/chart_type_suggester.py:271:21:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/validation/schema_validator.py#L182'>superset/mcp_service/chart/validation/schema_validator.py:182:21:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/apache/superset/blob/d0361cb88101ccea9ff400c0aad8b42074012ac2/superset/mcp_service/chart/validation/schema_validator.py#L184'>superset/mcp_service/chart/validation/schema_validator.py:184:21:</a> ISC004 Unparenthesized implicit string concatenation in collection
... 19 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/typings/bs4.pyi#L15'>src/typings/bs4.pyi:15:67:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L232'>tests/unit/bokeh/io/test_export.py:232:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L299'>tests/unit/bokeh/io/test_export.py:299:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/io/test_export.py#L303'>tests/unit/bokeh/io/test_export.py:303:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/core/tests/unit_tests/_api/test_beta_decorator.py#L19'>libs/core/tests/unit_tests/_api/test_beta_decorator.py:19:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/core/tests/unit_tests/_api/test_beta_decorator.py#L38'>libs/core/tests/unit_tests/_api/test_beta_decorator.py:38:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/core/tests/unit_tests/_api/test_deprecation.py#L26'>libs/core/tests/unit_tests/_api/test_deprecation.py:26:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/core/tests/unit_tests/_api/test_deprecation.py#L53'>libs/core/tests/unit_tests/_api/test_deprecation.py:53:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/core/tests/unit_tests/language_models/chat_models/test_rate_limiting.py#L217'>libs/core/tests/unit_tests/language_models/chat_models/test_rate_limiting.py:217:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/langchain/langchain_classic/evaluation/scoring/prompt.py#L42'>libs/langchain/langchain_classic/evaluation/scoring/prompt.py:42:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py#L31'>libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py:31:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py#L37'>libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py:37:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py#L47'>libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py:47:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/langchain-ai/langchain/blob/37d8666276cee78dbe410f9d6e472f3024125825/libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py#L56'>libs/langchain/tests/unit_tests/chains/test_qa_with_sources.py:56:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+81 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/conftest.py#L161'>pandas/conftest.py:161:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/io/parsers/c_parser_wrapper.py#L369'>pandas/io/parsers/c_parser_wrapper.py:369:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/arithmetic/test_period.py#L1157'>pandas/tests/arithmetic/test_period.py:1157:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/arithmetic/test_period.py#L1192'>pandas/tests/arithmetic/test_period.py:1192:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/arrays/datetimes/test_constructors.py#L34'>pandas/tests/arrays/datetimes/test_constructors.py:34:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/arrays/floating/test_arithmetic.py#L170'>pandas/tests/arrays/floating/test_arithmetic.py:170:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/computation/test_eval.py#L179'>pandas/tests/computation/test_eval.py:179:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/computation/test_eval.py#L224'>pandas/tests/computation/test_eval.py:224:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/groupby/test_raises.py#L265'>pandas/tests/groupby/test_raises.py:265:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/groupby/test_raises.py#L442'>pandas/tests/groupby/test_raises.py:442:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pandas-dev/pandas/blob/b43b95d2b4ef015903f45c9b34e2030bb582d70a/pandas/tests/groupby/test_raises.py#L448'>pandas/tests/groupby/test_raises.py:448:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
... 70 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+29 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/src/pip/_internal/index/collector.py#L480'>src/pip/_internal/index/collector.py:480:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/src/pip/_internal/operations/freeze.py#L201'>src/pip/_internal/operations/freeze.py:201:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/functional/test_check.py#L181'>tests/functional/test_check.py:181:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/functional/test_install.py#L2030'>tests/functional/test_install.py:2030:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/functional/test_install_check.py#L112'>tests/functional/test_install_check.py:112:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/functional/test_new_resolver_hashes.py#L45'>tests/functional/test_new_resolver_hashes.py:45:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/functional/test_new_resolver_hashes.py#L54'>tests/functional/test_new_resolver_hashes.py:54:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/unit/test_collector.py#L130'>tests/unit/test_collector.py:130:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/unit/test_collector.py#L778'>tests/unit/test_collector.py:778:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/pip/blob/91d1ae2f22df116f710931133db46130dc07e945/tests/unit/test_collector.py#L816'>tests/unit/test_collector.py:816:9:</a> ISC004 Unparenthesized implicit string concatenation in collection
... 19 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+11 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/pkg_resources/tests/test_resources.py#L278'>pkg_resources/tests/test_resources.py:278:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/pkg_resources/tests/test_resources.py#L296'>pkg_resources/tests/test_resources.py:296:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/pkg_resources/tests/test_resources.py#L325'>pkg_resources/tests/test_resources.py:325:17:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/bdist_egg.py#L82'>setuptools/command/bdist_egg.py:82:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/bdist_egg.py#L89'>setuptools/command/bdist_egg.py:89:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/bdist_wheel.py#L148'>setuptools/command/bdist_wheel.py:148:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/bdist_wheel.py#L154'>setuptools/command/bdist_wheel.py:154:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/bdist_wheel.py#L188'>setuptools/command/bdist_wheel.py:188:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/bdist_wheel.py#L200'>setuptools/command/bdist_wheel.py:200:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
+ <a href='https://github.com/pypa/setuptools/blob/d198e86f57231e83de87975c5c82bc40c196da79/setuptools/command/dist_info.py#L32'>setuptools/command/dist_info.py:32:13:</a> ISC004 Unparenthesized implicit string concatenation in collection
... 1 additional changes omitted for project
</pre>

</p>
</details>

_... Truncated remaining completed project reports due to GitHub comment length restrictions_

<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ISC004 | 342 | 342 | 0 | 0 | 0 |

</p>
</details>





---

_Marked ready for review by @hauntsaninja on 2025-12-14 06:24_

---

_Comment by @zsol on 2025-12-14 16:40_

FWIW This was one of the more impactful lint rules in Fixit that prevented several serious outages at Instagram/Meta

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:96 on 2025-12-15 07:44_

I don't think we should offer a fix here. The whole point of the rule is that it's unclear if the string is supposed to be two collection items or a single collection item.

---

_@MichaReiser reviewed on 2025-12-15 07:44_

Thank you. This rule makes sense to me

---

_@hauntsaninja reviewed on 2025-12-15 07:52_

---

_Review comment by @hauntsaninja on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:96 on 2025-12-15 07:52_

Would you be okay with an "unsafe" fix?

I've been working with this rule on a massive codebase and having the fix made diffs much easier to review (in addition to genuinely fixing all the non-buggy cases)

---

_@hauntsaninja reviewed on 2025-12-15 07:55_

---

_Review comment by @hauntsaninja on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:96 on 2025-12-15 07:55_

In general, there are several ruff autofixes that aren't exactly in line with the point of the rule (e.g. adding strict=False to `zip`s) but are useful because they make diffs much more obvious and help people adopt the rule in the first place

---

_Review requested from @ntBre by @MichaReiser on 2025-12-16 07:03_

---

_@MichaReiser reviewed on 2025-12-16 07:34_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:96 on 2025-12-16 07:34_

I'd be okay with that, but it's a stretch on what unsafe is intended for, because there's nothing inherently unsafe with the fix. 

---

_Label `rule` added by @ntBre on 2025-12-16 16:13_

---

_Label `preview` added by @ntBre on 2025-12-16 16:13_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:96 on 2025-12-16 16:26_

I'm okay with this too. It's not what we usually use unsafe fixes for, but I'm convinced by the benefits.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:57 on 2025-12-16 16:31_

I don't feel strongly about this at all, but the `did you forget a comma?` after the semi-colon feels slightly too long/out of place to me. A couple of other options would be dropping it entirely or moving it to a `help` (or `info`) sub-diagnostic. That would look something like this:

```
ISC004 [*] Unparenthesized implicit string concatenation in collection
 --> ISC004.py:4:5
  |
2 |       "Lobsters have blue blood.",
3 |       "The liver is the only human organ that can fully regenerate itself.",
4 | /     "Clarinets are made almost entirely out of wood from the mpingo tree."
5 | |     "In 1971, astronaut Alan Shepard played golf on the moon.",
  | |______________________________________________________________^
6 |   )
  |
help: Did you forget a comma?
help: Wrap implicitly concatenated strings in parentheses
1 | facts = (
2 |     "Lobsters have blue blood.",
3 |     "The liver is the only human organ that can fully regenerate itself.",
  -     "Clarinets are made almost entirely out of wood from the mpingo tree."
  -     "In 1971, astronaut Alan Shepard played golf on the moon.",
4 +     ("Clarinets are made almost entirely out of wood from the mpingo tree."
5 +     "In 1971, astronaut Alan Shepard played golf on the moon."),
6 | )
7 | 
8 | facts = [
```

But again, I don't feel strongly, and we can leave it here in the main message if you prefer. That has the benefit of showing up in the `--output-format=concise` diagnostic anyway.

I also wonder if `Ambiguous` might be a better first word in the diagnostic than `Unparenthesized` since that's the problem parenthesizing is trying to fix, but that's an even smaller nit/suggestion.

---

_@ntBre reviewed on 2025-12-16 16:42_

Thank you, this looks great!

I only had one comment with a couple of small ideas, but I'm also happy with the current state of the diagnostic.

I do agree with Micha that we should make this an unsafe fix, if we keep the fix, though.

I think we should also touch base upstream. They expressed interest in adding this rule as well (https://github.com/flake8-implicit-str-concat/flake8-implicit-str-concat/issues/55), so we should double check the error code to stay in sync, if we make it an ISC rule, which I think makes sense otherwise.

Have you had a chance to go through the ecosystem results? Cases like this feel a bit unfortunate to me:

```py
        assert not np.isclose(val, expected, rtol=1e-16, atol=0.0), (
            f"Permittivity value test gives {val} and should not be "
            f"equal to {expected}.",
        )
```

Maybe we should exclude cases with a single element?

---

_Comment by @MichaReiser on 2025-12-16 17:41_

I think I'm fine with

```py
        assert not np.isclose(val, expected, rtol=1e-16, atol=0.0), (
          f"Permittivity value test gives {val} and should not be "
          f"equal to {expected}.",
      )
```

because the trailing `,` is probably unintentional (it's now a tuple instead of string)

---

_Comment by @hauntsaninja on 2025-12-16 20:59_

Thank you both!

> cases with single element

Like Micha says, this should be a string not a tuple. There were several accidental tuples in my work codebase, so this would be good to keep. (Most of these were asserts, we could consider a separate rule that complains about single element str tuples as assert message)

> did you forget a comma

Moved it to a note, thanks!

> I do agree with Micha that we should make this an unsafe fix

Made it an unsafe fix, thanks!

> ambiguous vs unparenthesized

I prefer "unparenthesized" because it makes it clearer what the lint rule wants you to do

> I think we should also touch base upstream

Commented upstream

> ecosystem results

Yeah, I looked at them, felt mostly positive / expected hits to me. In a large internal codebase this has caught dozens and dozens and dozens of bugs, and is a readability improvement in most of the other cases.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/collection_literal.rs`:54 on 2025-12-16 21:07_

Could you bump this to `0.14.10`? Sorry!

```suggestion
#[violation_metadata(preview_since = "0.14.10")]
```

---

_@ntBre approved on 2025-12-16 21:07_

Thank you!

---

_Comment by @hauntsaninja on 2025-12-16 21:40_

Fixed, and all is green!

---

_Comment by @ntBre on 2025-12-16 21:48_

Thanks again! I might give upstream at least until tomorrow to make sure there are no objections on the rule code (I take the :rocket: react on your comment as a positive sign), but this is otherwise good to go and should land before our release on Thursday.

---

_Merged by @ntBre on 2025-12-17 22:37_

---

_Closed by @ntBre on 2025-12-17 22:37_

---
