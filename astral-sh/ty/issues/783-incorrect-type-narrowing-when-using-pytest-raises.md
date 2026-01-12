```yaml
number: 783
title: "incorrect type narrowing when using `pytest.raises`"
type: issue
state: closed
author: graipher
labels:
  - generics
assignees: []
created_at: 2025-07-08T15:42:06Z
updated_at: 2025-11-28T08:20:26Z
url: https://github.com/astral-sh/ty/issues/783
synced_at: 2026-01-12T15:54:24Z
```

# incorrect type narrowing when using `pytest.raises`

---

_@graipher_

### Summary

Consider the following example:

```python
import pytest
from typing import reveal_type


class FooError(Exception):
    foo: str


def foo():
    raise FooError("foo")


def test_foo():
    with pytest.raises(FooError) as e:
        reveal_type(e)
        foo()
    assert e.value.foo == "foo"
```

Both `mypy` and `pyright` will consider this OK:

```
$ uv run pyright
/tmp/foo/main.py
  /tmp/foo/main.py:15:21 - information: Type of "e" is "ExceptionInfo[FooError]"
0 errors, 0 warnings, 1 information

$ uv run mypy --strict .
main.py:15: note: Revealed type is "_pytest._code.code.ExceptionInfo[main.FooError]"
Success: no issues found in 1 source file
```

But `ty` finds an error:

```
$ uv run ty check .
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
info[revealed-type]: Revealed type
  --> main.py:15:21
   |
13 | def test_foo() -> None:
14 |     with pytest.raises(FooError) as e:
15 |         reveal_type(e)
   |                     ^ `ExceptionInfo[BaseException]`
16 |         foo()
17 |     assert e.value.foo == "foo"
   |

error[unresolved-attribute]: Type `BaseException` has no attribute `foo`
  --> main.py:17:12
   |
15 |         reveal_type(e)
16 |         foo()
17 |     assert e.value.foo == "foo"
   |            ^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default

Found 2 diagnostics
```

It seems like the type of `e` is narrowed to `BaseException` instead of the actual exception type `FooError`.

Here are the overloads of `pytest.raises`:

```python
@overload
def raises(
    expected_exception: type[E] | tuple[type[E], ...],
    *,
    match: str | re.Pattern[str] | None = ...,
    check: Callable[[E], bool] = ...,
) -> RaisesExc[E]: ...


@overload
def raises(
    *,
    match: str | re.Pattern[str],
    # If exception_type is not provided, check() must do any typechecks itself.
    check: Callable[[BaseException], bool] = ...,
) -> RaisesExc[BaseException]: ...


@overload
def raises(*, check: Callable[[BaseException], bool]) -> RaisesExc[BaseException]: ...


@overload
def raises(
    expected_exception: type[E] | tuple[type[E], ...],
    func: Callable[..., Any],
    *args: Any,
    **kwargs: Any,
) -> ExceptionInfo[E]: ...


def raises(
    expected_exception: type[E] | tuple[type[E], ...] | None = None,
    *args: Any,
    **kwargs: Any,
) -> RaisesExc[BaseException] | ExceptionInfo[E]:
    """Actual implemenation"""
```

Naively, I would expect to be in the first or fourth case here, where the type of the input exception should be part of the return type.

### Version

ty 0.0.1-alpha.14

---

_Label `generics` added by @carljm on 2025-07-21 17:37_

---

_Comment by @carljm on 2025-07-21 17:38_

Thanks for the report! Without having dug into it deeply, I suspect this is a limitation of our current typevar solver, which we are currently working on improving. CC @dcreager, this may be a useful test case.

---

_Comment by @mthuurne on 2025-08-03 01:31_

I made [a more minimal testcase that doesn't import from pytest](https://play.ty.dev/5fb78393-dc77-4183-8b0c-d4e5f015727f).

---

_Comment by @AlexWaygood on 2025-08-04 10:49_

> I made [a more minimal testcase that doesn't import from pytest](https://play.ty.dev/5fb78393-dc77-4183-8b0c-d4e5f015727f).

Thank you very much! That makes it look very much like this is the same issue as https://github.com/astral-sh/ty/issues/501. I'm inclined to leave this open, however, as users will probably search for `pytest.raises` in the issue tracker if they run into this issue, and this issue has `pytest.raises` in the title

---

_Comment by @lengau on 2025-09-05 22:09_

A workaround that's probably fine when using pytest but maybe not in production code is to use `assert isinstance` to narrow the type:

```python
import pytest
from typing import reveal_type


class FooError(Exception):
    foo: str


def foo():
    raise FooError("foo")


def test_foo():
    with pytest.raises(FooError) as e:
        reveal_type(e)
        foo()
	
	assert isinstance(e.value, FooError)
	reveal_type(e)
    assert e.value.foo == "foo"
```

---

_Added to milestone `Stable` by @carljm on 2025-11-14 17:07_

---

_Comment by @MatthijsKok on 2025-11-26 19:34_

Quick report that this bug is still present in `ty 0.0.1-alpha.28 (8c342496a 2025-11-25)`

---

_Closed by @sharkdp on 2025-11-28 08:20_

---
