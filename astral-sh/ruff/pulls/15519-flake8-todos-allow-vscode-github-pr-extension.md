```yaml
number: 15519
title: "[`flake8-todos`] Allow VSCode GitHub PR extension style links in `missing-todo-link` (`TD003`)"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
assignees: []
merged: true
base: main
head: todos-gh
created_at: 2025-01-15T23:21:58Z
updated_at: 2025-01-15T23:47:33Z
url: https://github.com/astral-sh/ruff/pull/15519
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-todos`] Allow VSCode GitHub PR extension style links in `missing-todo-link` (`TD003`)

---

_@dylwil3_

## Summary
Allow links to issues that appear on the same line as the TODO directive, if they conform to the format that VSCode's GitHub PR extension produces.

Revival of #9627 (the branch was stale enough that rebasing was a lot harder than just making the changes anew). Credit should go to the author of that PR though.

Closes #8061

Co-authored-by: Martin Bernstorff <martinbernstorff@gmail.com>

---

_Label `rule` added by @dylwil3 on 2025-01-15 23:21_

---

_Label `do-not-merge` added by @dylwil3 on 2025-01-15 23:21_

---

_Comment by @github-actions[bot] on 2025-01-15 23:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -28 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/kubernetes_tests/test_kubernetes_pod_operator.py#L1402'>kubernetes_tests/test_kubernetes_pod_operator.py:1402:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/providers/src/airflow/providers/google/cloud/sensors/tasks.py#L81'>providers/src/airflow/providers/google/cloud/sensors/tasks.py:81:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/providers/src/airflow/providers/http/hooks/http.py#L310'>providers/src/airflow/providers/http/hooks/http.py:310:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/src/airflow/sdk/definitions/_internal/abstractoperator.py#L329'>task_sdk/src/airflow/sdk/definitions/_internal/abstractoperator.py:329:19:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/src/airflow/sdk/execution_time/task_runner.py#L505'>task_sdk/src/airflow/sdk/execution_time/task_runner.py:505:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/src/airflow/sdk/execution_time/task_runner.py#L506'>task_sdk/src/airflow/sdk/execution_time/task_runner.py:506:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/tests/defintions/_internal/test_templater.py#L58'>task_sdk/tests/defintions/_internal/test_templater.py:58:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L156'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:156:29:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/fc8710f50ae5ec2f7280d1f14ae54d3a96300ac1/superset/sql_parse.py#L87'>superset/sql_parse.py:87:3:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L361'>src/bokeh/layouts.py:361:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L247'>src/bokeh/resources.py:247:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_color_picker.py#L107'>tests/integration/widgets/test_color_picker.py:107:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_daterange_slider.py#L169'>tests/integration/widgets/test_daterange_slider.py:169:13:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_daterange_slider.py#L192'>tests/integration/widgets/test_daterange_slider.py:192:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L163'>tests/integration/widgets/test_dateslider.py:163:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L197'>tests/integration/widgets/test_dateslider.py:197:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L220'>tests/integration/widgets/test_dateslider.py:220:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L243'>tests/integration/widgets/test_dateslider.py:243:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_range_slider.py#L220'>tests/integration/widgets/test_range_slider.py:220:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_range_slider.py#L243'>tests/integration/widgets/test_range_slider.py:243:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L186'>tests/integration/widgets/test_slider.py:186:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L220'>tests/integration/widgets/test_slider.py:220:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L243'>tests/integration/widgets/test_slider.py:243:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L266'>tests/integration/widgets/test_slider.py:266:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_spinner.py#L228'>tests/integration/widgets/test_spinner.py:228:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/project.py#L162'>tests/support/plugins/project.py:162:15:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/views/test_multi_root_static_handler.py#L45'>tests/unit/bokeh/server/views/test_multi_root_static_handler.py:45:50:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e963d5eaa9d73faa31b9c9e1463a2ac0a5169e7a/zerver/lib/message_cache.py#L452'>zerver/lib/message_cache.py:452:15:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TD003 | 28 | 0 | 28 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -28 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/kubernetes_tests/test_kubernetes_pod_operator.py#L1402'>kubernetes_tests/test_kubernetes_pod_operator.py:1402:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/providers/src/airflow/providers/google/cloud/sensors/tasks.py#L81'>providers/src/airflow/providers/google/cloud/sensors/tasks.py:81:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/providers/src/airflow/providers/http/hooks/http.py#L310'>providers/src/airflow/providers/http/hooks/http.py:310:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/src/airflow/sdk/definitions/_internal/abstractoperator.py#L329'>task_sdk/src/airflow/sdk/definitions/_internal/abstractoperator.py:329:19:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/src/airflow/sdk/execution_time/task_runner.py#L505'>task_sdk/src/airflow/sdk/execution_time/task_runner.py:505:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/src/airflow/sdk/execution_time/task_runner.py#L506'>task_sdk/src/airflow/sdk/execution_time/task_runner.py:506:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/task_sdk/tests/defintions/_internal/test_templater.py#L58'>task_sdk/tests/defintions/_internal/test_templater.py:58:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/apache/airflow/blob/8c420b122d01f14cbe44befe44086aa457146b3b/tests/ti_deps/deps/test_mapped_task_upstream_dep.py#L156'>tests/ti_deps/deps/test_mapped_task_upstream_dep.py:156:29:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/fc8710f50ae5ec2f7280d1f14ae54d3a96300ac1/superset/sql_parse.py#L87'>superset/sql_parse.py:87:3:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -18 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/layouts.py#L361'>src/bokeh/layouts.py:361:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/resources.py#L247'>src/bokeh/resources.py:247:3:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_color_picker.py#L107'>tests/integration/widgets/test_color_picker.py:107:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_daterange_slider.py#L169'>tests/integration/widgets/test_daterange_slider.py:169:13:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_daterange_slider.py#L192'>tests/integration/widgets/test_daterange_slider.py:192:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L163'>tests/integration/widgets/test_dateslider.py:163:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L197'>tests/integration/widgets/test_dateslider.py:197:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L220'>tests/integration/widgets/test_dateslider.py:220:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_dateslider.py#L243'>tests/integration/widgets/test_dateslider.py:243:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_range_slider.py#L220'>tests/integration/widgets/test_range_slider.py:220:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_range_slider.py#L243'>tests/integration/widgets/test_range_slider.py:243:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L186'>tests/integration/widgets/test_slider.py:186:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L220'>tests/integration/widgets/test_slider.py:220:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L243'>tests/integration/widgets/test_slider.py:243:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_slider.py#L266'>tests/integration/widgets/test_slider.py:266:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/integration/widgets/test_spinner.py#L228'>tests/integration/widgets/test_spinner.py:228:11:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/project.py#L162'>tests/support/plugins/project.py:162:15:</a> TD003 Missing issue link on the line following this TODO
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/server/views/test_multi_root_static_handler.py#L45'>tests/unit/bokeh/server/views/test_multi_root_static_handler.py:45:50:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/e963d5eaa9d73faa31b9c9e1463a2ac0a5169e7a/zerver/lib/message_cache.py#L452'>zerver/lib/message_cache.py:452:15:</a> TD003 Missing issue link on the line following this TODO
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TD003 | 28 | 0 | 28 | 0 | 0 |

</p>
</details>




---

_Comment by @AlexWaygood on 2025-01-15 23:29_

> Revival of #9627 (the branch was stale enough that rebasing was a lot harder than just making the changes anew). Credit should go to the author of that PR though.

I edited your PR message so that the commit should show up as co-authored by Martin if this PR is merged :-)

---

_Comment by @dylwil3 on 2025-01-15 23:31_

> I edited your PR message so that the commit should show up as co-authored by Martin if this PR is merged :-)

Perfect, I was just looking up how to do that!

---

_Comment by @dylwil3 on 2025-01-15 23:32_

In other news, these ecosystem checks look correct to me. All of these folks dutifully putting issue links in their TODOs and getting yelled at by TD003... such injustice.

---

_Label `do-not-merge` removed by @dylwil3 on 2025-01-15 23:35_

---

_Merged by @dylwil3 on 2025-01-15 23:47_

---

_Closed by @dylwil3 on 2025-01-15 23:47_

---
