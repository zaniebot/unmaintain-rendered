```yaml
number: 14399
title: "[`ruff`] Stabilize unsafe fix for `zip-instead-of-pairwise (RUF007)`"
type: pull_request
state: closed
author: dylwil3
labels:
  - fixes
  - do-not-merge
assignees: []
base: ruff-0.8
head: stabilize-ruf007
created_at: 2024-11-17T13:48:26Z
updated_at: 2024-11-17T16:35:15Z
url: https://github.com/astral-sh/ruff/pull/14399
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Stabilize unsafe fix for `zip-instead-of-pairwise (RUF007)`

---

_@dylwil3_

This PR stabilizes the unsafe fix for `[zip-instead-of-pairwise (RUF007)](https://docs.astral.sh/ruff/rules/zip-instead-of-pairwise/#zip-instead-of-pairwise-ruf007)`, which replaces the use of zip with that of `itertools.pairwise` and has been available under preview since version 0.5.7.

There are no open issues regarding RUF007 at the time of this writing.




---

_Label `fixes` added by @dylwil3 on 2024-11-17 13:48_

---

_Label `do-not-merge` added by @dylwil3 on 2024-11-17 13:48_

---

_Review requested from @carljm by @dylwil3 on 2024-11-17 13:48_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-11-17 13:48_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-11-17 13:48_

---

_Review requested from @sharkdp by @dylwil3 on 2024-11-17 13:48_

---

_Comment by @github-actions[bot] on 2024-11-17 14:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8323 -8304 violations, +66 -0 fixes in 8 projects; 1 project error; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6154 -6151 violations, +58 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/__init__.py#L43'>airflow/api/__init__.py:43:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/__init__.py#L44'>airflow/api/__init__.py:44:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/auth/backend/deny_all.py#L38'>airflow/api/auth/backend/deny_all.py:38:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/auth/backend/deny_all.py#L44'>airflow/api/auth/backend/deny_all.py:44:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/client/__init__.py#L27'>airflow/api/client/__init__.py:27:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/client/__init__.py#L38'>airflow/api/client/__init__.py:38:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/airflow_health.py#L30'>airflow/api/common/airflow_health.py:30:5:</a> DOC201 `return` is not documented in docstring
... 9012 additional changes omitted for rule DOC201
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `DagNotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/delete_dag.py#L61'>airflow/api/common/delete_dag.py:61:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/delete_dag.py#L64'>airflow/api/common/delete_dag.py:64:15:</a> DOC501 Raised exception `DagNotFound` missing from docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/mark_tasks.py#L125'>airflow/api/common/mark_tasks.py:125:15:</a> DOC501 Raised exception `ValueError` missing from docstring
... 2944 additional changes omitted for rule DOC501
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/mark_tasks.py#L186'>airflow/api/common/mark_tasks.py:186:5:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api/common/mark_tasks.py#L190'>airflow/api/common/mark_tasks.py:190:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api_fastapi/common/db/common.py#L33'>airflow/api_fastapi/common/db/common.py:33:5:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/api_fastapi/common/db/common.py#L47'>airflow/api_fastapi/common/db/common.py:47:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/assets/__init__.py#L238'>airflow/assets/__init__.py:238:9:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/assets/__init__.py#L243'>airflow/assets/__init__.py:243:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/assets/__init__.py#L358'>airflow/assets/__init__.py:358:9:</a> DOC402 `yield` is not documented in docstring
... 328 additional changes omitted for rule DOC402
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 [*] Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 Use `float` instead of `int | float`
... 52 additional changes omitted for rule PYI041
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/models/dag.py#L1038'>airflow/models/dag.py:1038:36:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/models/dagrun.py#L1317'>airflow/models/dagrun.py:1317:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/apache/airflow/blob/987c50a0ae9baffe3dfffdb0d1710679c732f990/airflow/models/dagrun.py#L1439'>airflow/models/dagrun.py:1439:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
... 12332 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1168 -1166 violations, +8 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L104'>RELEASING/changelog.py:104:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L107'>RELEASING/changelog.py:107:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L113'>RELEASING/changelog.py:113:13:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L52'>RELEASING/changelog.py:52:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L54'>RELEASING/changelog.py:54:13:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L87'>RELEASING/changelog.py:87:9:</a> DOC201 `return` is not documented in docstring
... 1824 additional changes omitted for rule DOC201
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L43'>scripts/benchmark_migration.py:43:5:</a> DOC501 Raised exception `Exception` missing from docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L51'>scripts/benchmark_migration.py:51:11:</a> DOC501 Raised exception `Exception` missing from docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/cancel_github_workflows.py#L162'>scripts/cancel_github_workflows.py:162:5:</a> DOC501 Raised exception `ClickException` missing from docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/cancel_github_workflows.py#L164'>scripts/cancel_github_workflows.py:164:15:</a> DOC501 Raised exception `ClickException` missing from docstring
... 2332 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+402 -402 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L107'>examples/advanced/extensions/parallel_plot/parallel_plot.py:107:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L15'>examples/advanced/extensions/parallel_plot/parallel_plot.py:15:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L53'>examples/basic/data/server_sent_events_source.py:53:9:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L60'>examples/basic/data/server_sent_events_source.py:60:13:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L16'>examples/interaction/js_callbacks/js_on_event.py:16:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L21'>examples/interaction/js_callbacks/js_on_event.py:21:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L33'>examples/models/gauges.py:33:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L34'>examples/models/gauges.py:34:5:</a> DOC201 `return` is not documented in docstring
... 395 additional changes omitted for rule DOC201
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
... 794 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/groupby.py#L4069'>pandas/core/groupby/groupby.py:4069:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L1027'>pandas/io/html.py:1027:28:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+4 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/ast.pyi#L1480'>stdlib/ast.pyi:1480:16:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/ast.pyi#L1481'>stdlib/ast.pyi:1481:35:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
+ <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/ast.pyi#L1484'>stdlib/ast.pyi:1484:45:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/random.pyi#L45'>stdlib/random.pyi:45:31:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/random.pyi#L52'>stdlib/random.pyi:52:27:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/turtle.pyi#L443'>stdlib/turtle.pyi:443:16:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stdlib/turtle.pyi#L444'>stdlib/turtle.pyi:444:17:</a> PYI041 Use `float` instead of `int | float`
+ <a href='https://github.com/python/typeshed/blob/b4cd0bdf1bb9949efb3c751664050214b13be0a6/stubs/pyxdg/xdg/Menu.pyi#L97'>stubs/pyxdg/xdg/Menu.pyi:97:11:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+589 -581 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC501 Raised exception `AssertionError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L56'>analytics/lib/fixtures.py:56:15:</a> DOC501 Raised exception `AssertionError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L77'>analytics/lib/fixtures.py:77:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L125'>confirmation/models.py:125:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L279'>confirmation/models.py:279:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L283'>confirmation/models.py:283:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L298'>confirmation/models.py:298:5:</a> DOC201 `return` is not documented in docstring
... 857 additional changes omitted for rule DOC201
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L298'>confirmation/models.py:298:5:</a> DOC501 Raised exception `InvalidError` missing from docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L304'>confirmation/models.py:304:15:</a> DOC501 Raised exception `InvalidError` missing from docstring
... 1160 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/aabdc9b0a142d8c277f2c462ad28893624ef9314/nox/command.py#L33'>nox/command.py:33:16:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>
<details><summary>Changes by rule (10 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| DOC201 | 12109 | 6055 | 6054 | 0 | 0 |
| DOC501 | 3920 | 1960 | 1960 | 0 | 0 |
| DOC402 | 408 | 204 | 204 | 0 | 0 |
| DOC202 | 158 | 79 | 79 | 0 | 0 |
| PYI041 | 70 | 0 | 4 | 66 | 0 |
| PYI061 | 14 | 14 | 0 | 0 | 0 |
| RUF038 | 7 | 7 | 0 | 0 | 0 |
| DOC502 | 4 | 2 | 2 | 0 | 0 |
| DOC403 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2024-11-17 14:32_

I have commited some kind of high git crime from which there is no recovery within my limited skillset... nuking this and starting over (i.e. by branching off of `ruff-0.8` instead of `main`)

---

_Closed by @dylwil3 on 2024-11-17 14:32_

---

_Branch deleted on 2024-11-17 16:35_

---
