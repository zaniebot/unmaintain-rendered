```yaml
number: 14059
title: "[`flake8-pyi`] - include all python file types for `PYI006` and `PYI066`"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pyi006-and-pyi066-for-py-files
created_at: 2024-11-02T23:25:51Z
updated_at: 2024-11-03T11:47:36Z
url: https://github.com/astral-sh/ruff/pull/14059
synced_at: 2026-01-12T15:55:46Z
```

# [`flake8-pyi`] - include all python file types for `PYI006` and `PYI066`

---

_@diceroll123_

## Summary

Allows `bad-version-info-order` and `bad-version-info-comparison` to work in file types besides stubs

Closes #13836

## Test Plan

`cargo test`

_I really only just moved code around and updated snapshots_

---

_Review requested from @AlexWaygood by @diceroll123 on 2024-11-02 23:25_

---

_Comment by @github-actions[bot] on 2024-11-02 23:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+42 -0 violations, +0 -0 fixes in 6 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6fd7052f863ad9fc95ea4b82f8993fc5858d0dc3/providers/src/airflow/providers/cloudant/hooks/cloudant.py#L25'>providers/src/airflow/providers/cloudant/hooks/cloudant.py:25:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/apache/airflow/blob/6fd7052f863ad9fc95ea4b82f8993fc5858d0dc3/providers/tests/cloudant/hooks/test_cloudant.py#L30'>providers/tests/cloudant/hooks/test_cloudant.py:30:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/apache/airflow/blob/6fd7052f863ad9fc95ea4b82f8993fc5858d0dc3/providers/tests/google/cloud/utils/test_mlengine_prediction_summary.py#L27'>providers/tests/google/cloud/utils/test_mlengine_prediction_summary.py:27:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/apache/airflow/blob/6fd7052f863ad9fc95ea4b82f8993fc5858d0dc3/tests/plugins/test_plugins_manager.py#L65'>tests/plugins/test_plugins_manager.py:65:12:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/d2c7614c3a959e9500f186344ddf89e5e77937ea/bin/bump_version.py#L20'>bin/bump_version.py:20:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pypa/cibuildwheel/blob/d2c7614c3a959e9500f186344ddf89e5e77937ea/cibuildwheel/_compat/typing.py#L5'>cibuildwheel/_compat/typing.py:5:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/544cf20583a37b206eb8ee6ea5881a8d64d4b460/setuptools/_importlib.py#L3'>setuptools/_importlib.py:3:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pypa/setuptools/blob/544cf20583a37b206eb8ee6ea5881a8d64d4b460/setuptools/_importlib.py#L9'>setuptools/_importlib.py:9:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pypa/setuptools/blob/544cf20583a37b206eb8ee6ea5881a8d64d4b460/setuptools/command/bdist_wheel.py#L493'>setuptools/command/bdist_wheel.py:493:20:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pypa/setuptools/blob/544cf20583a37b206eb8ee6ea5881a8d64d4b460/setuptools/tests/test_bdist_wheel.py#L524'>setuptools/tests/test_bdist_wheel.py:524:8:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+19 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/docs/conf.py#L16'>docs/conf.py:16:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/builtins.py#L5'>src/scikit_build_core/_compat/builtins.py:5:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/importlib/metadata.py#L15'>src/scikit_build_core/_compat/importlib/metadata.py:15:8:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/importlib/metadata.py#L17'>src/scikit_build_core/_compat/importlib/metadata.py:17:10:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/importlib/metadata.py#L6'>src/scikit_build_core/_compat/importlib/metadata.py:6:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/importlib/resources.py#L5'>src/scikit_build_core/_compat/importlib/resources.py:5:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/tomllib.py#L5'>src/scikit_build_core/_compat/tomllib.py:5:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/typing.py#L14'>src/scikit_build_core/_compat/typing.py:14:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/typing.py#L19'>src/scikit_build_core/_compat/typing.py:19:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_compat/typing.py#L6'>src/scikit_build_core/_compat/typing.py:6:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/_logging.py#L80'>src/scikit_build_core/_logging.py:80:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/builder/sysconfig.py#L179'>src/scikit_build_core/builder/sysconfig.py:179:8:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/file_api/reply.py#L76'>src/scikit_build_core/file_api/reply.py:76:24:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/src/scikit_build_core/settings/json_schema.py#L67'>src/scikit_build_core/settings/json_schema.py:67:16:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/conftest.py#L16'>tests/conftest.py:16:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/conftest.py#L23'>tests/conftest.py:23:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/packages/navigate_editable/python/shared_pkg/py_module.py#L3'>tests/packages/navigate_editable/python/shared_pkg/py_module.py:3:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/test_builder.py#L247'>tests/test_builder.py:247:8:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/f6e60f41e46ce13ddbd344542d1bf45743b74514/tests/test_pyproject_pep660.py#L100'>tests/test_pyproject_pep660.py:100:8:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_core/_tests/test_run.py#L2783'>src/trio/_core/_tests/test_run.py:2783:4:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_tests/type_tests/path.py#L110'>src/trio/_tests/type_tests/path.py:110:8:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_tests/type_tests/path.py#L51'>src/trio/_tests/type_tests/path.py:51:8:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_tests/type_tests/path.py#L57'>src/trio/_tests/type_tests/path.py:57:8:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_tests/type_tests/path.py#L60'>src/trio/_tests/type_tests/path.py:60:8:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_tests/type_tests/path.py#L78'>src/trio/_tests/type_tests/path.py:78:8:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/python-trio/trio/blob/9d6e134d3256c811ea311322407058e102a8918e/src/trio/_tests/type_tests/path.py#L98'>src/trio/_tests/type_tests/path.py:98:8:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/a1a491837b35b719d77b4f03be318b505d495386/src/_pytest/_code/code.py#L699'>src/_pytest/_code/code.py:699:16:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pytest-dev/pytest/blob/a1a491837b35b719d77b4f03be318b505d495386/src/_pytest/assertion/rewrite.py#L299'>src/_pytest/assertion/rewrite.py:299:16:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pytest-dev/pytest/blob/a1a491837b35b719d77b4f03be318b505d495386/src/_pytest/pathlib.py#L41'>src/_pytest/pathlib.py:41:4:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pytest-dev/pytest/blob/a1a491837b35b719d77b4f03be318b505d495386/testing/_py/test_local.py#L21'>testing/_py/test_local.py:21:12:</a> PYI006 Use `<` or `>=` for `sys.version_info` comparisons
+ <a href='https://github.com/pytest-dev/pytest/blob/a1a491837b35b719d77b4f03be318b505d495386/testing/code/test_excinfo.py#L1771'>testing/code/test_excinfo.py:1771:8:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
+ <a href='https://github.com/pytest-dev/pytest/blob/a1a491837b35b719d77b4f03be318b505d495386/testing/test_debugging.py#L106'>testing/test_debugging.py:106:12:</a> PYI066 Use `>=` when using `if`-`else` with `sys.version_info` comparisons
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI066 | 34 | 34 | 0 | 0 | 0 |
| PYI006 | 8 | 8 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-11-03 02:48_

Thanks! We probably need to gate this under preview, since it's an expansion of the rule.

---

_Label `rule` added by @charliermarsh on 2024-11-03 02:48_

---

_Label `preview` added by @charliermarsh on 2024-11-03 02:48_

---

_@MichaReiser approved on 2024-11-03 11:47_

---

_Merged by @MichaReiser on 2024-11-03 11:47_

---

_Closed by @MichaReiser on 2024-11-03 11:47_

---
