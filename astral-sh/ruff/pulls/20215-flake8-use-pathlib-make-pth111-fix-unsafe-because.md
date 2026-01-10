```yaml
number: 20215
title: "[`flake8-use-pathlib`] Make `PTH111` fix unsafe because it can change behavior"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: pth111
created_at: 2025-09-03T15:14:19Z
updated_at: 2025-09-17T15:24:52Z
url: https://github.com/astral-sh/ruff/pull/20215
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-use-pathlib`] Make `PTH111` fix unsafe because it can change behavior

---

_Pull request opened by @chirizxc on 2025-09-03 15:14_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/20214

## Test Plan



---

_Comment by @github-actions[bot] on 2025-09-03 15:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -74 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -52 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/cli/commands/info_command.py#L73'>airflow-core/src/airflow/cli/commands/info_command.py:73:21:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/cli/commands/info_command.py#L73'>airflow-core/src/airflow/cli/commands/info_command.py:73:21:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/config_templates/airflow_local_settings.py#L48'>airflow-core/src/airflow/config_templates/airflow_local_settings.py:48:24:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/config_templates/airflow_local_settings.py#L48'>airflow-core/src/airflow/config_templates/airflow_local_settings.py:48:24:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/configuration.py#L124'>airflow-core/src/airflow/configuration.py:124:24:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/configuration.py#L124'>airflow-core/src/airflow/configuration.py:124:24:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/settings.py#L105'>airflow-core/src/airflow/settings.py:105:20:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/settings.py#L105'>airflow-core/src/airflow/settings.py:105:20:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/settings.py#L245'>airflow-core/src/airflow/settings.py:245:19:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/settings.py#L245'>airflow-core/src/airflow/settings.py:245:19:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/utils/cli.py#L226'>airflow-core/src/airflow/utils/cli.py:226:34:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/utils/cli.py#L226'>airflow-core/src/airflow/utils/cli.py:226:34:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/utils/log/file_processor_handler.py#L49'>airflow-core/src/airflow/utils/log/file_processor_handler.py:49:24:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/utils/log/file_processor_handler.py#L49'>airflow-core/src/airflow/utils/log/file_processor_handler.py:49:24:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/utils/serve_logs/log_server.py#L117'>airflow-core/src/airflow/utils/serve_logs/log_server.py:117:21:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/src/airflow/utils/serve_logs/log_server.py#L117'>airflow-core/src/airflow/utils/serve_logs/log_server.py:117:21:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/tests/unit/cli/commands/test_info_command.py#L49'>airflow-core/tests/unit/cli/commands/test_info_command.py:49:21:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/tests/unit/cli/commands/test_info_command.py#L49'>airflow-core/tests/unit/cli/commands/test_info_command.py:49:21:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/tests/unit/core/test_configuration.py#L58'>airflow-core/tests/unit/core/test_configuration.py:58:12:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-core/tests/unit/core/test_configuration.py#L58'>airflow-core/tests/unit/core/test_configuration.py:58:12:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/src/airflowctl/api/client.py#L132'>airflow-ctl/src/airflowctl/api/client.py:132:61:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/src/airflowctl/api/client.py#L132'>airflow-ctl/src/airflowctl/api/client.py:132:61:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/src/airflowctl/api/client.py#L147'>airflow-ctl/src/airflowctl/api/client.py:147:61:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/src/airflowctl/api/client.py#L147'>airflow-ctl/src/airflowctl/api/client.py:147:61:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/tests/airflow_ctl/api/test_client.py#L117'>airflow-ctl/tests/airflow_ctl/api/test_client.py:117:53:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/tests/airflow_ctl/api/test_client.py#L117'>airflow-ctl/tests/airflow_ctl/api/test_client.py:117:53:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/tests/airflow_ctl/api/test_client.py#L137'>airflow-ctl/tests/airflow_ctl/api/test_client.py:137:53:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/tests/airflow_ctl/api/test_client.py#L137'>airflow-ctl/tests/airflow_ctl/api/test_client.py:137:53:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/tests/airflow_ctl/api/test_client.py#L159'>airflow-ctl/tests/airflow_ctl/api/test_client.py:159:53:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/airflow-ctl/tests/airflow_ctl/api/test_client.py#L159'>airflow-ctl/tests/airflow_ctl/api/test_client.py:159:53:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L100'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:100:17:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/dev/breeze/src/airflow_breeze/commands/ci_commands.py#L100'>dev/breeze/src/airflow_breeze/commands/ci_commands.py:100:17:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/devel-common/src/sphinx_exts/sphinx_script_update.py#L51'>devel-common/src/sphinx_exts/sphinx_script_update.py:51:16:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/devel-common/src/sphinx_exts/sphinx_script_update.py#L51'>devel-common/src/sphinx_exts/sphinx_script_update.py:51:16:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/apache/airflow/blob/7fee3e9002374edd2ae61938469630d3fea0fe16/devel-common/src/sphinx_exts/sphinx_script_update.py#L53'>devel-common/src/sphinx_exts/sphinx_script_update.py:53:44:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
... 17 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/superset/config.py#L99'>superset/config.py:99:16:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/apache/superset/blob/3e554674ff3b994b8d73f42737e6fc89ee96c22c/superset/config.py#L99'>superset/config.py:99:16:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +0 -8 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L117'>src/bokeh/command/util.py:117:28:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/util.py#L117'>src/bokeh/command/util.py:117:28:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L131'>src/bokeh/io/export.py:131:20:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L131'>src/bokeh/io/export.py:131:20:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/saving.py#L99'>src/bokeh/io/saving.py:99:20:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/saving.py#L99'>src/bokeh/io/saving.py:99:20:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L513'>src/bokeh/settings.py:513:10:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/settings.py#L513'>src/bokeh/settings.py:513:10:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch_cli/centromere/utils.py#L133'>src/latch_cli/centromere/utils.py:133:46:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/latchbio/latch/blob/277eff6188e9cd65e02988c449d3ec5bb4134648/src/latch_cli/centromere/utils.py#L133'>src/latch_cli/centromere/utils.py:133:46:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -0 violations, +0 -10 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/tools/droplets/add_mentor.py#L50'>tools/droplets/add_mentor.py:50:23:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/tools/droplets/add_mentor.py#L50'>tools/droplets/add_mentor.py:50:23:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/tools/lib/provision_inner.py#L123'>tools/lib/provision_inner.py:123:26:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/tools/lib/provision_inner.py#L123'>tools/lib/provision_inner.py:123:26:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/tools/lib/provision_inner.py#L165'>tools/lib/provision_inner.py:165:9:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/tools/lib/provision_inner.py#L165'>tools/lib/provision_inner.py:165:9:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/zerver/management/commands/export.py#L146'>zerver/management/commands/export.py:146:43:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/zerver/management/commands/export.py#L146'>zerver/management/commands/export.py:146:43:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
- <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/zerver/management/commands/import.py#L96'>zerver/management/commands/import.py:96:37:</a> PTH111 [*] `os.path.expanduser()` should be replaced by `Path.expanduser()`
+ <a href='https://github.com/zulip/zulip/blob/35ed477b324b0b8dbbe14a71498ff3a0c97b66f3/zerver/management/commands/import.py#L96'>zerver/management/commands/import.py:96:37:</a> PTH111 `os.path.expanduser()` should be replaced by `Path.expanduser()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH111 | 74 | 0 | 0 | 0 | 74 |

</p>
</details>




---

_Comment by @dscorbett on 2025-09-05 03:30_

~[Apparently](https://github.com/search?q=language%3APython+%2Fexpanduser%5C%28%5Bfr%5D*%5B%27%22%5D%5B%5E%7E%7B%5D%2F&type=code), it’s not uncommon for `os.path.expanduser` to be called on a string literal that doesn’t start with `"~"`, in which case the PTH111 fix changes a (probably unintended) no-op into a guaranteed error. Should the fix be suppressed or changed in that case?~ This is not true.

---

_Comment by @chirizxc on 2025-09-05 16:08_

> [Apparently](https://github.com/search?q=language%3APython+%2Fexpanduser%5C%28%5Bfr%5D*%5B%27%22%5D%5B%5E%7E%7B%5D%2F&type=code), it’s not uncommon for `os.path.expanduser` to be called on a string literal that doesn’t start with `"~"`, in which case the PTH111 fix changes a (probably unintended) no-op into a guaranteed error. Should the fix be suppressed or changed in that case?

I think in that case it's better to offer only diagnostic

---

_Converted to draft by @chirizxc on 2025-09-05 16:12_

---

_Comment by @eli-schwartz on 2025-09-16 04:05_

> [Apparently](https://github.com/search?q=language%3APython+%2Fexpanduser%5C%28%5Bfr%5D*%5B%27%22%5D%5B%5E%7E%7B%5D%2F&type=code), it’s not uncommon for `os.path.expanduser` to be called on a string literal that doesn’t start with `"~"`, in which case the PTH111 fix changes a (probably unintended) no-op into a guaranteed error. Should the fix be suppressed or changed in that case?

It which?

```python
>>> pathlib.Path('.').expanduser()
PosixPath('.')
```

There's nothing to catch unless a genuine ~ exists.

---

_Comment by @ntBre on 2025-09-17 14:01_

@chirizxc is this ready for review? It sounds like the `~` question is resolved, so I think it's still fine just to mark the fix as unsafe.

---

_Comment by @chirizxc on 2025-09-17 14:19_

> @chirizxc is this ready for review? It sounds like the `~` question is resolved, so I think it's still fine just to mark the fix as unsafe.

yes

---

_Marked ready for review by @chirizxc on 2025-09-17 14:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:80 on 2025-09-17 14:28_

This is a different rule, not PTH111 right? I'd prefer moving this change to a separate PR to keep this one focused on the fix safety issue.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_expanduser.rs`:42 on 2025-09-17 14:31_

```suggestion
/// This rule's fix is always marked as unsafe because the behaviors of 
/// `os.path.expanduser` and `Path.expanduser` differ when a user's home 
/// directory can't be resolved: `os.path.expanduser` returns the
/// input unchanged, while `Path.expanduser` raises `RuntimeError`.
```

I think something like that sounds a little nicer, but feel free to tweak it further if you prefer.

---

_@ntBre reviewed on 2025-09-17 14:34_

Thanks! Just a couple of minor suggestions.

---

_Label `fixes` added by @ntBre on 2025-09-17 14:34_

---

_Label `preview` added by @ntBre on 2025-09-17 14:34_

---

_@chirizxc reviewed on 2025-09-17 14:50_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_path_abspath.rs`:80 on 2025-09-17 14:50_

I've sent this change as a separate PR

---

_@ntBre approved on 2025-09-17 15:23_

---

_Merged by @ntBre on 2025-09-17 15:23_

---

_Closed by @ntBre on 2025-09-17 15:23_

---

_Branch deleted on 2025-09-17 15:24_

---
