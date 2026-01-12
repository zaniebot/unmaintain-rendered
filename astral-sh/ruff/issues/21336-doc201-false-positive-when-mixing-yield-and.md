```yaml
number: 21336
title: "DOC201: false positive when mixing yield and return statements"
type: issue
state: open
author: loenard97
labels:
  - bug
assignees: []
created_at: 2025-11-08T10:32:58Z
updated_at: 2025-11-10T14:45:49Z
url: https://github.com/astral-sh/ruff/issues/21336
synced_at: 2026-01-12T15:54:57Z
```

# DOC201: false positive when mixing yield and return statements

---

_@loenard97_

### Summary

# Current unexpected behaviour
Using `uvx ruff check --preview --select DOC201,DOC202 --ignore D203,D212 --isolated` on the following code
```python
def my_generator(val: object) -> object:
    """
    Minimal generator.

    Yields:
        val

    """
    if val is not None:
        yield val

    return

```
raises a `DOC201` suggesting to add a `Returns` section to the docstirng. However, trying to fix it by adding a `Returns` section
```python
def my_generator(val: object) -> object:
    """
    Minimal generator.

    Yields:
        val

    Returns:
        None

    """
    if val is not None:
        yield val

    return

```
now raises a `DOC202` suggesting to remove the `Returns` section again.

[Playground link](https://play.ruff.rs/b7c0be7a-a188-4ef8-92f3-cc602dade0ff)

# Expected behaviour
Seems to me like the first code snippet is not supposed to raise a `DOC201`, because the return does not return anything.  Quote from [DOC201 rules](https://docs.astral.sh/ruff/rules/docstring-missing-returns/):
 > This rule is not enforced for abstract methods or functions that only return None.

The problem seems be the `yield val` statement. If I change that to `return val`, it works as expected and the second example does not raise any linter errors. The obvious fix is ofc to remove the redundant `return` from the last line, which then works as expected, but the behaviour of ruff still seems to be unexpected to me.

### Version

ruff 0.14.4 (c7ff9826d 2025-11-06)

---

_Comment by @MichaReiser on 2025-11-10 09:25_

There's certainly a mismatch between `DOC201` and `DOC202`

`DOC201` takes the return type into account and only allows an implicit `return` (without a value) if the function has no annotated return type. 

https://github.com/astral-sh/ruff/blob/a630a46849bd25c461aeda2e1efa73c0e1b37a34/crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs#L1223-L1230

`DOC202` always allows implicit returns

https://github.com/astral-sh/ruff/blob/a630a46849bd25c461aeda2e1efa73c0e1b37a34/crates/ruff_linter/src/rules/pydoclint/rules/check_docstring.rs#L1312-L1316



---

_Label `bug` added by @MichaReiser on 2025-11-10 09:25_

---

_Comment by @loenard97 on 2025-11-10 14:40_

After looking at it again, I think there are two separate things here:
Concerning `DOC202` ([Playground link](https://play.ruff.rs/b5f831c3-46cc-4ccb-88af-686f0c68ca5b)):
```python
def func(val):
    """
    Empty function.

    Returns:
        nothing

    """
    return None

```
This code snippet _does not_ trigger a `DOC202` error, but replacing the `return None` with a `return` _does_ trigger a `DOC202`. IMO these two things should behave the same way, because the executed Python behaves the same way. Note, that `DOC201` does treat `return` and `return None` exactly the same way.


Concerning `DOC202`:
as @MichaReiser noted, `DOC202` takes the return annotation into account:
```python
def func(val) -> int:
    """
    Empty function.

    """
    return

```
This code snippet raises a `DOC202`. Note, that it is explicitly the `int` annotation, that triggers the `DOC202`, but not the `return`. Removing the annotation does not trigger a `DOC202` as expected. However, leaving the `int` annotation in, but changing the `return` to a `pass`, does suddenly not trigger a `DOC202` anymore. 

IMO a clarification would be good whether the `DOC` rules should take annotations into account or not and then make their behaviour consistent. (My personal opinion would be, that these docstring rules should not take annotations into account and only parse the actual code. Only after these docstring rules I would validate rules that concern annotations and check whether the annotations are correct with regards to the code. I.e., the code above should trigger an error that the `int` annotation is wrong, but no error about the docstrings. I feel like that's not for me to decide though.)

After this is clarified, would this be a good first contribution that I could try to solve? I know Python and Rust, but haven't done anything in the ruff code base yet. I would be interested in figureing out a PR for this.

---
