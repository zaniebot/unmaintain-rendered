```yaml
number: 20693
title: New Rule to Validate Pytest Argnames
type: issue
state: open
author: George-Ogden
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-03T15:36:28Z
updated_at: 2025-10-03T19:35:27Z
url: https://github.com/astral-sh/ruff/issues/20693
synced_at: 2026-01-12T15:54:57Z
```

# New Rule to Validate Pytest Argnames

---

_@George-Ogden_

### Summary

#### Intro 

Hey,

I've been relying on Ruff heavily in my projects for pre-commit checks, linting and the VS Code extension. Like many developers, I make a lot of mistakes and use other tools to compensate for my finite memory.

#### Overview

One mistake I frequently make is changing the names of arguments in my test and then not updating the `argnames` string when using Pytest. 

#### Example
Here's an example:
```python
@pytest.mark.parametrize("object", [["foo", "bar"]])
def test_simple(objects: list[str]) -> None: ...
```
Here, "object" is incorrect and `argnames` should be "objects" instead. I would like to create a rule that warns you of this error. I think that checking for arguments in `argnames` that are not function arguments would be a good place to start, before trying to validate all the `argnames`.

Looking at [crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs), it seems like this would be possible to implement, but nontrivial. I haven't contributed to Ruff before, but I would be keen to work on this.

#### Edge Cases
There are a lot of edge cases, which I'm going to list, that make this harder:

##### Multiple Arguments
```python
@pytest.mark.parametrize("arg1, arg2", [(1, 1.0), (2, 2.0)])
def test_multi_args_together(arg1: int, arg2: float) -> None: ...

@pytest.mark.parametrize("arg1", [1, 2])
@pytest.mark.parametrize("arg2", [1.0, 2.0])
def test_multi_args_independent(arg1: int, arg2: float) -> None: ...
```

##### Automatic Fixtures
```python
@pytest.fixture
def auto_fixture(): ...

@pytest.mark.parametrize("arg", [None])
def test_auto_fixture(auto_fixture: pytest.FixtureDef, arg: None) -> None: ...
```
This includes default Pytest fixtures, such as `capsys`.

##### Indirect Fixtures
```python
@pytest.fixture(autouse=True)
def indirect_fixture(indirect_arg): # may exist in a different file
    yield indirect_arg

@pytest.mark.parametrize("indirect_arg", [1, 2, 3])
def test_indirect(indirect_fixture: int) -> None: ...
```
Solving this specific case would require checking `conftest.py` files in parent directories to be fully exhaustive. A full algorithm would create a graph to identify whether the current arguments form a valid path to the root (current test). A simplified version could create a set of valid argument names (from all fixtures and tests) and validate whether each argname is the argument to one of these, raising an error if not.

#### Closing

Thanks for your hard work on Ruff so far, and I'd be interested to hear your thoughts on this.

---

_Comment by @ntBre on 2025-10-03 19:35_

Thanks for the kind words and the suggestion!

This does sound reasonable to me and related to the [pytest-parametrize-names-wrong-type (PT006)](https://docs.astral.sh/ruff/rules/pytest-parametrize-names-wrong-type/#pytest-parametrize-names-wrong-type-pt006) implementation you found. It also reminds me of [fast-api-unused-path-parameter (FAST003)](https://docs.astral.sh/ruff/rules/fast-api-unused-path-parameter/#fast-api-unused-path-parameter-fast003).

I'll just add the `needs-decision` label for now to give others a chance to weigh in.

---

_Label `rule` added by @ntBre on 2025-10-03 19:35_

---

_Label `needs-decision` added by @ntBre on 2025-10-03 19:35_

---
