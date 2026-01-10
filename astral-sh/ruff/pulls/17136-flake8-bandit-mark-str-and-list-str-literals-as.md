```yaml
number: 17136
title: "[`flake8-bandit`] Mark `str` and `list[str]` literals as trusted input (`S603`)"
type: pull_request
state: merged
author: trag1c
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: s603-safe-literal
created_at: 2025-04-01T22:19:04Z
updated_at: 2025-05-02T19:53:08Z
url: https://github.com/astral-sh/ruff/pull/17136
synced_at: 2026-01-10T18:57:02Z
```

# [`flake8-bandit`] Mark `str` and `list[str]` literals as trusted input (`S603`)

---

_Pull request opened by @trag1c on 2025-04-01 22:19_

## Summary

Closes #17112. Allows passing in string and list-of-strings literals into `subprocess.run` (and related) calls without marking them as untrusted input:
```py
import subprocess

subprocess.run("true")

# "instant" named expressions are also allowed
subprocess.run(c := "ls")
```

## Test Plan

Added test cases covering new behavior, passed with `cargo nextest run`.


---

_Comment by @github-actions[bot] on 2025-04-01 22:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -78 violations, +0 -0 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -59 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/airflow-core/src/airflow/cli/commands/dag_command.py#L269'>airflow-core/src/airflow/cli/commands/dag_command.py:269:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/airflow-core/tests/unit/utils/test_process_utils.py#L146'>airflow-core/tests/unit/utils/test_process_utils.py:146:30:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/airflow-core/tests/unit/utils/test_process_utils.py#L152'>airflow-core/tests/unit/utils/test_process_utils.py:152:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/airflow-core/tests/unit/utils/test_process_utils.py#L157'>airflow-core/tests/unit/utils/test_process_utils.py:157:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/airflow-core/tests/unit/utils/test_process_utils.py#L165'>airflow-core/tests/unit/utils/test_process_utils.py:165:25:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/airflow-core/tests/unit/utils/test_process_utils.py#L173'>airflow-core/tests/unit/utils/test_process_utils.py:173:25:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/dev/breeze/src/airflow_breeze/commands/main_command.py#L138'>dev/breeze/src/airflow_breeze/commands/main_command.py:138:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/dev/breeze/src/airflow_breeze/commands/main_command.py#L184'>dev/breeze/src/airflow_breeze/commands/main_command.py:184:27:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L151'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:151:13:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L153'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:153:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/dev/breeze/src/airflow_breeze/utils/reinstall.py#L43'>dev/breeze/src/airflow_breeze/utils/reinstall.py:43:21:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/dev/breeze/src/airflow_breeze/utils/reinstall.py#L50'>dev/breeze/src/airflow_breeze/utils/reinstall.py:50:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/kubernetes-tests/tests/kubernetes_tests/test_base.py#L124'>kubernetes-tests/tests/kubernetes_tests/test_base.py:124:19:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/kubernetes-tests/tests/kubernetes_tests/test_base.py#L239'>kubernetes-tests/tests/kubernetes_tests/test_base.py:239:21:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py#L78'>providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py:78:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py#L79'>providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py:79:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L67'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:67:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L68'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:68:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L83'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:83:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L84'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:84:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/breeze_cmd_line.py#L63'>scripts/ci/pre_commit/breeze_cmd_line.py:63:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/breeze_cmd_line.py#L79'>scripts/ci/pre_commit/breeze_cmd_line.py:79:11:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/breeze_cmd_line.py#L88'>scripts/ci/pre_commit/breeze_cmd_line.py:88:7:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/check_kubeconform.py#L30'>scripts/ci/pre_commit/check_kubeconform.py:30:13:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/check_kubeconform.py#L43'>scripts/ci/pre_commit/check_kubeconform.py:43:10:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/check_min_python_version.py#L34'>scripts/ci/pre_commit/check_min_python_version.py:34:9:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_fab_assets.py#L76'>scripts/ci/pre_commit/compile_fab_assets.py:76:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_fab_assets.py#L88'>scripts/ci/pre_commit/compile_fab_assets.py:88:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L35'>scripts/ci/pre_commit/compile_lint_ui.py:35:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L36'>scripts/ci/pre_commit/compile_lint_ui.py:36:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L39'>scripts/ci/pre_commit/compile_lint_ui.py:39:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L40'>scripts/ci/pre_commit/compile_lint_ui.py:40:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L41'>scripts/ci/pre_commit/compile_lint_ui.py:41:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L44'>scripts/ci/pre_commit/compile_lint_ui.py:44:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/6f26a388b25ba61914cf1de6d2c2bb551126b5e2/scripts/ci/pre_commit/compile_lint_ui.py#L46'>scripts/ci/pre_commit/compile_lint_ui.py:46:5:</a> S603 `subprocess` call: check for execution of untrusted input
... 24 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/4f0020d0df9634b06a2c70699c3ed5b88cffee7c/scripts/change_detector.py#L103'>scripts/change_detector.py:103:5:</a> DOC501 Raised exception `ValueError` missing from docstring
- <a href='https://github.com/apache/superset/blob/4f0020d0df9634b06a2c70699c3ed5b88cffee7c/scripts/change_detector.py#L103'>scripts/change_detector.py:103:5:</a> DOC501 Raised exception `ValueError` missing from docstring
+ <a href='https://github.com/apache/superset/blob/4f0020d0df9634b06a2c70699c3ed5b88cffee7c/scripts/change_detector.py#L142'>scripts/change_detector.py:142:65:</a> RUF100 [*] Unused `noqa` directive (unused: `S603`)
+ <a href='https://github.com/apache/superset/blob/4f0020d0df9634b06a2c70699c3ed5b88cffee7c/setup.py#L33'>setup.py:33:73:</a> RUF100 [*] Unused `noqa` directive (unused: `S603`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/output/apis/server_document/flask_server.py#L45'>examples/output/apis/server_document/flask_server.py:45:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/scripts/hooks/protect_branches.py#L10'>scripts/hooks/protect_branches.py:10:22:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L52'>setup.py:52:16:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_code_quality.py#L118'>tests/codebase/test_code_quality.py:118:13:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_eslint.py#L37'>tests/codebase/test_eslint.py:37:12:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_no_request_host.py#L50'>tests/codebase/test_no_request_host.py:50:13:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/codebase/test_ruff.py#L33'>tests/codebase/test_ruff.py:33:12:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/test_bokehjs.py#L34'>tests/test_bokehjs.py:34:16:</a> S603 `subprocess` call: check for execution of untrusted input
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/dcfc0299704a978770d26ceadb60f115495ff205/rotkehlchen/tests/unit/test_search.py#L60'>rotkehlchen/tests/unit/test_search.py:60:40:</a> RUF100 [*] Unused `noqa` directive (unused: `S603`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -10 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/scripts/lib/check_rabbitmq_queue.py#L146'>scripts/lib/check_rabbitmq_queue.py:146:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/scripts/lib/puppet_cache.py#L25'>scripts/lib/puppet_cache.py:25:30:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/tools/lib/provision.py#L271'>tools/lib/provision.py:271:16:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/tools/lib/provision_inner.py#L109'>tools/lib/provision_inner.py:109:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/tools/lib/test_script.py#L125'>tools/lib/test_script.py:125:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/zerver/data_import/mattermost.py#L443'>zerver/data_import/mattermost.py:443:19:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/zerver/lib/test_fixtures.py#L344'>zerver/lib/test_fixtures.py:344:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/zerver/logging_handlers.py#L8'>zerver/logging_handlers.py:8:16:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/zerver/management/commands/compilemessages.py#L76'>zerver/management/commands/compilemessages.py:76:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/zulip/zulip/blob/f145d5738e4a2ae296e2b51ebd725e303a306d6e/zerver/management/commands/makemessages.py#L207'>zerver/management/commands/makemessages.py:207:21:</a> S603 `subprocess` call: check for execution of untrusted input
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S603 | 77 | 0 | 77 | 0 | 0 |
| RUF100 | 3 | 3 | 0 | 0 | 0 |
| DOC501 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_Review requested from @ntBre by @MichaReiser on 2025-04-02 07:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:12 on 2025-04-02 07:51_

Hmm, it seems it was very deliberate to flag string literals. I wonder why

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:34 on 2025-04-02 07:53_

I don't think I'd allow references because someone could call `cmd.push(inject)` and that would lead to a false negative. That's why I think we should only allow string literals and arrays of string literals

---

_@MichaReiser reviewed on 2025-04-02 07:53_

---

_Comment by @MichaReiser on 2025-04-02 07:53_

We may want to make this a preview only change, as it changes the rule's scope significantly (as seen by the ecosystem checks)

---

_@MichaReiser reviewed on 2025-04-02 07:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:12 on 2025-04-02 07:56_

Okay, upstream seems to agree that string literals should be safe: https://github.com/PyCQA/bandit/issues/333#issuecomment-404233717

---

_Review comment by @trag1c on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:34 on 2025-04-02 10:58_

Very fair, I'll disallow those then 👍

---

_@trag1c reviewed on 2025-04-02 10:58_

---

_Label `rule` added by @MichaReiser on 2025-04-02 11:03_

---

_@trag1c reviewed on 2025-04-02 11:33_

---

_Review comment by @trag1c on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:34 on 2025-04-02 11:33_

Done, I chose to still allow `run(cmd := "true")` if that's fine though.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:4 on 2025-04-02 13:09_

Is it worth keeping these old cases to show that they are all allowed now? It looks like we only have two literal cases left, but maybe that's enough.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/shell_injection.rs`:295 on 2025-04-02 13:10_

nit: `ruff_python_ast` is already aliased to `ast` up above, so you can use that.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/shell_injection.rs`:328 on 2025-04-02 13:13_

nit: Can these two conditions be joined with `&&` instead of nesting?

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:11 on 2025-04-02 13:16_

I know you didn't add this comment, but it reads pretty strangely. Is it supposed to be "Falsey values are treated as false" possibly?

---

_@ntBre approved on 2025-04-02 13:22_

Nice! This looks good to me besides a few minor comments and gating this behind preview, like Micha said. I think you just need something like `if !is_trusted_input(arg) || checker.preview.is_disabled() { ... }` because the match case is otherwise the same as before right?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/shell_injection.rs`:328 on 2025-04-02 13:23_

This might actually look better as it is if you add the `preview` check here.

---

_@ntBre reviewed on 2025-04-02 13:23_

---

_@trag1c reviewed on 2025-04-02 13:28_

---

_Review comment by @trag1c on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:4 on 2025-04-02 13:28_

I can throw in a few of the old ones with explicit `shell=False` back for clarity.

---

_@trag1c reviewed on 2025-04-02 13:28_

---

_Review comment by @trag1c on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S603.py`:11 on 2025-04-02 13:28_

Haha it threw me off-guard too; yeah I think that's what it's supposed to say, will correct that.

---

_@trag1c reviewed on 2025-04-02 13:33_

---

_Review comment by @trag1c on `crates/ruff_linter/src/rules/flake8_bandit/rules/shell_injection.rs`:328 on 2025-04-02 13:33_

I was going to have these joined initially but I looked around the code and found a nested condition so I thought that's preferred 😄 (presumably better for grepping?).


---

_Comment by @trag1c on 2025-04-02 13:50_

> because the match case is otherwise the same as before right?

Yep, I simplified that a bit from `Some(true) => S604, Some(false) => S603, None => S603` to `Some(true) => S604, _ => S603` to deduplicate the identical arms 🙂

<sub>also TIL <kbd>cmd+enter</kbd> makes you post the comment 🤦</sub>

---

_Review requested from @ntBre by @trag1c on 2025-04-02 14:14_

---

_@ntBre approved on 2025-04-02 15:21_

This is great, thanks! I also went through the ecosystem check, and everything looks good to me. Even the new RUF100 lints are true because the S603 `noqa` comments are no longer needed in a few places!

---

_Label `preview` added by @ntBre on 2025-04-02 15:21_

---

_Merged by @ntBre on 2025-04-02 15:22_

---

_Closed by @ntBre on 2025-04-02 15:22_

---

_Branch deleted on 2025-04-02 15:25_

---

_Comment by @Avasam on 2025-05-02 19:53_

Hi.
This update doesn't work on tuples
![image](https://github.com/user-attachments/assets/aa447131-b564-4ef0-bb6f-a2979bda3c13)


---
