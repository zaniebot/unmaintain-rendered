```yaml
number: 19245
title: "[`flake8-use-pathlib`] Add autofix for `PTH109`"
type: pull_request
state: merged
author: chirizxc
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/more-pth-autofixes
created_at: 2025-07-09T19:41:30Z
updated_at: 2025-07-17T14:13:01Z
url: https://github.com/astral-sh/ruff/pull/19245
synced_at: 2026-01-12T15:56:34Z
```

# [`flake8-use-pathlib`] Add autofix for `PTH109`

---

_@chirizxc_

## Summary

Part of #2331

## Test Plan

`cargo nextest run flake8_use_pathlib`


---

_@chirizxc reviewed on 2025-07-09 19:47_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/preview.rs`:137 on 2025-07-09 19:47_

i was able to do the math this time 😁

---

_Comment by @github-actions[bot] on 2025-07-09 19:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +50 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +24 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/airflow-core/src/airflow/cli/cli_config.py#L178'>airflow-core/src/airflow/cli/cli_config.py:178:40:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/airflow-core/src/airflow/cli/cli_config.py#L178'>airflow-core/src/airflow/cli/cli_config.py:178:40:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/airflow-core/tests/unit/utils/test_cli_util.py#L188'>airflow-core/tests/unit/utils/test_cli_util.py:188:38:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/airflow-core/tests/unit/utils/test_cli_util.py#L188'>airflow-core/tests/unit/utils/test_cli_util.py:188:38:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/airflow-core/tests/unit/utils/test_cli_util.py#L193'>airflow-core/tests/unit/utils/test_cli_util.py:193:37:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/airflow-core/tests/unit/utils/test_cli_util.py#L193'>airflow-core/tests/unit/utils/test_cli_util.py:193:37:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/minor_release_command.py#L180'>dev/breeze/src/airflow_breeze/commands/minor_release_command.py:180:17:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/minor_release_command.py#L180'>dev/breeze/src/airflow_breeze/commands/minor_release_command.py:180:17:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L372'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:372:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/release_candidate_command.py#L372'>dev/breeze/src/airflow_breeze/commands/release_candidate_command.py:372:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/release_command.py#L197'>dev/breeze/src/airflow_breeze/commands/release_command.py:197:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/release_command.py#L197'>dev/breeze/src/airflow_breeze/commands/release_command.py:197:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/release_command.py#L213'>dev/breeze/src/airflow_breeze/commands/release_command.py:213:19:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/commands/release_command.py#L213'>dev/breeze/src/airflow_breeze/commands/release_command.py:213:19:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/utils/docs_publisher.py#L85'>dev/breeze/src/airflow_breeze/utils/docs_publisher.py:85:61:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/utils/docs_publisher.py#L85'>dev/breeze/src/airflow_breeze/utils/docs_publisher.py:85:61:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/utils/reproducible.py#L60'>dev/breeze/src/airflow_breeze/utils/reproducible.py:60:21:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/utils/reproducible.py#L60'>dev/breeze/src/airflow_breeze/utils/reproducible.py:60:21:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/utils/run_utils.py#L132'>dev/breeze/src/airflow_breeze/utils/run_utils.py:132:41:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/dev/breeze/src/airflow_breeze/utils/run_utils.py#L132'>dev/breeze/src/airflow_breeze/utils/run_utils.py:132:41:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py#L883'>providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py:883:48:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py#L883'>providers/elasticsearch/tests/unit/elasticsearch/log/test_es_task_handler.py:883:48:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/scripts/ci/pre_commit/common_precommit_utils.py#L83'>scripts/ci/pre_commit/common_precommit_utils.py:83:29:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/airflow/blob/47bbe55355757953507b8ae64cd05f9380e16748/scripts/ci/pre_commit/common_precommit_utils.py#L83'>scripts/ci/pre_commit/common_precommit_utils.py:83:29:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/7229e1ccf391dc05faf020fe42a785093ea4c95f/superset/extensions/pylint.py#L44'>superset/extensions/pylint.py:44:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/apache/superset/blob/7229e1ccf391dc05faf020fe42a785093ea4c95f/superset/extensions/pylint.py#L44'>superset/extensions/pylint.py:44:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -0 violations, +22 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L57'>release/system.py:57:50:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L57'>release/system.py:57:50:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L61'>release/system.py:61:34:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/release/system.py#L61'>release/system.py:61:34:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:16:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/application/handlers/code_runner.py#L219'>src/bokeh/application/handlers/code_runner.py:219:16:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L527'>src/bokeh/io/export.py:527:76:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/export.py#L527'>src/bokeh/io/export.py:527:76:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/util.py#L78'>src/bokeh/io/util.py:78:36:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/io/util.py#L78'>src/bokeh/io/util.py:78:36:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/util.py#L48'>src/bokeh/sphinxext/util.py:48:22:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/sphinxext/util.py#L48'>src/bokeh/sphinxext/util.py:48:22:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L246'>src/bokeh/util/compiler.py:246:20:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/util/compiler.py#L246'>src/bokeh/util/compiler.py:246:20:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L160'>tests/support/util/filesystem.py:160:11:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/support/util/filesystem.py#L160'>tests/support/util/filesystem.py:160:11:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L154'>tests/unit/bokeh/application/handlers/test_code_runner.py:154:19:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L154'>tests/unit/bokeh/application/handlers/test_code_runner.py:154:19:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L159'>tests/unit/bokeh/application/handlers/test_code_runner.py:159:16:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/application/handlers/test_code_runner.py#L159'>tests/unit/bokeh/application/handlers/test_code_runner.py:159:16:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_browser.py#L98'>tests/unit/bokeh/util/test_browser.py:98:63:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_browser.py#L98'>tests/unit/bokeh/util/test_browser.py:98:63:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/latchbio/latch">latchbio/latch</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/snakemake/serialize.py#L152'>src/latch_cli/snakemake/serialize.py:152:25:</a> PTH109 [*] `os.getcwd()` should be replaced by `Path.cwd()`
- <a href='https://github.com/latchbio/latch/blob/848eccdfc5401cc674edf70c86c5512adc4b0699/src/latch_cli/snakemake/serialize.py#L152'>src/latch_cli/snakemake/serialize.py:152:25:</a> PTH109 `os.getcwd()` should be replaced by `Path.cwd()`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PTH109 | 50 | 0 | 0 | 50 | 0 |

</p>
</details>




---

_Comment by @chirizxc on 2025-07-10 18:17_

@ntBre I'm not really sure about the behavior of 
```python
cwd = os.getcwd(
    # 1
)
```
when we have comments inside brackets, because I've seen this somewhere, even though the function takes no arguments it's still worth marking the autofix as unsafe

---

_Label `fixes` added by @ntBre on 2025-07-11 13:10_

---

_Label `preview` added by @ntBre on 2025-07-11 13:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/snapshots/ruff_linter__rules__flake8_use_pathlib__tests__preview_full_name.py.snap`:185 on 2025-07-11 13:22_

This test case was probably not intended to show this, but we actually shouldn't offer a fix if there's an unexpected argument. This might apply to some of the other fixes you've added too, but we should add a check like 

```rust
if !call.arguments.is_empty() {
    return; // without a fix, a diagnostic is still okay
}
```

We'll also need  to add test cases without arguments to these `cwd` functions to actually  test the fix after that.

---

_@ntBre reviewed on 2025-07-11 13:24_

Thanks, this looks good. We just need to validate the number of arguments before offering a fix.

> when we have comments inside brackets, because I've seen this somewhere, even though the function takes no arguments it's still worth marking the autofix as unsafe

I think you made the right call making the fix unsafe, even if this is probably uncommon 👍 

---

_Comment by @chirizxc on 2025-07-11 14:57_

What about `os.getcwdb()`? Is it worth adding an example in the documentation?

---

_@ntBre approved on 2025-07-17 14:11_

Thanks!

> What about os.getcwdb()? Is it worth adding an example in the documentation?

I think it's okay to leave out the example since it's so similar to the existing one. The `## What it does` section mentions `getcwdb` at least.

---

_Merged by @ntBre on 2025-07-17 14:11_

---

_Closed by @ntBre on 2025-07-17 14:11_

---

_Branch deleted on 2025-07-17 14:13_

---
