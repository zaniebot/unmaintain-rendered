```yaml
number: 14699
title: "Fix `pytest-parametrize-names-wrong-type (PT006)` to edit both `argnames` and `argvalues` if both of them are single-element tuples/lists"
type: pull_request
state: merged
author: harupy
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PT006-single-element
created_at: 2024-12-01T13:42:25Z
updated_at: 2024-12-09T12:45:54Z
url: https://github.com/astral-sh/ruff/pull/14699
synced_at: 2026-01-12T15:55:48Z
```

# Fix `pytest-parametrize-names-wrong-type (PT006)` to edit both `argnames` and `argvalues` if both of them are single-element tuples/lists

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Close #11243. Fix `pytest-parametrize-names-wrong-type (PT006)` to edit both `argnames` and `argvalues` if both of them are single-element tuples/lists.

```python
# Before fix
@pytest.mark.parametrize(("x",), [(1,), (2,)])
def test_foo(x):
    ...

# After fix:
@pytest.mark.parametrize("x", [1, 2])
def test_foo(x):
    ...
```

## Test Plan

<!-- How was it tested? -->

New test cases


---

_Comment by @github-actions[bot] on 2024-12-02 00:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-12-02 01:28_

I'm sure you're right, but can you expand on why that's a problem? (Should we not be raising a diagnostic at all?)

---

_Comment by @harupy on 2024-12-02 11:57_

@charliermarsh Thanks for the comment!

> Should we not be raising a diagnostic at all?

This is another option.

---

_Comment by @harupy on 2024-12-02 12:00_

I just found https://github.com/astral-sh/ruff/issues/11243. Should we unpack each item in `argvalues` as a fix?

---

_Comment by @charliermarsh on 2024-12-02 13:34_

In this case, would _this_ be correct?

```
@pytest.mark.parametrize("x,", [(1,), (2,)])
```

With the comma inside the string? I don't know enough about pytest.

---

_Comment by @harupy on 2024-12-02 13:41_

@charliermarsh Let me check.

---

```python
import pytest


@pytest.mark.parametrize("x,", [(1,), (2,)])
def test_foo(x):
    assert isinstance(x, int)
```

```
_____________________________________________________________________________________________________ test_foo[x0] ______________________________________________________________________________________________________

x = (1,)

    @pytest.mark.parametrize("x,", [(1,), (2,)])
    def test_foo(x):
>       assert isinstance(x, int)
E       assert False
E        +  where False = isinstance((1,), int)

a.py:6: AssertionError
_____________________________________________________________________________________________________ test_foo[x1] ______________________________________________________________________________________________________

x = (2,)

    @pytest.mark.parametrize("x,", [(1,), (2,)])
    def test_foo(x):
>       assert isinstance(x, int)
E       assert False
E        +  where False = isinstance((2,), int)

a.py:6: AssertionError
```

`@pytest.mark.parametrize("x,", [(1,), (2,)])` is equivalent to `@pytest.mark.parametrize("x", [(1,), (2,)])`.

---

_Comment by @MichaReiser on 2024-12-03 10:04_

Source of the relevant pytest code 

https://github.com/pytest-dev/pytest/blob/9d4f36d87dae9a968fb527e2cb87e8a507b0beb3/src/_pytest/mark/structures.py#L124-L135

According to the rule. How should a user rewrite your example? 

```
@pytest.mark.parametrize(("x",), [(1,), (2,)])
                       # ^^^^^^
def test_foo(x):
    ...
```

---

_Comment by @harupy on 2024-12-03 10:37_

@MichaReiser 

```python
@pytest.mark.parametrize(("x",), [(1,), (2,)])
def test_foo(x):
    ...
```

should be rewritten to:


```python
@pytest.mark.parametrize("x", [1, 2])
def test_foo(x):
    ...

# I'd prefer this style because I don't need to wonder what x is (tuple vs. int)
```

Both `argnames` and items in `argvalues` need to be fixed. PT006 only fixes `argnames` now.

(I implemented a fix to fix `argvalues` as well but didn't push it yet).

---

_Renamed from "Fix `pytest-parametrize-names-wrong-type (PT006)` not to suggest a fix if both `argnames` and `argvalues` are single-element sequences" to "Fix `pytest-parametrize-names-wrong-type (PT006)` to edit `argnames` and `argvalues` as a fix" by @harupy on 2024-12-03 14:37_

---

_Renamed from "Fix `pytest-parametrize-names-wrong-type (PT006)` to edit `argnames` and `argvalues` as a fix" to "Fix `pytest-parametrize-names-wrong-type (PT006)` to edit both `argnames` and `argvalues` if both of them are single-element tuples/lists" by @harupy on 2024-12-03 14:38_

---

_Comment by @MichaReiser on 2024-12-04 08:21_

> (I implemented a fix to fix argvalues as well but didn't push it yet).

That sounds great. I'll have a look when you push the change

---

_Label `bug` added by @MichaReiser on 2024-12-04 08:21_

---

_Label `fixes` added by @MichaReiser on 2024-12-04 08:21_

---

_@harupy reviewed on 2024-12-04 08:56_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:729 on 2024-12-04 08:56_

@MichaReiser 

```suggestion
    diagnostic.set_fix(Fix::unsafe_edits(
```

Should `unsafe_edits` be used in case `argvalues` contains comments?

```python
pytest.mark.parametrize(
    "param",
    [
        (
            # hello
            1,
        ),
        (2,),
    ],
)
```

---

_@MichaReiser reviewed on 2024-12-04 09:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:729 on 2024-12-04 09:09_

If the comment gets removed, then yes, the fix should be marked as unsafe

---

_@harupy reviewed on 2024-12-04 11:00_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:729 on 2024-12-04 11:00_

Got it. I'll make this change.

---

_Review requested from @MichaReiser by @harupy on 2024-12-04 11:10_

---

_@harupy reviewed on 2024-12-04 14:35_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:744 on 2024-12-04 14:35_

Should I add a test to ensure this doesn't conflict with PT007 which may also edit `argvalues`?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:744 on 2024-12-05 07:49_

This sounds like a great idea if we're concerned about whether the rules play nicely together.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:718 on 2024-12-05 07:53_

Can we extend the rule documentation with when the fix is unsafe? That would also help me understand why we changed the fix to unsafe :) If it's because of possible comments, then we should change the fix to use `checker.comment_ranges().has_comments(range)` to test if the edited range contains comments and only set the fix to unsafe if that's the case.



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:737 on 2024-12-05 07:56_

What does it mean if `elts` contain more than one element? Is it possible? Should we error or not provide a fix in that case?

---

_@MichaReiser reviewed on 2024-12-05 07:57_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:737 on 2024-12-05 11:48_

That's possible. That would make `pytest.mark.parametrize` throw. A fix should not be provided.

---

_@harupy reviewed on 2024-12-05 11:48_

---

_@harupy reviewed on 2024-12-05 11:59_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:718 on 2024-12-05 11:59_

It's because of possible comments. I'll use has_comments to determine safety

---

_Review requested from @MichaReiser by @harupy on 2024-12-05 14:14_

---

_Review comment by @harupy on `crates/ruff_linter/resources/test/fixtures/flake8_pytest_style/PT006.py`:158 on 2024-12-06 12:02_

Maybe a fix should not be provided in this case because the fix would hide the error.

---

_@harupy reviewed on 2024-12-06 12:02_

---

_@harupy reviewed on 2024-12-06 13:48_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:744 on 2024-12-06 13:48_

@MichaReiser Manually confirmed PT006 and PT007 don't conflicts:


```python
> cargo run -p ruff -- check --no-cache --select PT007,PT006 a.py --unsafe-fixes --fix
--- a.py
+++ a.py
@@ -1,6 +1,6 @@
 import pytest
 
 
-@pytest.mark.parametrize(("param",), [[1], [2], [3]])
+@pytest.mark.parametrize("param", [1, 2, 3])
 def test_foo(param):
     ...

Would fix 1 error.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:683 on 2024-12-06 16:37_

Nit: I suggest moving this method after `handle_single_name`. It eases the reading flow. I start with `check_names`, then jump to `handle_single_name`, and then keep reading this method.

---

_@MichaReiser reviewed on 2024-12-06 16:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:737 on 2024-12-06 16:38_

Would you mind adding a a test for this and handle it appropriately (by bailing to create a fix)?

---

_@MichaReiser reviewed on 2024-12-06 16:39_

---

_@harupy reviewed on 2024-12-07 02:29_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:683 on 2024-12-07 02:29_

Sure!

---

_Review requested from @MichaReiser by @harupy on 2024-12-07 03:19_

---

_@harupy reviewed on 2024-12-07 04:38_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:737 on 2024-12-07 04:38_

Added!

---

_@harupy reviewed on 2024-12-07 04:39_

---

_Review comment by @harupy on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs`:744 on 2024-12-07 04:39_

Added a test case for this.

---

_@MichaReiser approved on 2024-12-09 08:58_

This is great. Thank you for following it thourgh

---

_Merged by @MichaReiser on 2024-12-09 08:58_

---

_Closed by @MichaReiser on 2024-12-09 08:58_

---

_Branch deleted on 2024-12-09 12:45_

---
