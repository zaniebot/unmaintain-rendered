```yaml
number: 17650
title: "[`flake8-pytest-style`] Don't recommend `usefixtures` for parametrize values in `PT019`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-PT019
created_at: 2025-04-26T22:27:44Z
updated_at: 2025-05-14T15:08:22Z
url: https://github.com/astral-sh/ruff/pull/17650
synced_at: 2026-01-12T15:56:03Z
```

# [`flake8-pytest-style`] Don't recommend `usefixtures` for parametrize values in `PT019`

---

_@LaBatata101_



<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #17599.

## Test Plan

Snapshot tests.


---

_Comment by @github-actions[bot] on 2025-04-26 22:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L944'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:944:89:</a> PT019 Fixture `_json` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/providers/fab/tests/unit/fab/www/views/test_views_custom_user_views.py#L102'>providers/fab/tests/unit/fab/www/views/test_views_custom_user_views.py:102:71:</a> PT019 Fixture `_` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/providers/google/tests/unit/google/common/test_deprecated.py#L155'>providers/google/tests/unit/google/common/test_deprecated.py:155:61:</a> PT019 Fixture `_str` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/providers/standard/tests/unit/standard/operators/test_weekday.py#L297'>providers/standard/tests/unit/standard/operators/test_weekday.py:297:57:</a> PT019 Fixture `_` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT019 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py#L944'>airflow-core/tests/unit/api_fastapi/execution_api/versions/head/test_task_instances.py:944:89:</a> PT019 Fixture `_json` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/providers/fab/tests/unit/fab/www/views/test_views_custom_user_views.py#L102'>providers/fab/tests/unit/fab/www/views/test_views_custom_user_views.py:102:71:</a> PT019 Fixture `_` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/providers/google/tests/unit/google/common/test_deprecated.py#L155'>providers/google/tests/unit/google/common/test_deprecated.py:155:61:</a> PT019 Fixture `_str` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
- <a href='https://github.com/apache/airflow/blob/3b93650a4f6fd2c7607bfdc75f8a18c5c99a5d40/providers/standard/tests/unit/standard/operators/test_weekday.py#L297'>providers/standard/tests/unit/standard/operators/test_weekday.py:297:57:</a> PT019 Fixture `_` without value is injected as parameter, use `@pytest.mark.usefixtures` instead
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PT019 | 4 | 0 | 4 | 0 | 0 |

</p>
</details>




---

_Renamed from "[`flake8_pytest_style`] Don't recommend `usefixtures` for parametrize values" to "[`flake8_pytest_style`] Don't recommend `usefixtures` for parametrize values in `PT019`" by @LaBatata101 on 2025-04-26 22:35_

---

_Label `rule` added by @ntBre on 2025-04-28 20:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/helpers.rs`:32 on 2025-04-30 21:03_

We could write this slightly more tersely as

```suggestion
    decorators.iter().filter(|decorator| {
        UnqualifiedName::from_expr(map_callable(&decorator.expression))
            .is_some_and(|name| matches!(name.segments(), ["pytest", "mark", "parametrize"]))
    })
```

At that point, I wouldn't really mind just inlining this into `check_test_function_args` since the helper isn't called anywhere else, but I don't feel strongly about either part of this suggestion.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:827 on 2025-04-30 21:20_

I think you could use something like this here:

```suggestion
        let first_arg = call_expr
            .arguments
            .find_argument_value("argnames", 0)
            .and_then(Expr::as_string_literal_expr);
```

However, based on the [docs](https://docs.pytest.org/en/stable/reference/reference.html#pytest.Metafunc.parametrize), I'm not sure the check for a string literal is entirely correct:

> Parameters:
argnames ([str](https://docs.python.org/3/library/stdtypes.html#str) | [Sequence](https://docs.python.org/3/library/typing.html#typing.Sequence)[[str](https://docs.python.org/3/library/stdtypes.html#str)]) – A comma-separated string denoting one or more argument names, or a list/tuple of argument strings.

Based on this, I think it can either be a comma-separated string or a list/tuple of strings. It also works with variables, not just literals, but that might be too rare to worry about much:

```python
import pytest

x = "test_input,expected"

@pytest.mark.parametrize(x, [("3+5", 8), ("2+4", 6), ("6*9", 42)])
def test_eval(test_input, expected):
    assert eval(test_input) == expected
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:834 on 2025-04-30 21:23_

We may need to strip whitespace from these as well. I tried `x = "test_input, expected"` in the example above and it worked fine.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:836 on 2025-04-30 21:23_

I think we could `extend` with the iterator directly here instead of a loop and `insert`.

---

_@ntBre requested changes on 2025-04-30 21:26_

Thanks!

The logic here looks good. I made a couple of stylistic suggestions and also noted a couple of additional cases we may need to handle for `parameterize` argument types.

---

_@LaBatata101 reviewed on 2025-04-30 21:57_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:842 on 2025-04-30 21:57_

I've used `filter_map` here, so it would ignore stuff that isn't a string literal. Do you think this is fine, or should we skip the check if the user passes something that isn't a string?

---

_Review requested from @ntBre by @LaBatata101 on 2025-04-30 21:59_

---

_Renamed from "[`flake8_pytest_style`] Don't recommend `usefixtures` for parametrize values in `PT019`" to "[`flake8-pytest-style`] Don't recommend `usefixtures` for parametrize values in `PT019`" by @LaBatata101 on 2025-05-01 13:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:832 on 2025-05-12 14:33_

I think the `is_empty` check is logically redundant with the `starts_with` check, but maybe it's slightly faster to filter out empty strings first? So it's probably fine to leave it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:821 on 2025-05-12 14:44_

Can we do `let ... else { continue }` here? That would remove the need for the `Some` wrapping in the `match` below.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:850 on 2025-05-12 14:48_

What do you think about returning if we seem an `Expr::Name` either on its own or in a list or tuple? Like I said before, I would guess that's pretty rare, but it would be easy enough to return and avoid the possibility of false positives.

---

_@ntBre reviewed on 2025-05-12 14:49_

Thanks! Just a couple small nits and a question. It might also be good to throw in a test for a string where trimming whitespace matters. If we add special handling for `Expr::Name`, that might deserve a test too.

---

_@LaBatata101 reviewed on 2025-05-12 14:54_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:821 on 2025-05-12 14:54_

Sure

---

_@LaBatata101 reviewed on 2025-05-12 15:02_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:850 on 2025-05-12 15:02_

Sounds good to me

---

_Review requested from @ntBre by @LaBatata101 on 2025-05-12 15:03_

---

_@ntBre reviewed on 2025-05-12 17:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:842 on 2025-05-12 17:15_

I think this makes sense, especially with the new `Name` check. I think strings and variables that resolve to strings are the only valid arguments here.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT019.py`:54 on 2025-05-12 17:16_

Thanks for these. I was actually picturing the case where `x` doesn't correspond to one of the parameters, i.e. a false negative:

```python
x = "something else"
@pytest.mark.parametrize(x, [1, 2, 3])
def test_thingy5(_foo, _bar):  # known false negative, we don't try to resolve variables
    pass
```

I would probably replace these new cases with this false negative case because they're a bit misleading. We're not really resolving `x` to _know_ that it contains `"_bar"`, we're just trading false negatives for false positives.

---

_@ntBre reviewed on 2025-05-12 17:17_

Thanks, just one clarification on the test I was picturing before.

---

_@LaBatata101 reviewed on 2025-05-12 19:24_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT019.py`:54 on 2025-05-12 19:24_

Ah ok, got it!

---

_Review requested from @ntBre by @LaBatata101 on 2025-05-12 19:27_

---

_@ntBre approved on 2025-05-14 14:28_

Thanks!

---

_Merged by @ntBre on 2025-05-14 14:31_

---

_Closed by @ntBre on 2025-05-14 14:31_

---

_Branch deleted on 2025-05-14 15:08_

---
