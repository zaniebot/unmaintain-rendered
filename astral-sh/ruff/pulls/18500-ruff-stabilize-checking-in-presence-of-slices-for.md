```yaml
number: 18500
title: "[`ruff`] Stabilize checking in presence of slices for `collection-literal-concatenation` (`RUF005`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: brent/release-0.12.0
head: dylan/stabilize-slices-in-concat
created_at: 2025-06-06T15:30:28Z
updated_at: 2025-06-06T17:40:13Z
url: https://github.com/astral-sh/ruff/pull/18500
synced_at: 2026-01-10T18:45:04Z
```

# [`ruff`] Stabilize checking in presence of slices for `collection-literal-concatenation` (`RUF005`)

---

_Pull request opened by @dylwil3 on 2025-06-06 15:30_

_No description provided._

---

_Added to milestone `v0.12` by @dylwil3 on 2025-06-06 15:30_

---

_Label `rule` added by @dylwil3 on 2025-06-06 15:30_

---

_Comment by @github-actions[bot] on 2025-06-06 15:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+21 -0 violations, +0 -0 fixes in 8 projects; 47 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py#L113'>providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py:113:38:</a> RUF005 Consider iterable unpacking instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py#L138'>providers/apache/spark/tests/unit/apache/spark/hooks/test_spark_jdbc_script.py:138:38:</a> RUF005 Consider iterable unpacking instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/providers/celery/src/airflow/providers/celery/executors/celery_executor.py#L325'>providers/celery/src/airflow/providers/celery/executors/celery_executor.py:325:32:</a> RUF005 Consider `(*task_tuple[:3], execute_command)` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/providers/google/tests/unit/google/cloud/hooks/test_cloud_storage_transfer_service.py#L859'>providers/google/tests/unit/google/cloud/hooks/test_cloud_storage_transfer_service.py:859:81:</a> RUF005 Consider `[*pages_requests[1:], None]` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/providers/google/tests/unit/google/cloud/hooks/test_mlengine.py#L178'>providers/google/tests/unit/google/cloud/hooks/test_mlengine.py:178:81:</a> RUF005 Consider `[*pages_requests[1:], None]` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/providers/google/tests/unit/google/cloud/hooks/test_mlengine.py#L807'>providers/google/tests/unit/google/cloud/hooks/test_mlengine.py:807:81:</a> RUF005 Consider `[*pages_requests[1:], None]` instead of concatenation
+ <a href='https://github.com/apache/airflow/blob/8f3c996a82f0e1d89b7121490ef436763b6ace62/task-sdk/src/airflow/sdk/io/path.py#L102'>task-sdk/src/airflow/sdk/io/path.py:102:20:</a> RUF005 Consider `(parsed_url.geturl(), *args[1:])` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/b3f436a030ae110c75e5a6cd02d63e63021eb49d/superset/viz.py#L2265'>superset/viz.py:2265:33:</a> RUF005 Consider `[DTTM_ALIAS, *groups[:i]]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/binary-husky/gpt_academic/blob/8c2143229113ad0953b4e4dfd2a543292ce65b14/toolbox.py#L544'>toolbox.py:544:23:</a> RUF005 Consider iterable unpacking instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/topics/pie/donut.py#L41'>examples/topics/pie/donut.py:41:14:</a> RUF005 Consider `[0, *angles[:-1]]` instead of concatenation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L449'>src/bokeh/server/tornado.py:449:36:</a> RUF005 Consider `(self._prefix + p[0], *p[1:], data)` instead of concatenation
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/server/tornado.py#L452'>src/bokeh/server/tornado.py:452:32:</a> RUF005 Consider `(self._prefix + p[0], *p[1:])` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/761f8c3231806d4e9e995366773ec79b135c1628/libs/core/langchain_core/messages/utils.py#L1344'>libs/core/langchain_core/messages/utils.py:1344:34:</a> RUF005 Consider `[*messages[:idx], excluded]` instead of concatenation
+ <a href='https://github.com/langchain-ai/langchain/blob/761f8c3231806d4e9e995366773ec79b135c1628/libs/core/langchain_core/messages/utils.py#L1345'>libs/core/langchain_core/messages/utils.py:1345:32:</a> RUF005 Consider `[*messages[:idx], excluded]` instead of concatenation
+ <a href='https://github.com/langchain-ai/langchain/blob/761f8c3231806d4e9e995366773ec79b135c1628/libs/core/langchain_core/messages/utils.py#L1396'>libs/core/langchain_core/messages/utils.py:1396:32:</a> RUF005 Consider `[*messages[:idx], excluded]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/971d6a30f53772e1b03efeca9f8ce0168e803034/tests/pyright_test.py#L40'>tests/pyright_test.py:40:15:</a> RUF005 Consider `[npx, f"pyright@{pyright_version}", *sys.argv[1:]]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L115'>src/poetry/inspection/lazy_wheel.py:115:25:</a> RUF005 Consider `[start, *lslice[:1]]` instead of concatenation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L116'>src/poetry/inspection/lazy_wheel.py:116:19:</a> RUF005 Consider `[end, *rslice[-1:]]` instead of concatenation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_run.py#L133'>tests/console/commands/test_run.py:133:49:</a> RUF005 Consider `[file, *args[1:]]` instead of concatenation
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/tests/console/commands/test_run.py#L165'>tests/console/commands/test_run.py:165:49:</a> RUF005 Consider `[file, *args[1:]]` instead of concatenation
</pre>

</p>
</details>
<details><summary><a href="https://github.com/wntrblm/nox">wntrblm/nox</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/wntrblm/nox/blob/382fc8c36292056d315929dad59cb12e74bdfb92/nox/_resolver.py#L198'>nox/_resolver.py:198:21:</a> RUF005 Consider `[*walk_list[walk_list.index(node):], node]` instead of concatenation
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF005 | 21 | 21 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-06-06 17:40_

---

_Closed by @dylwil3 on 2025-06-06 17:40_

---

_Branch deleted on 2025-06-06 17:40_

---
