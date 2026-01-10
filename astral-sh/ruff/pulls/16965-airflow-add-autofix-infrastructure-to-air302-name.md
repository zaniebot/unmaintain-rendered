```yaml
number: 16965
title: "[`airflow`] Add autofix infrastructure to `AIR302` name checks"
type: pull_request
state: merged
author: Lee-W
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: autofix-AIR302
created_at: 2025-03-25T11:11:06Z
updated_at: 2025-04-02T15:31:22Z
url: https://github.com/astral-sh/ruff/pull/16965
synced_at: 2026-01-10T19:40:36Z
```

# [`airflow`] Add autofix infrastructure to `AIR302` name checks

---

_Pull request opened by @Lee-W on 2025-03-25 11:11_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Add autofix logic to `"airflow", "api_connexion", "security", "requires_access_dataset"`, `"airflow", "Dataset"` and `"airflow", "datasets", "Dataset"`

## Test Plan

<!-- How was it tested? -->

The existing test fixture reflects the update


---

_Comment by @Lee-W on 2025-03-25 11:13_

This is intentionally to be kept simple for easier review. I can update the rest nams in this or the following PR after we agree on how this should work. Thanks!

---

_Comment by @github-actions[bot] on 2025-03-25 11:17_

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
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/src/airflow/cli/commands/dag_command.py#L269'>airflow-core/src/airflow/cli/commands/dag_command.py:269:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/utils/test_process_utils.py#L146'>airflow-core/tests/unit/utils/test_process_utils.py:146:30:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/utils/test_process_utils.py#L152'>airflow-core/tests/unit/utils/test_process_utils.py:152:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/utils/test_process_utils.py#L157'>airflow-core/tests/unit/utils/test_process_utils.py:157:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/utils/test_process_utils.py#L165'>airflow-core/tests/unit/utils/test_process_utils.py:165:25:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/airflow-core/tests/unit/utils/test_process_utils.py#L173'>airflow-core/tests/unit/utils/test_process_utils.py:173:25:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/breeze/src/airflow_breeze/commands/main_command.py#L138'>dev/breeze/src/airflow_breeze/commands/main_command.py:138:26:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/breeze/src/airflow_breeze/commands/main_command.py#L184'>dev/breeze/src/airflow_breeze/commands/main_command.py:184:27:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L151'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:151:13:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/breeze/src/airflow_breeze/commands/setup_commands.py#L153'>dev/breeze/src/airflow_breeze/commands/setup_commands.py:153:17:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/breeze/src/airflow_breeze/utils/reinstall.py#L43'>dev/breeze/src/airflow_breeze/utils/reinstall.py:43:21:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/dev/breeze/src/airflow_breeze/utils/reinstall.py#L50'>dev/breeze/src/airflow_breeze/utils/reinstall.py:50:23:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/kubernetes-tests/tests/kubernetes_tests/test_base.py#L124'>kubernetes-tests/tests/kubernetes_tests/test_base.py:124:19:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/kubernetes-tests/tests/kubernetes_tests/test_base.py#L239'>kubernetes-tests/tests/kubernetes_tests/test_base.py:239:21:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py#L78'>providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py:78:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py#L79'>providers/google/tests/unit/google/cloud/log/test_gcs_task_handler_system.py:79:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L67'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:67:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L68'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:68:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L83'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:83:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py#L84'>providers/google/tests/unit/google/cloud/log/test_stackdriver_task_handler_system.py:84:20:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/breeze_cmd_line.py#L63'>scripts/ci/pre_commit/breeze_cmd_line.py:63:14:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/breeze_cmd_line.py#L79'>scripts/ci/pre_commit/breeze_cmd_line.py:79:11:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/breeze_cmd_line.py#L88'>scripts/ci/pre_commit/breeze_cmd_line.py:88:7:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/check_kubeconform.py#L30'>scripts/ci/pre_commit/check_kubeconform.py:30:13:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/check_kubeconform.py#L43'>scripts/ci/pre_commit/check_kubeconform.py:43:10:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/check_min_python_version.py#L34'>scripts/ci/pre_commit/check_min_python_version.py:34:9:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_fab_assets.py#L76'>scripts/ci/pre_commit/compile_fab_assets.py:76:18:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_fab_assets.py#L88'>scripts/ci/pre_commit/compile_fab_assets.py:88:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L35'>scripts/ci/pre_commit/compile_lint_ui.py:35:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L36'>scripts/ci/pre_commit/compile_lint_ui.py:36:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L39'>scripts/ci/pre_commit/compile_lint_ui.py:39:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L40'>scripts/ci/pre_commit/compile_lint_ui.py:40:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L41'>scripts/ci/pre_commit/compile_lint_ui.py:41:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L44'>scripts/ci/pre_commit/compile_lint_ui.py:44:5:</a> S603 `subprocess` call: check for execution of untrusted input
- <a href='https://github.com/apache/airflow/blob/de3f7d1576f21f85213ee265387138df4134167e/scripts/ci/pre_commit/compile_lint_ui.py#L46'>scripts/ci/pre_commit/compile_lint_ui.py:46:5:</a> S603 `subprocess` call: check for execution of untrusted input
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

_Label `fixes` added by @ntBre on 2025-03-26 16:18_

---

_Comment by @kaxil on 2025-03-31 10:58_

Gentle reminder @dhruvmanila 

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/mod.rs`:1 on 2025-03-31 20:50_

We usually don't include any logic in `mod.rs` for a plugin. Any common logic to multiple rules in a plugin would go under `airflow/helpers.rs` file.

But, I see that the logic in this file is same as the one from `numpy_2_0_deprecation.rs`. I'd suggest to reuse the same infrastructure to avoid code duplication. What you can do is move the necessary code from `numpy/rules/numpy_2_0_deprecation.rs` to `numpy/helpers.rs`, make the necessary APIs `pub(crate)` and reuse them in `airflow/rules/removal_in_3.rs`.

---

_@dhruvmanila reviewed on 2025-03-31 20:50_

---

_@dhruvmanila reviewed on 2025-03-31 20:52_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_class_attribute.py.snap`:5 on 2025-03-31 20:52_

This seems to be the fix title, I'd suggest using the rule description for this as the fix title is already displayed as the "help: " text below in the `full` output format (as seen in the test snapshot).

---

_Label `preview` added by @dhruvmanila on 2025-03-31 21:04_

---

_@Lee-W reviewed on 2025-04-01 09:35_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/mod.rs`:1 on 2025-04-01 09:35_

I'm trying to extract some of the common code to `numpy/helpers.rs` and `airflow/helper.rs`. But the structure is a bit different. Not sure whether we can reuse the whole `is_guarded_by_try_except` logic here

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/snapshots/ruff_linter__rules__airflow__tests__AIR302_AIR302_class_attribute.py.snap`:5 on 2025-04-01 09:43_

updated. Thanks!

---

_@Lee-W reviewed on 2025-04-01 09:43_

---

_@Lee-W reviewed on 2025-04-01 09:45_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:1 on 2025-04-01 09:45_

This is moved here so that we can reused it after we reorg airflow rules

---

_@Lee-W reviewed on 2025-04-01 09:46_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:918 on 2025-04-01 09:46_

I tried to move the fix a bit but did not succeeded. Should we refactor the fix here as well?

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/mod.rs`:1 on 2025-04-02 14:56_

Oh I see. It's fine to not reuse `is_guarded_by_try_accept` then and have a logic specific to Airflow.

---

_@dhruvmanila reviewed on 2025-04-02 14:56_

---

_@dhruvmanila reviewed on 2025-04-02 14:57_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:918 on 2025-04-02 14:57_

I'm not sure I follow here. Do you mean to avoid the clone here that I suggested in your other PRs?

---

_@dhruvmanila reviewed on 2025-04-02 14:58_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:918 on 2025-04-02 14:58_

If it requires too much effort then it's fine to avoid making the change.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/helpers.rs`:24 on 2025-04-02 15:04_

Is this always going to be a `Name` node? Can it be an `Attribute` node?

For example, in NumPy deprecation rules, there's this test case:

https://github.com/astral-sh/ruff/blob/ae2cf91a360e927f5da307d0290be1a6229ced9a/crates/ruff_linter/resources/test/fixtures/numpy/NPY201_2.py#L71-L74

Is something like that possible? It's totally fine if you don't want to support something like that.

---

_@dhruvmanila reviewed on 2025-04-02 15:04_

Looks good. Are you going to now change other `Replacement::Name` to use `Replacement::AutoImport`?

---

_@Lee-W reviewed on 2025-04-02 15:09_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/rules/removal_in_3.rs`:918 on 2025-04-02 15:09_

> Do you mean to avoid the clone here that I suggested in your other PRs?

Yep

> If it requires too much effort then it's fine to avoid making the change.

We can probably do that in a following PR

---

_Comment by @Lee-W on 2025-04-02 15:10_

> Looks good. Are you going to now change other `Replacement::Name` to use `Replacement::AutoImport`?

I'm thinking of making it another PR. @sunank200 will take over during the time I'm out.

---

_@Lee-W reviewed on 2025-04-02 15:11_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:24 on 2025-04-02 15:11_

We'll want to add it. But I'm thinking of addressing it in another PR. @sunank200 will  take over during the time I'm out.

---

_@dhruvmanila reviewed on 2025-04-02 15:14_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/airflow/helpers.rs`:24 on 2025-04-02 15:14_

Sounds good. Should I go merge this PR then?

Enjoy your time off!

---

_Renamed from "[airflow] add autofix to AIR302 check_name" to "[`airflow`] Add autofix infrastructure to `AIR302` name checks" by @dhruvmanila on 2025-04-02 15:15_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:24 on 2025-04-02 15:19_

Yep, it would be super helpful if we could merge it first (also unblock a few draft PR). Let me resolve the conflict now!

---

_@Lee-W reviewed on 2025-04-02 15:19_

---

_@Lee-W reviewed on 2025-04-02 15:25_

---

_Review comment by @Lee-W on `crates/ruff_linter/src/rules/airflow/helpers.rs`:24 on 2025-04-02 15:25_

Updated 🙌 

---

_Merged by @dhruvmanila on 2025-04-02 15:27_

---

_Closed by @dhruvmanila on 2025-04-02 15:27_

---
