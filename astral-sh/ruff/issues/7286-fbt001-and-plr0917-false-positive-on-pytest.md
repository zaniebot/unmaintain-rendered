```yaml
number: 7286
title: "`FBT001` and `PLR0917` - false positive on pytest functions"
type: issue
state: open
author: DetachHead
labels:
  - rule
assignees: []
created_at: 2023-09-12T04:46:21Z
updated_at: 2025-01-05T10:03:20Z
url: https://github.com/astral-sh/ruff/issues/7286
synced_at: 2026-01-12T15:54:47Z
```

# `FBT001` and `PLR0917` - false positive on pytest functions

---

_@DetachHead_

```py
# test_foo.py
@fixture
def a():
    return True

def test_foo(a: bool): # error: FBT001
    assert a
```

also happens on hook functions
```py
# conftest.py
def pytest_foo(a: bool): # error: FBT001
    ...
```

arguments to functions called by pytest are always positional, as they are called by pytest itself so this rule is not an issue.

---

_Label `bug` added by @charliermarsh on 2024-01-05 04:48_

---

_Renamed from "`boolean-type-hint-positional-argument` (`FBT001`) false positive on pytest functions" to "`boolean-type-hint-positional-argument` (`FBT001`) - false positive on pytest functions" by @DetachHead on 2024-02-02 22:04_

---

_Renamed from "`boolean-type-hint-positional-argument` (`FBT001`) - false positive on pytest functions" to "`FBT001` and `PLR0917` - false positive on pytest functions" by @DetachHead on 2024-02-16 10:05_

---

_Comment by @DetachHead on 2024-02-16 10:05_

also `too-many-positional` (`PLR0917`)

---

_Comment by @MichaReiser on 2025-01-03 09:25_

I'd argue that the rules should apply to fixtures and parametrized tests because the same problem applies as when calling the function manually (assuming you agree with FBT001 and PLR0917 in the first place). 

For example

```py
@fixture
def a():
	return [(True, False, True), (False, True, False)] # this is kind of hard to read

def pytest_foo(a: bool, b: bool, c: bool): ...
```

The same is true for too many positional arguments. If you don't like too many positional arguments for regular functions because it makes the call site harder to read, then you probably also dislike it for tests. 

In the end, a fixture return, the parametrize list or using a regular call are all just different DSLs for expressing the arguments for a function. 

I think we should only excel functions where the signature isn't under the user's control, which doesn't apply here (as far as I can tell, except that pytest restricts you to use positional arguments, but you could, e.g., return a dict?). I also think that exempting fixtures by default makes Ruff less flexible because there's no way for users who care about enforcing those rules in tests to enable them again. On the other hand, disabling them using `per-file-ignores` or `# ruff: noqa: FBT001` is straightforward enough.

---

_Label `bug` removed by @MichaReiser on 2025-01-03 09:25_

---

_Label `rule` added by @MichaReiser on 2025-01-03 09:25_

---

_Comment by @DetachHead on 2025-01-03 13:10_

> I'd argue that the rules should apply to fixtures and parametrized tests because the same problem applies as when calling the function manually (assuming you agree with FBT001 and PLR0917 in the first place).
> 
> For example
> 
> ```py
> @fixture
> def a():
> 	return [(True, False, True), (False, True, False)] # this is kind of hard to read
> 
> def pytest_foo(a: bool, b: bool, c: bool): ...
> ```

i don't really understand this example. the return type of that `a` fixture is associated with the type of the `a` parameter:

```py
def pytest_foo(a: list[tuple[bool, bool, bool]]):
    ...
```

and it would be equally difficult to understand what each value in the tuple represents regardless of whether `a` was a keyword or positional argument.

> The same is true for too many positional arguments. If you don't like too many positional arguments for regular functions because it makes the call site harder to read, then you probably also dislike it for tests.

i don't think tests should be called manually either. imo if a user wants to do that, they are probably misusing pytest

---

_Comment by @MichaReiser on 2025-01-03 13:38_

Yeah, I'm sorry. I misunderstood the fixture documentation. But I still think that `FBT001` applies. Specifically:

> The use of a boolean will also limit the function to only two possible behaviors, which makes the function difficult to extend in the future.

> Calling a function with boolean positional arguments is confusing as the meaning of the boolean value is not clear to the caller and to future readers of the code.

Where "Calling" is the fixture's return value position. 

But I think it would be easiest to discuss this based on a real world example. Do you have any? Do you know if it is a common pattern to have boolean fixtures?

---

_Comment by @DetachHead on 2025-01-04 01:14_

i still don't believe these are issues at all when it comes to fixtures

> The use of a boolean will also limit the function to only two possible behaviors, which makes the function difficult to extend in the future.

take this fixture and test for example:

```py
@fixture
def condition() -> bool:
    ...

def test_foo(condition: bool):
    ...
```

if another fixture is added to the function in the future, it doesn't matter where the parameter appears in the signature, as long as its name matches the name of the fixture:

```py
@fixture
def condition() -> bool:
    ...

@fixture
def foo() -> bool:
    ...

def test_foo(foo: bool, condition: bool):
    ...
```

> Calling a function with boolean positional arguments is confusing as the meaning of the boolean value is not clear to the caller and to future readers of the code.

this isn't an issue either since fixtures cannot be called directly anyway. pytest is able to automatically determine how to call fixture and test functions by inspecting their parameters at runtime

> But I think it would be easiest to discuss this based on a real world example. Do you have any? Do you know if it is a common pattern to have boolean fixtures?

[the real world example i encountered](https://github.com/DetachHead/pytest-robotframework/blob/e9984db408922828f6e12498b78787426fd77a96/pytest_robotframework/hooks.py#L47-L54) was related to hook functions rather than fixtures, but pytest behaves the same with them as well:

```py
@hookspec
def pytest_robot_assertion(
    item: Item,
    expression: str,
    fail_message: object,
    line_number: int,
    assertion_error: AssertionError | None,
    explanation: str,
) -> None:
    ...
```

this hook currently triggers `PLR0917`. i used to have a boolean argument there that triggered `FBT001` as well but that's since been removed. my issue with changing these arguments to be keyword only is that it's not consistent with [how all other pytest hooks are defined](https://docs.pytest.org/en/stable/how-to/writing_hook_functions.html#declaring-new-hooks). note that although it does say "Hooks receive parameters using only keyword arguments." the examples don't define the arguments as keyword only. i've worked with many pytest plugins (including pytest's own ones) and none of them seem to do this either. it would cause unnecessary confusion for users who are used to writing hooks the normal way. specifically, the user would encounter this unhelpful error message if they forgot to make the arguments keyword only in their `pytest_robot_assertion` hook:

```
pluggy._manager.PluginValidationError: Plugin 'robotframework' for hook 'pytest_robot_assertion'
hookimpl definition: pytest_robot_assertion(item: 'Item', expression: 'str', fail_message: 'object', explanation: 'str', assertion_error: 'AssertionError | None')
Argument(s) {'fail_message', 'expression', 'assertion_error', 'explanation', 'item'} are declared in the hookimpl but can not be found in the hookspec
```

imo since neither fixture, test or hook functions should ever be called manually and are only supposed to be called internally by pytest itself, it doesn't make sense to report these errors on them, since these rules are specifically designed for functions that are intended to be called by a user.

---

_Comment by @MichaReiser on 2025-01-04 12:02_

I agree that it makes sense to exclude pytest hooks for PLR0917 and FBT001 (if there are any) because the user does not control the method signatures. 

I would exclude changing fixtures' behavior because a function with too many fixture arguments is harder to understand than one with fewer (so PLR0917 applies). 

---

_Comment by @DetachHead on 2025-01-05 09:57_

`PLR0917` is specifically for positional arguments though, and i don't see how that makes the function harder to understand. if the user never calls it manually anyway, it makes no difference whether the arguments are positional or keyword only

---

_Comment by @MichaReiser on 2025-01-05 10:03_

I think we can reconsider it when we have real world use cases where this is indeed a problem. I still think that e.g. having a test with 5+ fixtures can be split into either different tests or by refactoring fixtures. 

Anyway. Let's focus on what we have agreement on and change `PLR0917` to ignore functions that match known pytest hooks by name. We can revisit fixtures separately if they're common enough to justify special handling. 

---
