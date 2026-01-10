```yaml
number: 20100
title: "[`flake8-use-pathlib`] Make `PTH100` fix unsafe because it can change behavior"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth100
created_at: 2025-08-26T16:23:50Z
updated_at: 2025-08-26T19:09:36Z
url: https://github.com/astral-sh/ruff/pull/20100
synced_at: 2026-01-10T17:46:21Z
```

# [`flake8-use-pathlib`] Make `PTH100` fix unsafe because it can change behavior

---

_Pull request opened by @chirizxc on 2025-08-26 16:23_


<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes https://github.com/astral-sh/ruff/issues/20088
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo nextest run flake8_use_pathlib`


---

_Comment by @github-actions[bot] on 2025-08-26 16:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -208 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -56 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/docs/conf.py#L330'>airflow-core/docs/conf.py:330:29:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/docs/conf.py#L330'>airflow-core/docs/conf.py:330:29:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/config_templates/default_webserver_config.py#L32'>airflow-core/src/airflow/config_templates/default_webserver_config.py:32:11:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/config_templates/default_webserver_config.py#L32'>airflow-core/src/airflow/config_templates/default_webserver_config.py:32:11:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/utils/cli.py#L224'>airflow-core/src/airflow/utils/cli.py:224:18:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/utils/cli.py#L224'>airflow-core/src/airflow/utils/cli.py:224:18:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/utils/cli.py#L363'>airflow-core/src/airflow/utils/cli.py:363:15:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/utils/cli.py#L363'>airflow-core/src/airflow/utils/cli.py:363:15:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/utils/log/file_processor_handler.py#L145'>airflow-core/src/airflow/utils/log/file_processor_handler.py:145:25:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/src/airflow/utils/log/file_processor_handler.py#L145'>airflow-core/src/airflow/utils/log/file_processor_handler.py:145:25:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L114'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:114:34:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py#L114'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_between_tasks.py:114:34:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/airflow/blob/c8a0f51f89dc5c75b86c6888154073c08c2a0671/airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py#L50'>airflow-core/tests/integration/otel/dags/otel_test_dag_with_pause_in_task.py:50:34:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
... 43 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/docker/pythonpath_dev/superset_config.py#L124'>docker/pythonpath_dev/superset_config.py:124:21:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/docker/pythonpath_dev/superset_config.py#L124'>docker/pythonpath_dev/superset_config.py:124:21:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/scripts/cypress_run.py#L135'>scripts/cypress_run.py:135:34:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/scripts/cypress_run.py#L135'>scripts/cypress_run.py:135:34:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/setup.py#L23'>setup.py:23:12:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/setup.py#L23'>setup.py:23:12:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/superset/cli/update.py#L76'>superset/cli/update.py:76:20:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/superset/cli/update.py#L76'>superset/cli/update.py:76:20:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/superset/translations/utils.py#L27'>superset/translations/utils.py:27:23:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/apache/superset/blob/659db162d65d8856d871b64e6dda4d16a906561d/superset/translations/utils.py#L27'>superset/translations/utils.py:27:23:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -44 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L9'>scripts/sri.py:9:7:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/sri.py#L9'>scripts/sri.py:9:7:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L182'>src/bokeh/application/handlers/code_runner.py:182:39:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L182'>src/bokeh/application/handlers/code_runner.py:182:39:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L924'>src/bokeh/command/subcommands/serve.py:924:20:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/serve.py#L924'>src/bokeh/command/subcommands/serve.py:924:20:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L117'>src/bokeh/command/util.py:117:12:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L117'>src/bokeh/command/util.py:117:12:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L277'>src/bokeh/embed/bundle.py:277:21:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/embed/bundle.py#L277'>src/bokeh/embed/bundle.py:277:21:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
... 34 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/778cf7a02ca46bd708ae952d39cdd6691604464d/docs/source/conf.py#L18'>docs/source/conf.py:18:20:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/latchbio/latch/blob/778cf7a02ca46bd708ae952d39cdd6691604464d/docs/source/conf.py#L18'>docs/source/conf.py:18:20:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/8c167bbe0082d212d5d506983a62131740155d2d/docs/source/conf.py#L15'>docs/source/conf.py:15:20:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/milvus-io/pymilvus/blob/8c167bbe0082d212d5d506983a62131740155d2d/docs/source/conf.py#L15'>docs/source/conf.py:15:20:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -94 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/docs/conf.py#L11'>docs/conf.py:11:20:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/docs/conf.py#L11'>docs/conf.py:11:20:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/manage.py#L7'>manage.py:7:28:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/manage.py#L7'>manage.py:7:28:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:62:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/check_rabbitmq_queue.py#L10'>scripts/lib/check_rabbitmq_queue.py:10:62:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/clean_emoji_cache.py#L6'>scripts/lib/clean_emoji_cache.py:6:62:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/clean_emoji_cache.py#L6'>scripts/lib/clean_emoji_cache.py:6:62:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/clean_unused_caches.py#L7'>scripts/lib/clean_unused_caches.py:7:62:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/clean_unused_caches.py#L7'>scripts/lib/clean_unused_caches.py:7:62:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/puppet_cache.py#L13'>scripts/lib/puppet_cache.py:13:62:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/puppet_cache.py#L13'>scripts/lib/puppet_cache.py:13:62:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/queue_workers.py#L5'>scripts/lib/queue_workers.py:5:60:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/queue_workers.py#L5'>scripts/lib/queue_workers.py:5:60:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/setup_path.py#L10'>scripts/lib/setup_path.py:10:64:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/setup_path.py#L10'>scripts/lib/setup_path.py:10:64:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/setup_venv.py#L5'>scripts/lib/setup_venv.py:5:62:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/setup_venv.py#L5'>scripts/lib/setup_venv.py:5:62:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/sharding.py#L9'>scripts/lib/sharding.py:9:60:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/sharding.py#L9'>scripts/lib/sharding.py:9:60:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
- <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/zulip_tools.py#L562'>scripts/lib/zulip_tools.py:562:19:</a> PTH100 [*] `os.path.abspath()` should be replaced by `Path.resolve()`
+ <a href='https://github.com/zulip/zulip/blob/baecf159555628d8e2f0be5dcc95727bf7ec7f47/scripts/lib/zulip_tools.py#L562'>scripts/lib/zulip_tools.py:562:19:</a> PTH100 `os.path.abspath()` should be replaced by `Path.resolve()`
... 72 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH100 | 208 | 0 | 0 | 0 | 208 |

</p>
</details>




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:115 on 2025-08-26 17:08_

Since it's always unsafe, we can just use `Fix::unsafe_edits` instead of `applicable_edits`.


```suggestion
        Ok(Fix::unsafe_edits(
            Edit::range_replacement(replacement, range),
            [import_edit],
        ))
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:47 on 2025-08-26 17:17_

I don't think it's so important to mention the comment issue now that the rule is always unsafe. I'd probably say something like:

> This rule's fix is always marked as unsafe because `Path.resolve()` resolves symlinks, while `os.path.abspath()` does not. If resolving symlinks is important, you may need to use `Path.absolute()`. However, `Path.absolute()` also does not remove any `..` components in a path, unlike `os.path.abspath()` and `Path.resolve()`, so if that specific combination of behaviors is required, there's no existing `pathlib` alternative. See CPython issue [#69200](https://github.com/python/cpython/issues/69200).

That's a bit wordy, so it could be nice to include @dscorbett's nice table from the issue instead, up to you.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:47 on 2025-08-26 17:19_

I can't comment on the line below, but as I mentioned on the issue, we should also update the `Correspondence between os and pathlib` link below (https://github.com/astral-sh/ruff/issues/20088#issuecomment-3225030199). The new link is https://docs.python.org/3/library/pathlib.html#corresponding-tools

---

_@ntBre reviewed on 2025-08-26 17:40_

Thanks! Just a couple of small suggestions.

---

_Label `fixes` added by @ntBre on 2025-08-26 17:41_

---

_Label `preview` added by @ntBre on 2025-08-26 17:41_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:47 on 2025-08-26 17:57_

It seems to me that if a fix is going to change behavior in any way, it's better to leave it as a unsafe

---

_@chirizxc reviewed on 2025-08-26 17:57_

---

_Comment by @chirizxc on 2025-08-26 17:58_

Should I change links in this PR in the same way in all `PTH` rules?

---

_Comment by @chirizxc on 2025-08-26 18:33_

I also see that there are many more functions in the table for which there are no rules and fixes, can we start adding them?

---

_Comment by @ntBre on 2025-08-26 18:53_

> Should I change links in this PR in the same way in all `PTH` rules?

Sure, but let's do that in a follow-up documentation PR. I think this one is good to go now.

> I also see that there are many more functions in the table for which there are no rules and fixes, can we start adding them?

I think we've implemented all of the rules in the upstream `flake8-use-pathlib`, and it's not a high priority to add more rules right now, so I don't think we need to exhaustively cover the table. You could open an issue to track interest in any of the ones we're missing, though, if you want!

---

_@ntBre approved on 2025-08-26 18:56_

---

_Merged by @ntBre on 2025-08-26 18:59_

---

_Closed by @ntBre on 2025-08-26 18:59_

---

_Branch deleted on 2025-08-26 19:09_

---
