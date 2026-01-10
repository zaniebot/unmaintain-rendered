```yaml
number: 18907
title: "[`pep8_naming`] Avoid false positives on standard library functions with uppercase names (`N802`)"
type: pull_request
state: merged
author: w0nder1ng
labels:
  - rule
assignees: []
merged: true
base: main
head: n802_ast_visit
created_at: 2025-06-24T02:02:54Z
updated_at: 2025-07-14T08:34:07Z
url: https://github.com/astral-sh/ruff/pull/18907
synced_at: 2026-01-10T18:33:12Z
```

# [`pep8_naming`] Avoid false positives on standard library functions with uppercase names (`N802`)

---

_Pull request opened by @w0nder1ng on 2025-06-24 02:02_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This change prevents the `visit_*` methods of ast.NodeVisitor/NodeTransformer and the `do_*` methods of http.server.BaseHTTPRequestHandler from triggering N802. See #9400 for more information.

In `Lib/ast.py`:
```python
class NodeVisitor(object):
    def visit(self, node):
        """Visit a node."""
        method = 'visit_' + node.__class__.__name__
        visitor = getattr(self, method, self.generic_visit)
        return visitor(node)
```
Example usage
```python
from ast import NodeVisitor

class Visitor(NodeVisitor):
    def visit_Constant(self, node):
        print(f"visited a constant with value {node.value}")
```

Previously, this would trigger a N802 violation, even though the uppercase name is the intended way of using those subclasses. 

## Test Plan

I added tests to `pep8_naming/N802.py`.


---

_Comment by @github-actions[bot] on 2025-06-24 02:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -8 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/airflow-core/src/airflow/utils/scheduler_health.py#L36'>airflow-core/src/airflow/utils/scheduler_health.py:36:9:</a> N802 Function name `do_GET` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3387'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3387:17:</a> N802 Function name `visit_FunctionDef` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/ci/pre_commit/check_deferrable_default.py#L45'>scripts/ci/pre_commit/check_deferrable_default.py:45:9:</a> N802 Function name `visit_FunctionDef` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/in_container/run_template_fields_check.py#L52'>scripts/in_container/run_template_fields_check.py:52:9:</a> N802 Function name `visit_FunctionDef` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/in_container/run_template_fields_check.py#L57'>scripts/in_container/run_template_fields_check.py:57:9:</a> N802 Function name `visit_Assign` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/in_container/run_template_fields_check.py#L66'>scripts/in_container/run_template_fields_check.py:66:9:</a> N802 Function name `visit_AnnAssign` should be lowercase
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L59'>tests/support/plugins/file_server.py:59:9:</a> N802 Function name `do_GET` should be lowercase
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a5f1774b769ad981d329e7469e593821d8db53b2/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L23'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:23:48:</a> RUF100 [*] Unused `noqa` directive (unused: `N802`)
+ <a href='https://github.com/langchain-ai/langchain/blob/a5f1774b769ad981d329e7469e593821d8db53b2/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L42'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:42:50:</a> RUF100 [*] Unused `noqa` directive (unused: `N802`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/auth/oauth2.py#L89'>src/latch_cli/auth/oauth2.py:89:17:</a> N802 Function name `do_GET` should be lowercase
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/centromere/ast_parsing.py#L32'>src/latch_cli/centromere/ast_parsing.py:32:58:</a> RUF100 [*] Unused `noqa` directive (unused: `N802`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N802 | 8 | 0 | 8 | 0 | 0 |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -8 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/airflow-core/src/airflow/utils/scheduler_health.py#L36'>airflow-core/src/airflow/utils/scheduler_health.py:36:9:</a> N802 Function name `do_GET` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/dev/breeze/src/airflow_breeze/commands/release_management_commands.py#L3387'>dev/breeze/src/airflow_breeze/commands/release_management_commands.py:3387:17:</a> N802 Function name `visit_FunctionDef` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/ci/pre_commit/check_deferrable_default.py#L45'>scripts/ci/pre_commit/check_deferrable_default.py:45:9:</a> N802 Function name `visit_FunctionDef` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/in_container/run_template_fields_check.py#L52'>scripts/in_container/run_template_fields_check.py:52:9:</a> N802 Function name `visit_FunctionDef` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/in_container/run_template_fields_check.py#L57'>scripts/in_container/run_template_fields_check.py:57:9:</a> N802 Function name `visit_Assign` should be lowercase
- <a href='https://github.com/apache/airflow/blob/4cc8ebf312fd85332a0c45607fd21ef96c13373c/scripts/in_container/run_template_fields_check.py#L66'>scripts/in_container/run_template_fields_check.py:66:9:</a> N802 Function name `visit_AnnAssign` should be lowercase
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/plugins/file_server.py#L59'>tests/support/plugins/file_server.py:59:9:</a> N802 Function name `do_GET` should be lowercase
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a5f1774b769ad981d329e7469e593821d8db53b2/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L23'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:23:48:</a> RUF100 [*] Unused `noqa` directive (unused: `N802`)
+ <a href='https://github.com/langchain-ai/langchain/blob/a5f1774b769ad981d329e7469e593821d8db53b2/libs/cli/langchain_cli/namespaces/migrate/generate/utils.py#L42'>libs/cli/langchain_cli/namespaces/migrate/generate/utils.py:42:50:</a> RUF100 [*] Unused `noqa` directive (unused: `N802`)
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/auth/oauth2.py#L89'>src/latch_cli/auth/oauth2.py:89:17:</a> N802 Function name `do_GET` should be lowercase
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/centromere/ast_parsing.py#L32'>src/latch_cli/centromere/ast_parsing.py:32:58:</a> RUF100 [*] Unused `noqa` directive (unused: `N802`)
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| N802 | 8 | 0 | 8 | 0 | 0 |
| RUF100 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-06-24 08:19_

---

_Comment by @MichaReiser on 2025-06-24 08:23_

I don't think the special handling for `NodeVisitor` is necessary because typeshed defines all methods on `NodeVisitor` and, therefore, using `@typing.override` works ([mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=3f2ec98dc16de67ecbeeca7a575891ad))

Special casing the http handler seems fine, while unfortunate 


---

_Comment by @w0nder1ng on 2025-06-25 00:01_

`@typing.override` was added in 3.12, so it's not available in 3.11 or earlier, which is where I ran into this issue. Could the NodeVisitor check be gated behind python version < 3.12?

---

_Comment by @w0nder1ng on 2025-07-07 04:22_

@MichaReiser is this an acceptable solution?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_function_name.rs`:101 on 2025-07-09 16:21_

nit: I think this could just be < 3.12:


```suggestion
    let is_ast_visitor =  checker.target_version() < PythonVersion::PY312 
    && name.starts_with("visit_")
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_function_name.rs`:147 on 2025-07-09 16:25_

It looks like we have a very similar existing function that will also recursively visit parent classes (I think):

https://github.com/astral-sh/ruff/blob/35a33f045eecb6652b5c73f82dce26e6c610c3b8/crates/ruff_python_semantic/src/analyze/class.rs#L123-L131

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pep8_naming/mod.rs`:204 on 2025-07-09 16:33_

I think we might be able to omit these custom settings if we're only testing `N802`.

---

_@ntBre reviewed on 2025-07-09 16:34_

This looks reasonable to me if it's okay with @MichaReiser. I just had a few small suggestions.

Before 3.12 I think users could still get `@override` from `typing_extensions`, but I'm not sure if that would be a better approach.

---

_@MichaReiser reviewed on 2025-07-11 16:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_function_name.rs`:93 on 2025-07-11 16:32_

I think we can remove this check. I'm not a fan of the change itself but it's also sort of fine. 

If we keep this check, then it should apply for all python versions unless typing_extensions is disallowed by https://docs.astral.sh/ruff/settings/#lint_typing-extensions

---

_Merged by @MichaReiser on 2025-07-14 08:26_

---

_Closed by @MichaReiser on 2025-07-14 08:26_

---
