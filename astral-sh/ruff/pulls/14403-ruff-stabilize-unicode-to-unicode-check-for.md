```yaml
number: 14403
title: "[`ruff`] Stabilize unicode-to-unicode check for `ambiguous-unicode-xxx` (`RUF001`, `RUF002`, `RUF003`)"
type: pull_request
state: closed
author: dylwil3
labels: []
assignees: []
base: ruff-0.8
head: stabilize-ruf00x
created_at: 2024-11-17T17:52:13Z
updated_at: 2024-11-18T13:06:44Z
url: https://github.com/astral-sh/ruff/pull/14403
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Stabilize unicode-to-unicode check for `ambiguous-unicode-xxx` (`RUF001`, `RUF002`, `RUF003`)

---

_@dylwil3_

This PR stabilizes the preview behavior introduced in #8473, where ambiguous unicode checks are flagged not only for unicode characters that may be confused for ASCII characters, but also for unicode characters that may be confused for other unicode characters.

Documentation has been updated to reflect this change.

Note: While there _are_ open issues related to these rules, none of them are affected by whether the behavior under review is in preview or not.

---

_Comment by @github-actions[bot] on 2024-11-17 18:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 3 projects; 1 project error; 50 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/tools/subcoordinates_zoom.py#L22'>examples/interaction/tools/subcoordinates_zoom.py:22:23:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/_libs/tslibs/timedeltas.pyi#L54'>pandas/_libs/tslibs/timedeltas.pyi:54:6:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/indexes/base.py#L5223'>pandas/core/indexes/base.py:5223:26:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/indexes/base.py#L5224'>pandas/core/indexes/base.py:5224:46:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/indexes/base.py#L5225'>pandas/core/indexes/base.py:5225:69:</a> RUF003 Comment contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/tests/io/test_clipboard.py#L57'>pandas/tests/io/test_clipboard.py:57:34:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/tests/scalar/timedelta/test_constructors.py#L304'>pandas/tests/scalar/timedelta/test_constructors.py:304:25:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/zerver/tests/test_invite.py#L1381'>zerver/tests/test_invite.py:1381:32:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/zerver/tests/test_signup.py#L1140'>zerver/tests/test_signup.py:1140:29:</a> RUF001 String contains ambiguous `µ` (MICRO SIGN). Did you mean `μ` (GREEK SMALL LETTER MU)?
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
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF001 | 6 | 6 | 0 | 0 | 0 |
| RUF003 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8300 -8323 violations, +0 -66 fixes in 8 projects; 1 project error; 45 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6151 -6154 violations, +0 -58 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/__init__.py#L32'>airflow/api/__init__.py:32:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/__init__.py#L43'>airflow/api/__init__.py:43:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/__init__.py#L44'>airflow/api/__init__.py:44:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/auth/backend/deny_all.py#L38'>airflow/api/auth/backend/deny_all.py:38:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/auth/backend/deny_all.py#L44'>airflow/api/auth/backend/deny_all.py:44:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/client/__init__.py#L27'>airflow/api/client/__init__.py:27:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/client/__init__.py#L38'>airflow/api/client/__init__.py:38:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/airflow_health.py#L30'>airflow/api/common/airflow_health.py:30:5:</a> DOC201 `return` is not documented in docstring
... 9012 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `AirflowException` missing from docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/delete_dag.py#L43'>airflow/api/common/delete_dag.py:43:5:</a> DOC501 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/delete_dag.py#L61'>airflow/api/common/delete_dag.py:61:15:</a> DOC501 Raised exception `AirflowException` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/delete_dag.py#L64'>airflow/api/common/delete_dag.py:64:15:</a> DOC501 Raised exception `DagNotFound` missing from docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/mark_tasks.py#L125'>airflow/api/common/mark_tasks.py:125:15:</a> DOC501 Raised exception `ValueError` missing from docstring
... 2944 additional changes omitted for rule DOC501
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/mark_tasks.py#L186'>airflow/api/common/mark_tasks.py:186:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api/common/mark_tasks.py#L190'>airflow/api/common/mark_tasks.py:190:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api_fastapi/common/db/common.py#L33'>airflow/api_fastapi/common/db/common.py:33:5:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/api_fastapi/common/db/common.py#L47'>airflow/api_fastapi/common/db/common.py:47:9:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/assets/__init__.py#L238'>airflow/assets/__init__.py:238:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/assets/__init__.py#L243'>airflow/assets/__init__.py:243:9:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/assets/__init__.py#L358'>airflow/assets/__init__.py:358:9:</a> DOC402 `yield` is not documented in docstring
... 328 additional changes omitted for rule DOC402
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/decorators/__init__.pyi#L117'>airflow/decorators/__init__.pyi:117:25:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/decorators/__init__.pyi#L256'>airflow/decorators/__init__.pyi:256:25:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 Use `float` instead of `int | float`
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/jobs/job.py#L308'>airflow/jobs/job.py:308:39:</a> PYI041 [*] Use `float` instead of `int | float`
+ <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/metrics/base_stats_logger.py#L39'>airflow/metrics/base_stats_logger.py:39:15:</a> PYI041 Use `float` instead of `int | float`
... 52 additional changes omitted for rule PYI041
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/models/dag.py#L1038'>airflow/models/dag.py:1038:36:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/models/dagrun.py#L1317'>airflow/models/dagrun.py:1317:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/apache/airflow/blob/abc97edd0aa7ea4423a5d9f5aa353de36ac7d4cb/airflow/models/dagrun.py#L1439'>airflow/models/dagrun.py:1439:23:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
... 12332 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1166 -1168 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L104'>RELEASING/changelog.py:104:9:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L107'>RELEASING/changelog.py:107:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L113'>RELEASING/changelog.py:113:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L52'>RELEASING/changelog.py:52:9:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L54'>RELEASING/changelog.py:54:13:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/RELEASING/changelog.py#L87'>RELEASING/changelog.py:87:9:</a> DOC201 `return` is not documented in docstring
... 1824 additional changes omitted for rule DOC201
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L43'>scripts/benchmark_migration.py:43:5:</a> DOC501 Raised exception `Exception` missing from docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/benchmark_migration.py#L51'>scripts/benchmark_migration.py:51:11:</a> DOC501 Raised exception `Exception` missing from docstring
- <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/cancel_github_workflows.py#L162'>scripts/cancel_github_workflows.py:162:5:</a> DOC501 Raised exception `ClickException` missing from docstring
+ <a href='https://github.com/apache/superset/blob/e528cb48c44543c14c1ac9a93528b147bcaecfde/scripts/cancel_github_workflows.py#L164'>scripts/cancel_github_workflows.py:164:15:</a> DOC501 Raised exception `ClickException` missing from docstring
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
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L107'>examples/advanced/extensions/parallel_plot/parallel_plot.py:107:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/advanced/extensions/parallel_plot/parallel_plot.py#L15'>examples/advanced/extensions/parallel_plot/parallel_plot.py:15:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L53'>examples/basic/data/server_sent_events_source.py:53:9:</a> DOC402 `yield` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/basic/data/server_sent_events_source.py#L60'>examples/basic/data/server_sent_events_source.py:60:13:</a> DOC402 `yield` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L16'>examples/interaction/js_callbacks/js_on_event.py:16:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/interaction/js_callbacks/js_on_event.py#L21'>examples/interaction/js_callbacks/js_on_event.py:21:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L33'>examples/models/gauges.py:33:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/models/gauges.py#L34'>examples/models/gauges.py:34:5:</a> DOC201 `return` is not documented in docstring
... 395 additional changes omitted for rule DOC201
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L63'>src/bokeh/__init__.py:63:5:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/__init__.py#L65'>src/bokeh/__init__.py:65:1:</a> DOC202 Docstring should not have a returns section because the function doesn't return anything
... 794 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/2c739ac73730d9e6de8b075e8c2c9bae26eca5c2/src/latch/types/metadata.py#L500'>src/latch/types/metadata.py:500:45:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/groupby.py#L4069'>pandas/core/groupby/groupby.py:4069:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/core/groupby/indexing.py#L299'>pandas/core/groupby/indexing.py:299:39:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L1027'>pandas/io/html.py:1027:28:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
- <a href='https://github.com/pandas-dev/pandas/blob/7fe270c8e7656c0c187260677b3b16a17a1281dc/pandas/io/html.py#L223'>pandas/io/html.py:223:32:</a> PYI061 `Literal[None, ...]` can be replaced with `Literal[...] | None`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/68d05208d41a344b4527b74926e0c20332d6418f/stdlib/ast.pyi#L1480'>stdlib/ast.pyi:1480:16:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/python/typeshed/blob/68d05208d41a344b4527b74926e0c20332d6418f/stdlib/ast.pyi#L1481'>stdlib/ast.pyi:1481:35:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/python/typeshed/blob/68d05208d41a344b4527b74926e0c20332d6418f/stdlib/ast.pyi#L1484'>stdlib/ast.pyi:1484:45:</a> RUF038 `Literal[True, False]` can be replaced with `bool`
- <a href='https://github.com/python/typeshed/blob/68d05208d41a344b4527b74926e0c20332d6418f/stubs/pyxdg/xdg/Menu.pyi#L97'>stubs/pyxdg/xdg/Menu.pyi:97:11:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+581 -589 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L19'>analytics/lib/fixtures.py:19:5:</a> DOC501 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L56'>analytics/lib/fixtures.py:56:15:</a> DOC501 Raised exception `AssertionError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/analytics/lib/fixtures.py#L77'>analytics/lib/fixtures.py:77:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L125'>confirmation/models.py:125:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L279'>confirmation/models.py:279:5:</a> DOC201 `return` is not documented in docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L283'>confirmation/models.py:283:5:</a> DOC201 `return` is not documented in docstring
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L298'>confirmation/models.py:298:5:</a> DOC201 `return` is not documented in docstring
... 857 additional changes omitted for rule DOC201
- <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L298'>confirmation/models.py:298:5:</a> DOC501 Raised exception `InvalidError` missing from docstring
+ <a href='https://github.com/zulip/zulip/blob/5d1de4c037fabda6622353b422af6b145d3d40de/confirmation/models.py#L304'>confirmation/models.py:304:15:</a> DOC501 Raised exception `InvalidError` missing from docstring
... 1160 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/wntrblm/nox/blob/aabdc9b0a142d8c277f2c462ad28893624ef9314/nox/command.py#L33'>nox/command.py:33:16:</a> RUF038 `Literal[True, False, ...]` can be replaced with `Literal[...] | bool`
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
| DOC201 | 12109 | 6054 | 6055 | 0 | 0 |
| DOC501 | 3920 | 1960 | 1960 | 0 | 0 |
| DOC402 | 408 | 204 | 204 | 0 | 0 |
| DOC202 | 158 | 79 | 79 | 0 | 0 |
| PYI041 | 66 | 0 | 0 | 0 | 66 |
| PYI061 | 14 | 0 | 14 | 0 | 0 |
| RUF038 | 7 | 0 | 7 | 0 | 0 |
| DOC502 | 4 | 2 | 2 | 0 | 0 |
| DOC403 | 2 | 1 | 1 | 0 | 0 |
| RUF100 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @dylwil3 on 2024-11-17 18:14_

I'm not sure how I feel about these ecosystem changes (in stable - the preview changes seem to be messed up). Seems to me like the use of the "micro" mu vs the greek letter mu is pretty common and done accurately in the context of SI units (e.g. microseconds etc.) So I'm wondering if this behavior should not be stabilized. What do you think @AlexWaygood / @MichaReiser ?

---

_Comment by @Skylion007 on 2024-11-17 19:37_

> I'm not sure how I feel about these ecosystem changes (in stable - the preview changes seem to be messed up). Seems to me like the use of the "micro" mu vs the greek letter mu is pretty common and done accurately in the context of SI units (e.g. microseconds etc.) So I'm wondering if this behavior should not be stabilized. What do you think @AlexWaygood / @MichaReiser ?

Agreed, I feel like the micro use case would be way more common empirically than the greek letter.

---

_Comment by @MichaReiser on 2024-11-18 08:47_

I don't think we should stabilize this change. I'm not sure what's our source for the confusables but I suspect it's the [unicode confusable specification](https://www.unicode.org/reports/tr39/#Identifier_Characters) and I understand that it's specific to identifiers. 

> Identifiers ("IDs") are strings used in application contexts to refer to specific entities of certain significance in the given application. In a given application, an identifier will map to at most one specific entity. Many applications have security requirements related to identifiers. A common example is URLs referring to pages or other resources on the Internet: when a user wishes to access a resource, it is important that the user can be certain what resource they are interacting with. For example, they need to know that they are interacting with a particular financial service and not some other entity that is spoofing the intended service for malicious purposes. This illustrates a general security concern for identifiers: potential ambiguity of strings. While a machine has no difficulty distinguishing between any two different character sequences, it could be very difficult for humans to recognize and distinguish identifiers if an application did not limit which Unicode characters could be in identifiers. The focus of this specification is mitigation of such issues related to the security of identifiers.

> Deliberately restricting the characters that can be used in identifiers is an important security technique. The exclusion of characters from identifiers does not affect the general use of those characters for other purposes, such as for general text in documents. Unicode Standard Annex #31, "Unicode Identifier and Pattern Syntax" [[UAX31](https://www.unicode.org/reports/tr39/#UAX31)] provides a recommended method of determining which strings should qualify as identifiers. The UAX #31 specification extends the common practice of defining identifiers in terms of letters and numbers to the Unicode repertoire.

I understand that applying this set to strings is not the recommended use case because the confusables are valid in text positions. 

I also noticed that neither clippy nor rust flag the mu character, not even in identifier positions, which makes sense to me. Neither does VS Code. One reason for this could be that mu should only be flagged if the rest of the code is in greek, because it's then very likely that you wanted to use the greek mu over the mathematical mu. 

Either way, I think we should revisit the classification and I suspect that this is related to https://github.com/astral-sh/ruff/issues/13330

---

_Comment by @AlexWaygood on 2024-11-18 11:36_

Let's leave this for Ruff 0.8 and open an issue capturing these concerns 👍

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 11:40_

---

_Comment by @dylwil3 on 2024-11-18 13:06_

Opened issue #14433 to track this

---

_Closed by @dylwil3 on 2024-11-18 13:06_

---
