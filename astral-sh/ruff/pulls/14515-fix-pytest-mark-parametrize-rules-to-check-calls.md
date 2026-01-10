```yaml
number: 14515
title: "Fix `pytest.mark.parametrize` rules to check calls instead of decorators"
type: pull_request
state: merged
author: harupy
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-pytest-parametrize
created_at: 2024-11-21T12:57:13Z
updated_at: 2024-11-25T12:58:29Z
url: https://github.com/astral-sh/ruff/pull/14515
synced_at: 2026-01-10T20:50:57Z
```

# Fix `pytest.mark.parametrize` rules to check calls instead of decorators

---

_Pull request opened by @harupy on 2024-11-21 12:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Currently, rules for `pytest.mark.parametrize` (e.g. `/pytest-parametrize-names-wrong-type (PT006)`) only check decorators, but `pytest.mark.parametrize` can appear as a function call as show below.

```python
# To parametrize all tests in a module, you can assign to pytest mark


import pytest

pytestmark = pytest.mark.parametrize("n,expected", [(1, 2), (3, 4)])


class TestClass:
    def test_simple_case(self, n, expected):
        assert n + 1 == expected

    def test_weird_simple_case(self, n, expected):
        assert (n * 1) + 1 == expected
```

- how-to guide: https://docs.pytest.org/en/stable/how-to/parametrize.html
- API reference: https://docs.pytest.org/en/stable/reference/reference.html#pytest.Metafunc.parametrize

## Test Plan

<!-- How was it tested? -->

Existing tests and a new test case to ensure the fix is valid.


---

_Comment by @harupy on 2024-11-21 13:17_

The upstream rule also checks calls:

https://github.com/m-burst/flake8-pytest-style/blob/2addab8b533ea101e25d022a37dc32d89db6c688/flake8_pytest_style/visitors/parametrize.py#L130-L132

```python
    def visit_Call(self, node: ast.Call) -> None:
        if is_parametrize_call(node):
            self._check_parametrize_call(node)
```

---

_@harupy reviewed on 2024-11-21 13:21_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:811 on 2024-11-21 13:21_

Is there a simpler way to write this?

---

_Comment by @github-actions[bot] on 2024-11-21 13:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+50 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+48 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/helm_tests/airflow_aux/test_annotations.py#L249'>helm_tests/airflow_aux/test_annotations.py:249:5:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L157'>providers/tests/dbt/cloud/hooks/test_dbt.py:157:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L168'>providers/tests/dbt/cloud/hooks/test_dbt.py:168:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L178'>providers/tests/dbt/cloud/hooks/test_dbt.py:178:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L201'>providers/tests/dbt/cloud/hooks/test_dbt.py:201:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L216'>providers/tests/dbt/cloud/hooks/test_dbt.py:216:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L235'>providers/tests/dbt/cloud/hooks/test_dbt.py:235:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L254'>providers/tests/dbt/cloud/hooks/test_dbt.py:254:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L273'>providers/tests/dbt/cloud/hooks/test_dbt.py:273:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L294'>providers/tests/dbt/cloud/hooks/test_dbt.py:294:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L315'>providers/tests/dbt/cloud/hooks/test_dbt.py:315:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L334'>providers/tests/dbt/cloud/hooks/test_dbt.py:334:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L356'>providers/tests/dbt/cloud/hooks/test_dbt.py:356:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L388'>providers/tests/dbt/cloud/hooks/test_dbt.py:388:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L423'>providers/tests/dbt/cloud/hooks/test_dbt.py:423:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L453'>providers/tests/dbt/cloud/hooks/test_dbt.py:453:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L458'>providers/tests/dbt/cloud/hooks/test_dbt.py:458:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L512'>providers/tests/dbt/cloud/hooks/test_dbt.py:512:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L534'>providers/tests/dbt/cloud/hooks/test_dbt.py:534:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L559'>providers/tests/dbt/cloud/hooks/test_dbt.py:559:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L586'>providers/tests/dbt/cloud/hooks/test_dbt.py:586:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L603'>providers/tests/dbt/cloud/hooks/test_dbt.py:603:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L624'>providers/tests/dbt/cloud/hooks/test_dbt.py:624:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L692'>providers/tests/dbt/cloud/hooks/test_dbt.py:692:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/dbt/cloud/hooks/test_dbt.py#L711'>providers/tests/dbt/cloud/hooks/test_dbt.py:711:18:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
... 23 additional changes omitted for rule PT006
+ <a href='https://github.com/apache/airflow/blob/5a68bca9b0378901bd5e6169dd776881feade5db/providers/tests/microsoft/azure/hooks/test_data_factory.py#L151'>providers/tests/microsoft/azure/hooks/test_data_factory.py:151:13:</a> PT007 Wrong values type in `@pytest.mark.parametrize` expected `list` of `tuple`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/setuptools/blob/9692cde009af4651819d18a1e839d3b6e3fcd77d/pkg_resources/tests/test_working_set.py#L107'>pkg_resources/tests/test_working_set.py:107:9:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
+ <a href='https://github.com/pypa/setuptools/blob/9692cde009af4651819d18a1e839d3b6e3fcd77d/setuptools/tests/test_egg_info.py#L308'>setuptools/tests/test_egg_info.py:308:17:</a> PT006 Wrong type passed to first argument of `@pytest.mark.parametrize`; expected `tuple`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT006 | 49 | 49 | 0 | 0 | 0 |
| PT007 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_@MichaReiser reviewed on 2024-11-21 16:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:811 on 2024-11-21 16:20_

You could do (and hope for the compiler to optimize a way the second call)
```suggestion
				let names = call.arguments.find_argument("argnames", 0) {
        let values = call.arguments.find_argument("argvalues", 1);
        
        if let (Some(names), Some(values)) = (names, values) {
                check_values(checker, names, values);
```

---

_Review requested from @MichaReiser by @harupy on 2024-11-22 13:23_

---

_@harupy reviewed on 2024-11-23 10:29_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:799 on 2024-11-23 10:29_

Unrelated to this PR, but PT rules don't have a `seen_module` guard. Shoud we add it?

---

_@harupy reviewed on 2024-11-23 13:12_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:817 on 2024-11-23 13:12_

This aligns with the upstream implementation:

https://github.com/m-burst/flake8-pytest-style/blob/2addab8b533ea101e25d022a37dc32d89db6c688/flake8_pytest_style/utils.py#L96

```python
def extract_parametrize_call_args(node: ast.Call) -> Optional[ParametrizeArgs]:
    """Extracts argnames, argvalues and ids from a parametrize call."""
    args = get_simple_call_args(node)

    names_arg = args.get_argument('argnames', 0)
    if names_arg is None:
        return None

    values_arg = args.get_argument('argvalues', 1)
    ids_arg = args.get_argument('ids')
    return ParametrizeArgs(names_arg, values_arg, ids_arg)
```

---

_Label `rule` added by @MichaReiser on 2024-11-25 10:03_

---

_Comment by @MichaReiser on 2024-11-25 10:05_

Thanks for the how-to-guide and API reference. That helps a lot with building the required context to review the rule. I really appreciate it :)

---

_@MichaReiser approved on 2024-11-25 10:09_

This is great. Did you already look through the ecosystem changes? I can do it, if you haven't. 

I think we should gate this change behind preview mode because I consider this a "significant increase in scope" and we should only make such changes in minor releases.

---

_@MichaReiser reviewed on 2024-11-25 10:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:799 on 2024-11-25 10:10_

Yeah we could. 

---

_Comment by @harupy on 2024-11-25 10:38_

@MichaReiser Thanks for the comment. This PR has the two changes. Do we want to gate both of them?

1. Only calls in decorators are covered -> Any calls are covered.
2. Only positional arguments are covered -> Both positional and keyword arguments are covered.


---

_Comment by @MichaReiser on 2024-11-25 11:08_

Oh right. Making 1. preview-only for now seems more important to me than 2. We can have a look at the ecosystem change to understand how much impact 2 has but I think it's fine to not gate 2.

---

_Comment by @harupy on 2024-11-25 11:37_

For Airflow, 2 has more impacts (more changes) than 1.

---

_Comment by @MichaReiser on 2024-11-25 11:56_

Right, I should have taken a closer look at the ecosystem results. We should then probably make 2 a preview style change as well. It's a bit annoying codewise but feels most in line with our versioning policy

---

_Review requested from @MichaReiser by @harupy on 2024-11-25 12:29_

---

_@harupy reviewed on 2024-11-25 12:29_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:799 on 2024-11-25 12:29_

Sounds like a nice good first issue.

---

_Label `preview` added by @MichaReiser on 2024-11-25 12:55_

---

_Merged by @MichaReiser on 2024-11-25 12:55_

---

_Closed by @MichaReiser on 2024-11-25 12:55_

---

_Branch deleted on 2024-11-25 12:58_

---
