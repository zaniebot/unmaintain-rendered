```yaml
number: 17368
title: "[`ruff`] Check for non-context-manager use of `pytest.raises`, `pytest.warns`, and `pytest.deprecated_call` (`RUF061`)"
type: pull_request
state: merged
author: twentyone212121
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-issue-16605
created_at: 2025-04-12T14:07:33Z
updated_at: 2025-06-16T17:03:55Z
url: https://github.com/astral-sh/ruff/pull/17368
synced_at: 2026-01-12T15:56:01Z
```

# [`ruff`] Check for non-context-manager use of `pytest.raises`, `pytest.warns`, and `pytest.deprecated_call` (`RUF061`)

---

_@twentyone212121_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

This PR aims to close #16605.

## Summary

This PR introduces a new rule (`RUF061`) that detects non-contextmanager usage of `pytest.raises`, `pytest.warns`, and `pytest.deprecated_call`. This pattern is discouraged and [was proposed in flake8-pytest-style](https://github.com/m-burst/flake8-pytest-style/pull/332), but the corresponding PR has been open for over a month without activity.

Additionally, this PR provides an unsafe fix for simple cases where the non-contextmanager form can be transformed into the context manager form. Examples of supported patterns are listed in `RUF061_raises.py`, `RUF061_warns.py`, and `RUF061_deprecated_call.py` test files.

The more complex case from the original issue (involving two separate statements):
```python
excinfo = pytest.raises(ValueError, int, "hello")
assert excinfo.match("^invalid literal")
```
is getting fixed like this:
```python
with pytest.raises(ValueError) as excinfo:
    int("hello")
assert excinfo.match("^invalid literal")
```
Putting match in the raises call requires multi-statement transformation, which I am not sure how to implement.

## Test Plan

<!-- How was it tested? -->

New test files were added to cover various usages of the non-contextmanager form of pytest.raises, warns, and deprecated_call.


---

_Converted to draft by @twentyone212121 on 2025-04-12 14:08_

---

_Renamed from "[`flake8-pytest-style`] Deprecated `pytest.raises` callable form rule…" to "[`flake8-pytest-style`] Deprecated `pytest.raises` callable form rule + fix (`PT032`)" by @twentyone212121 on 2025-04-12 14:21_

---

_Comment by @github-actions[bot] on 2025-04-12 14:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+231 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/9df990c2d1847b174dff550617dc28761a1a1b79/tests/integration_tests/datasource_tests.py#L349'>tests/integration_tests/datasource_tests.py:349:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/apache/superset/blob/9df990c2d1847b174dff550617dc28761a1a1b79/tests/integration_tests/datasource_tests.py#L497'>tests/integration_tests/datasource_tests.py:497:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/apache/superset/blob/9df990c2d1847b174dff550617dc28761a1a1b79/tests/integration_tests/datasource_tests.py#L509'>tests/integration_tests/datasource_tests.py:509:9:</a> RUF061 Use context-manager form of `pytest.raises()`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+21 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L114'>indico/core/settings/proxy_test.py:114:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L115'>indico/core/settings/proxy_test.py:115:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L116'>indico/core/settings/proxy_test.py:116:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L117'>indico/core/settings/proxy_test.py:117:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L118'>indico/core/settings/proxy_test.py:118:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L119'>indico/core/settings/proxy_test.py:119:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L120'>indico/core/settings/proxy_test.py:120:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L170'>indico/core/settings/proxy_test.py:170:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L171'>indico/core/settings/proxy_test.py:171:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/indico/indico/blob/e2f23c95902289f35874150b686f0fd36c3c2cec/indico/core/settings/proxy_test.py#L172'>indico/core/settings/proxy_test.py:172:5:</a> RUF061 Use context-manager form of `pytest.raises()`
... 11 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+134 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/doc/en/example/assertion/failure_demo.py#L168'>doc/en/example/assertion/failure_demo.py:168:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/doc/en/example/assertion/failure_demo.py#L171'>doc/en/example/assertion/failure_demo.py:171:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/_py/test_local.py#L1001'>testing/_py/test_local.py:1001:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/_py/test_local.py#L1002'>testing/_py/test_local.py:1002:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/_py/test_local.py#L1102'>testing/_py/test_local.py:1102:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/_py/test_local.py#L1400'>testing/_py/test_local.py:1400:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/_py/test_local.py#L628'>testing/_py/test_local.py:628:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_code.py#L91'>testing/code/test_code.py:91:15:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1019'>testing/code/test_excinfo.py:1019:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1068'>testing/code/test_excinfo.py:1068:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1082'>testing/code/test_excinfo.py:1082:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1101'>testing/code/test_excinfo.py:1101:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1118'>testing/code/test_excinfo.py:1118:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1149'>testing/code/test_excinfo.py:1149:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1182'>testing/code/test_excinfo.py:1182:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1214'>testing/code/test_excinfo.py:1214:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1244'>testing/code/test_excinfo.py:1244:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1271'>testing/code/test_excinfo.py:1271:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1327'>testing/code/test_excinfo.py:1327:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1380'>testing/code/test_excinfo.py:1380:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1473'>testing/code/test_excinfo.py:1473:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L1568'>testing/code/test_excinfo.py:1568:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L225'>testing/code/test_excinfo.py:225:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L243'>testing/code/test_excinfo.py:243:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L254'>testing/code/test_excinfo.py:254:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L276'>testing/code/test_excinfo.py:276:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L297'>testing/code/test_excinfo.py:297:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L315'>testing/code/test_excinfo.py:315:19:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/pytest-dev/pytest/blob/336cb917f7175ccc5a699f81b58f3587c5a7dcca/testing/code/test_excinfo.py#L331'>testing/code/test_excinfo.py:331:19:</a> RUF061 Use context-manager form of `pytest.raises()`
... 105 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+73 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/constants/tests/test_constant.py#L51'>astropy/constants/tests/test_constant.py:51:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/constants/tests/test_constant.py#L55'>astropy/constants/tests/test_constant.py:55:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/constants/tests/test_constant.py#L58'>astropy/constants/tests/test_constant.py:58:5:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/hdu/compressed/tests/test_compressed.py#L137'>astropy/io/fits/hdu/compressed/tests/test_compressed.py:137:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L1015'>astropy/io/fits/tests/test_core.py:1015:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L1016'>astropy/io/fits/tests/test_core.py:1016:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L1019'>astropy/io/fits/tests/test_core.py:1019:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L1020'>astropy/io/fits/tests/test_core.py:1020:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L1158'>astropy/io/fits/tests/test_core.py:1158:17:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L336'>astropy/io/fits/tests/test_core.py:336:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L337'>astropy/io/fits/tests/test_core.py:337:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L338'>astropy/io/fits/tests/test_core.py:338:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L339'>astropy/io/fits/tests/test_core.py:339:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L349'>astropy/io/fits/tests/test_core.py:349:9:</a> RUF061 Use context-manager form of `pytest.raises()`
+ <a href='https://github.com/astropy/astropy/blob/3a070e29f1ea0cf6a7e186477a11190d35f42913/astropy/io/fits/tests/test_core.py#L352'>astropy/io/fits/tests/test_core.py:352:9:</a> RUF061 Use context-manager form of `pytest.raises()`
... 58 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF061 | 231 | 231 | 0 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @twentyone212121 on 2025-04-12 14:36_

---

_Label `rule` added by @ntBre on 2025-04-18 18:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:200 on 2025-04-18 18:58_

nit: maybe something like this:
```suggestion
        Some("Use `pytest.raises()` as a context-manager".to_string())
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-18 19:03_

We might want to leave the `Deprecated` part out of this name since it's not actually deprecated yet, and my quick skim of the open PR making it deprecated made it sound a bit contentious to deprecate it at all. I think this can still be a useful stylistic rule even if it's not formally deprecated.

As more of a nit, I find the rest of the name a bit awkward, but I can't think of anything better that still fits within our naming conventions.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:191 on 2025-04-18 19:03_

nit: I'd just import this so it fits on one line.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-18 19:06_

To be fair, it is called the `legacy` form in the [docs](https://docs.pytest.org/en/stable/how-to/assert.html#alternate-form-legacy), but they also include this statement:

> Nonetheless, this form is fully supported and not deprecated in any way.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:355 on 2025-04-18 19:14_

Does anything here actually return a `Result` naturally? I might be missing something, but everything looks like it returns an `Option`, so we could just return an `Option` from the function and still use `?` everywhere. I don't think the additional messages from `bail` and `context` justify using a `Result` here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:401 on 2025-04-18 19:15_

Same comment as above, I'd return an `Option` here if possible.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:404 on 2025-04-18 19:15_

If you want the value, I think you can use `find_argument_value` instead of having to `match` manually.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:413 on 2025-04-18 19:16_

Same as above, `find_argument_value`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:453 on 2025-04-18 19:21_

I'm not sure there's anything inherently wrong with this, but it feels weird to be comparing the actual `Expr` values for equality here. Can we just use `arguments_source_order` and skip the first two elements of the iterator?

https://github.com/astral-sh/ruff/blob/1108f662e47e8ff5456f00e5a0742f1cd9e95852/crates/ruff_python_ast/src/nodes.rs#L2904-L2908

We should have already bailed out if we didn't find the `func` or `expected_exception` arguments, I think.

You'll still have to partition these into `args` and `keywords` to put into `func_call` below, but I think it will read a bit more nicely. You can also use `.cloned()` on the iterator to avoid needing to map separate `clone` calls.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:244 on 2025-04-18 19:26_

This may not be helpful because I'm not that familiar with this part of the API, but we do have an `adjust_indentation` function in the `edits` module:

https://github.com/astral-sh/ruff/blob/1108f662e47e8ff5456f00e5a0742f1cd9e95852/crates/ruff_linter/src/fix/edits.rs#L348-L354

It might be worth giving it a try to avoid having to manually handle it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:842 on 2025-04-18 19:28_

As you said in the PR description, this may end up needing to be a ruff rule instead, but we can delay changing that until right before landing.

---

_@ntBre reviewed on 2025-04-18 19:40_

Thanks for working on this!

I had some code style suggestions, but the overall approach looks good to me.

With regard to the multi-statement case, I think your example in the PR summary _is_ actually ideal, based on my reading of the pytest docs. See the first `Note` box in [this section](https://docs.pytest.org/en/stable/reference/reference.html#pytest-raises): 

> When using pytest.raises as a context manager, it’s worthwhile to note that normal context manager rules apply and that the exception raised must be the final line in the scope of the context manager. Lines of code after that, within the scope of the context manager will not be executed. For example:

However, to be safe, we may want to avoid offering a fix if the value returned by `raises` is assigned to a variable, or at least if we can see that variable being used later. 
On the other hand, it's already an unsafe fix, so if my reading of the pytest docs is correct, the behavior here might be right in most cases.

Either way, we should add some test cases demonstrating whatever the behavior ends up being.

---

_@webknjaz reviewed on 2025-04-18 20:19_

---

_Review comment by @webknjaz on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-18 20:19_

Yeah, we've been debating the deprecation and mostly settled on “let's try soft-deprecating and see if people complain, but meanwhile let the formatters/linters catch this”: https://github.com/pytest-dev/pytest/pull/13241#issuecomment-2694138535. The PR currently uses a `PendingDeprecationWarning` FWIW.

That “legacy” form is the stdlib unittest baggage, I think. It's probably mostly met in ancient code bases. I'd call it “discouraged” instead of “deprecated”. Or, perhaps, “unintuitive” or “callback-style”.

---

_@twentyone212121 reviewed on 2025-04-19 13:54_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:355 on 2025-04-19 13:54_

I agree, I replaced `Result` with `Option`. The additional messages were helpful for debugging earlier, but using `Option` makes things clearer now.

---

_@twentyone212121 reviewed on 2025-04-19 13:54_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:404 on 2025-04-19 13:54_

Thanks! That’s definitely cleaner

---

_@twentyone212121 reviewed on 2025-04-19 13:55_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:453 on 2025-04-19 13:55_

Yeah, I agree it felt awkward. I initially thought we couldn’t just skip the first two arguments since they aren't positional-only, but I also couldn’t come up with a `pytest.raises` call where `expected_exception` and `func` aren’t the first two.

I refactored it in the latest commit to use `arguments_source_order` and `partition_map`.

---

_@twentyone212121 reviewed on 2025-04-19 13:57_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:244 on 2025-04-19 13:57_

From what I could tell, `adjust_indentation` seems to be mostly used for dedenting code that already exists in the source.

When I was looking into similar fixes that turn a single statement into an indented block, I found the [lambda-assignment rule](https://docs.astral.sh/ruff/rules/lambda-assignment/), and borrowed the indentation logic from there:
https://github.com/astral-sh/ruff/blob/1108f662e47e8ff5456f00e5a0742f1cd9e95852/crates/ruff_linter/src/rules/pycodestyle/rules/lambda_assignment.rs#L80-L97

---

_@twentyone212121 reviewed on 2025-04-19 14:04_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-19 14:04_

Should I rename it to `LegacyPytestRaises` or maybe `CallablePytestRaises`?

---

_Comment by @twentyone212121 on 2025-04-19 14:24_

Thanks for the review!

About the multi-statement case — I initially thought it wasn’t ideal, since in the context manager form, `pytest.raises` accepts the match argument directly, so there's no need for a separate assertion. That said, I’m not sure whether this rule should be responsible for transforming that pattern.

Regarding the tests, I’ve added a multi-statement case, as well as a case where `func` is a lambda.

---

_@webknjaz reviewed on 2025-04-19 17:21_

---

_Review comment by @webknjaz on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-19 17:21_

Callable seems off. Callback would be more accurate.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:453 on 2025-04-23 15:27_

The docs don't seem quite as clear for the legacy form as for the context manager version, but it at least looks like you need them both, and any following args are passed to `func`, so it should be safe (I hope), as long as we already checked explicitly for the first two arguments.

I'll give this another review today!

---

_@ntBre reviewed on 2025-04-23 15:27_

---

_@MichaReiser reviewed on 2025-04-24 06:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-24 06:21_

How about `LegacyFormPytestRaises` or `PytestRaisesAsFunction`? 

It would match the terminolgoy used in the documentation. Other options are `pytest-raises-with-func` because that's really what we detect. What I find interesting is that this form is listed in context manager form in the pytest documentation (but what I understand is that it isn't supposed to be used in a context manager position)

with raises(expected_exception: [type](https://docs.python.org/3/library/functions.html#type)[E] | [tuple](https://docs.python.org/3/library/stdtypes.html#tuple)[[type](https://docs.python.org/3/library/functions.html#type)[E], ...], func: [Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[...], [Any](https://docs.python.org/3/library/typing.html#typing.Any)], *args: [Any](https://docs.python.org/3/library/typing.html#typing.Any), **kwargs: [Any](https://docs.python.org/3/library/typing.html#typing.Any)) → [ExceptionInfo](https://docs.pytest.org/en/stable/reference/reference.html#pytest.ExceptionInfo)[E] as excinfo

---

_@MichaReiser reviewed on 2025-04-24 06:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:247 on 2025-04-24 06:24_

Is it guaranteed that the generator always fits the with-context on a single line?

---

_@ntBre reviewed on 2025-04-24 18:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-24 18:31_

I like `LegacyFormPytestRaises` or maybe `PytestRaisesLegacyForm`, but these all sound good to me.

Ahh, that does explain why I didn't see a signature under the `Legacy` heading. I think you're right that that is the legacy form signature but with `with` in front of it.

What do you think about the rule category? `RUF` might make more sense just to be safe. The upstream PR still hasn't had any updates, but new rules were added in January, so it's still active.

---

_@ntBre reviewed on 2025-04-24 18:42_

Thanks, this looks great. I think now we just need to:
- decide on a name
- decide on a rule category
- address Micha's comment about the single line

I made some suggestions about the first two [here](https://github.com/astral-sh/ruff/pull/17368#discussion_r2059015744), but I'm definitely interested in other thoughts.

---

_@MichaReiser reviewed on 2025-04-24 20:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-24 20:27_

We should add the rule to the Ruff group. 

---

_@twentyone212121 reviewed on 2025-04-29 07:46_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:247 on 2025-04-29 07:46_

From my testing, the generator always produces a single-line `with` statement, even if the original `pytest.raises` call was split across multiple lines. I tried two cases:

1. A very long `expected_exception`:
   ```python
   def test():
       pytest.raises(
           VeryLongExceptionThatExceeds80CharactersToTestWhetherGeneratedWithStatementIsOneLineOrNot,
           test_func,
       )
   ```
   After applying the rule, it’s fixed to:
   ```python
   def test():
       with pytest.raises(VeryLongExceptionThatExceeds80CharactersToTestWhetherGeneratedWithStatementIsOneLineOrNot):
           test_func()
   ```
   Although the resulting line is not formatted according to ruff, it is still generated as a single line.

2. A multiline `match` string:
   ```python
   def test():
       pytest.raises(ValueError, test_func).match(
           """a
           b"""
       )
   ```
   After applying the rule:
   ```python
   def test():
       with pytest.raises(ValueError, match="""a\n           b"""):
           test_func()
   ```
   The `with` statement is produced on a single line.

I don't know if it's okay to generate potentially unformatted code, but it seems like the generator doesn't format its output.

---

_@twentyone212121 reviewed on 2025-04-29 07:50_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/raises.rs`:188 on 2025-04-29 07:50_

I also like `LegacyFormPytestRaises`, so if everyone agrees, I'll rename the rule

---

_@twentyone212121 reviewed on 2025-04-29 07:54_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/codes.rs`:842 on 2025-04-29 07:54_

Should I assign `RUF103` code to the rule? Also, if the rule is renamed, is it okay for the checking code to be in `flake8_pytest_style` folder?

---

_Comment by @twentyone212121 on 2025-04-29 08:02_

Should we also handle legacy forms of `pytest.warns` and `pytest.deprecated_call`? They are mentioned in the [PR to pytest](https://github.com/pytest-dev/pytest/pull/13241). These functions are slightly different from `pytest.raises`, so I’ll need to think about how best to integrate them into the current code to avoid code duplication. 

---

_@ntBre reviewed on 2025-04-29 22:00_

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:842 on 2025-04-29 22:00_

It looks like `RUF60` would be the lowest unused code, but we need to double-check that there aren't any other open PRs using it.

I think we'd also want to move the implementation, tests, and snapshots to the `rules/ruff` directory. It's always a bit tedious to move these around, sorry!

---

_Comment by @ntBre on 2025-04-29 22:05_

> Should we also handle legacy forms of `pytest.warns` and `pytest.deprecated_call`? They are mentioned in the [PR to pytest](https://github.com/pytest-dev/pytest/pull/13241). These functions are slightly different from `pytest.raises`, so I’ll need to think about how best to integrate them into the current code to avoid code duplication.

I think it's okay to handle these separately in follow-up PRs, or even as different rules, if that sounds good to @MichaReiser.

If we want to include them in this rule, it could possibly affect our name choice too.

---

_@ntBre reviewed on 2025-04-29 22:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:842 on 2025-04-29 22:07_

It looks like there are two open PRs for rules using RUF060 (and an additional closed one), so let's go for `RUF061`, which I didn't see any results for.

---

_Comment by @MichaReiser on 2025-04-30 06:40_

We should figure out the scope of this rule before landing this PR because renaming is somewhat annoying (we need to setup redirects for documentation). 

Whether it should be one or multiple rules normally comes down to whether there are cases where a user might only want to be warned about `pytest.warns` but not about `pytest.deprecated_call` or `pytest.raises`. 

---

_Comment by @ntBre on 2025-04-30 13:22_

> Whether it should be one or multiple rules normally comes down to whether there are cases where a user might only want to be warned about `pytest.warns` but not about `pytest.deprecated_call` or `pytest.raises`.

From that perspective, I'd probably lean towards one rule because it seems more consistent stylistically to move them all at once. But I'm definitely open to other ideas. I've only personally used the context manager forms of these.

We can still add support for the other functions in follow-up PRs (unless they're easy to add here), but we should definitely decide on the scope and name here to avoid future churn with the name, as Micha said. 

---

_Comment by @twentyone212121 on 2025-05-02 01:47_

I agree with Brent that it makes sense to combine all of these into 1 rule. I've made the following changes to the PR:

- In the first commit, I renamed the rule and moved it to the `RUF` category.
- In the second one, I've expanded the rule to cover `pytest.warns` and `pytest.deprecated_call`. Also, renamed it to `LegacyFormPytestRaisesWarnsDeprecatedCall` (I'm open to suggestions about the name). 

Note that unlike `pytest.raises`, both `pytest.warns` and `pytest.deprecated_call` return the result of the `func` they execute, not a context. Therefore, if these functions' results are assigned to a target, we treat it as an assign target in the body rather than an optional var. I tried to minimize code duplication by handling all variants in the same functions, which may make the code appear somewhat complex.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:131 on 2025-05-12 18:29_

```suggestion
    let stmt = semantic.current_statement();
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:135 on 2025-05-12 18:30_

```suggestion
        if let Some(with_stmt) = try_fix_legacy_call(context_type, stmt, semantic) {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:181 on 2025-05-12 18:37_

I think some example Python code in a comment might help me here. It seems like this is doing something important, but all I can think is "didn't we already call `PytestContextType::from_expr_name` above?" What kind of code will lead to the different branches here?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:230 on 2025-05-12 18:43_

I think we could use `and_then` here (but haven't tested it):

```suggestion
    let expected = context_type.expected_arg().and_then(|(name, position)| legacy_call.arguments.find_argument_value(name, position));
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF061_RUF061_deprecated_call.py.snap`:57 on 2025-05-12 18:47_

Hmm, it would be kind of nice if we could unwrap the `lambda` since this looks a bit awkward, but I'm not sure how tricky that would be.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:106 on 2025-05-12 18:49_

Could we do a `Display` impl here? Then we could possibly store a `PytestContextType` on the `Violation` struct and just pass it directly to the `format!` calls above.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:46 on 2025-05-12 18:57_

This name seems a bit unwieldy, but I'm not really coming up with something better. Is there some kind of catch-all term that covers all three? 

The pytest docs for `warns` actually say:

> Assert that code **raises** a particular class of warning.

So I think it would be fairly reasonable just to call it `LegacyFormPytestRaises` still. Not to say it's an ideal practice, but we do have some other rules with names that are more specific than the actual implementation, like [yield-outside-function (F704)](https://docs.astral.sh/ruff/rules/yield-outside-function/#yield-outside-function-f704), which actually covers `yield`, `yield from`, and even `await`.

---

_@ntBre reviewed on 2025-05-12 18:58_

Thanks! This is looking great. Just a few minor nits/suggestions/questions.

---

_@twentyone212121 reviewed on 2025-05-18 11:22_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:181 on 2025-05-18 11:22_

This handles two cases for legacy calls:
1. Direct usage: `pytest.raises(ZeroDivisionError, func, 1, b=0)`
2. With match method: `pytest.raises(ZeroDivisionError, func, 1, b=0).match("division by zero")`

The second branch specifically looks for `raises().match()` pattern which only exists for `raises` (not `warns`/`deprecated_call`) since only `raises` returns an `ExceptionInfo` object with a `.match()` method. We need to preserve this match condition when converting.

I have modified the comment to explain it better. There is also a test case for this in `RUF061_raises.py`:
```Python
def test_error_match():
    pytest.raises(ZeroDivisionError, func, 1, b=0).match("division by zero")
```

---

_@twentyone212121 reviewed on 2025-05-18 11:22_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:230 on 2025-05-18 11:22_

I think we can't use `and_then` here because we need different behavior:
For `raises`/`warns`, the `?` operator returns early if the argument isn't found.
For `deprecated_call`, we expect `None` and continue.

---

_@twentyone212121 reviewed on 2025-05-18 11:22_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF061_RUF061_deprecated_call.py.snap`:57 on 2025-05-18 11:22_

Yes, unwrapping the lambda would be cleaner output. For simple cases like this with no arguments, we could extract the lambda body and use it directly.

However, I am not sure if it's within the scope of the current rule. If you think it's worth addressing now though, I can expand the implementation.

---

_@twentyone212121 reviewed on 2025-05-18 11:22_

---

_Review comment by @twentyone212121 on `crates/ruff_linter/src/rules/ruff/rules/legacy_form_pytest_raises.rs`:46 on 2025-05-18 11:22_

Yes, it's quite long. I agree that `LegacyFormPytestRaises` will be cleaner. I renamed it in the latest commit.

---

_Comment by @jakkdl on 2025-05-26 10:09_

Oh I'd completely missed this PR, happy to see progress despite my PR to `flake8-pytest-style` going stale. :rocket: 

I suppose I can see the logic that transforming
```python
with pytest.raises(ValueError) as excinfo:
    ...
assert excinfo.match("bar")
```
into
```python
with pytest.raises(ValueError, match="bar"):
    ...
```
might not be the primary responsibility of this rule, but I do think it's a shame as the latter is *much* cleaner.

---

_@ntBre approved on 2025-06-16 14:38_

Thanks and sorry for the delay here. I think all of my questions have been resolved. 

Could you merge `main` and update the title/description? GitHub isn't showing any conflicts, but I've made some changes to our diagnostic reporting that I think will actually cause problems if we don't merge first. Basically instead of `Diagnostic::new` and a later `checker.report_diagnostic`, you'll just need to call `checker.report_diagnostic` with whatever you previously passed to `Diagnostic::new`. I'm happy to help with that or take care of it if you prefer.

Other than that I think this is good to go.

---

_Label `preview` added by @ntBre on 2025-06-16 14:38_

---

_Label `preview` added by @ntBre on 2025-06-16 14:38_

---

_Comment by @twentyone212121 on 2025-06-16 16:21_

Thanks! I’ve merged `main` and fixed the build errors from the diagnostic changes. Also updated the title earlier. It’s now `LegacyFormPytestRaises` as you suggested earlier. Or did you mean the PR title/description?

---

_Renamed from "[`flake8-pytest-style`] Deprecated `pytest.raises` callable form rule + fix (`PT032`)" to "[`ruff`] Check for non-contextmanager use of `pytest.raises` & friends (`RUF061`)" by @twentyone212121 on 2025-06-16 16:23_

---

_@ntBre approved on 2025-06-16 17:00_

Thanks! And nice work on the `AtomicNodeIndex` changes, I hadn't even seen those myself yet.

> Thanks! I’ve merged main and fixed the build errors from the diagnostic changes. Also updated the title earlier. It’s now LegacyFormPytestRaises as you suggested earlier. Or did you mean the PR title/description?

Yeah I meant the PR title and description, but I see you got those too. Thanks again!

---

_Renamed from "[`ruff`] Check for non-contextmanager use of `pytest.raises` & friends (`RUF061`)" to "[`ruff`] Check for non-context-manager use of `pytest.raises`, `pytest.warns`, and `pytest.deprecated_call` (`RUF061`)" by @ntBre on 2025-06-16 17:03_

---

_Comment by @ntBre on 2025-06-16 17:03_

(Just made the title a bit more verbose for inclusion directly into the release notes!)

---

_Merged by @ntBre on 2025-06-16 17:03_

---

_Closed by @ntBre on 2025-06-16 17:03_

---
