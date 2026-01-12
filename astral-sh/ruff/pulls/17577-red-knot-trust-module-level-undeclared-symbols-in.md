```yaml
number: 17577
title: "[red-knot] Trust module-level undeclared symbols in stubs"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/trust-undeclared-symbols-in-stubs
created_at: 2025-04-23T08:17:01Z
updated_at: 2025-04-23T17:56:26Z
url: https://github.com/astral-sh/ruff/pull/17577
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Trust module-level undeclared symbols in stubs

---

_@sharkdp_

## Summary

Many symbols in typeshed are defined without being declared. For example:
```pyi
# builtins:
IOError = OSError

# types
LambdaType = FunctionType
NotImplementedType = _NotImplementedType

# typing
Text = str

# random
uniform = _inst.uniform

# optparse
make_option = Option

# all over the place:
_T = TypeVar("_T")
```

Here, we introduce a change that skips widening the public type of these symbols (by unioning with `Unknown`).

fixes #17032

## Ecosystem analysis

This is difficult to analyze in detail, but I went over most changes and it looks very favorable to me overall. The diff on the overall numbers is:
```
errors: 1287 -> 859 (reduction by 428)
warnings: 45 -> 59 (increase by 14)
```

### Removed false positives

`invalid-base` examples:

```diff
- error[lint:invalid-base] /tmp/mypy_primer/projects/pip/src/pip/_vendor/rich/console.py:548:27: Invalid class base with type `Unknown | Literal[_local]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/tornado/tornado/iostream.py:84:25: Invalid class base with type `Unknown | Literal[OSError]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/mitmproxy/test/conftest.py:35:40: Invalid class base with type `Unknown | Literal[_UnixDefaultEventLoopPolicy]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
```

`invalid-exception-caught` examples:

```diff
- error[lint:invalid-exception-caught] /tmp/mypy_primer/projects/cloud-init/cloudinit/cmd/status.py:334:16: Cannot catch object of type `Literal[ProcessExecutionError]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[lint:invalid-exception-caught] /tmp/mypy_primer/projects/jinja/src/jinja2/loaders.py:537:16: Cannot catch object of type `Literal[TemplateNotFound]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
```

`unresolved-reference` examples

https://github.com/canonical/cloud-init/blob/7a0265d36e01e649f72005548f17dca9ac0150ad/cloudinit/handlers/jinja_template.py#L120-L123 (we now understand the `isinstance` narrowing)

```diff
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/cloud-init/cloudinit/handlers/jinja_template.py:123:16: Type `Exception` has no attribute `errno`
```

`unknown-argument` examples

https://github.com/hauntsaninja/boostedblob/blob/master/boostedblob/request.py#L53

```diff
- error[lint:unknown-argument] /tmp/mypy_primer/projects/boostedblob/boostedblob/request.py:53:17: Argument `connect` does not match any known parameter of bound method `__init__`
```

`unknown-argument`

There are a lot of `__init__`-related changes because we now understand [`@attr.s`](https://github.com/python-attrs/attrs/blob/3d42a6978ac60b487135db39218cfb742b100899/src/attr/__init__.pyi#L387) as a `@dataclass_transform` annotated symbol. For example:

```diff
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:72:18: Argument `x` does not match any known parameter of bound method `__init__`
```

### New false positives

This can happen if a symbol that previously was inferred as `X | Unknown` was assigned-to, but we don't yet understand the assignability to `X`:

https://github.com/strawberry-graphql/strawberry/blob/main/strawberry/exceptions/handler.py#L90

```diff
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/strawberry/strawberry/exceptions/handler.py:90:9: Object of type `def strawberry_threading_exception_handler(args: tuple[type[BaseException], BaseException | None, TracebackType | None, Thread | None]) -> None` is not assignable to attribute `excepthook` of type `(_ExceptHookArgs, /) -> Any`
```

### New true positives

https://github.com/DataDog/dd-trace-py/blob/6bbb5519fe4b3964f9ca73b21cf35df8387618b2/tests/tracer/test_span.py#L714

```diff
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/tracer/test_span.py:714:33: Argument to this function is incorrect: Expected `str`, found `Literal[b"\xf0\x9f\xa4\x94"]`
```

### Changed diagnostics

A lot of changed diagnostics because we now show `@Todo(Support for `typing.TypeVar` instances in type expressions)` instead of `Unknown` for all kinds of symbols that used a `_T = TypeVar("_T")` as a type. One prominent example is the `list.__getitem__` method:

`builtins.pyi`:
```pyi
_T = TypeVar("_T")  # previously `TypeVar | Unknown`, now just `TypeVar`

# …

class list(MutableSequence[_T]):
    # …
    @overload
    def __getitem__(self, i: SupportsIndex, /) -> _T: ...
    # …
```

which causes this change in diagnostics:
```py
xs = [1, 2]
reveal_type(xs[0])  # previously `Unknown`, now `@Todo(Support for `typing.TypeVar` instances in type expressions)`
```

## Test Plan

Updated Markdown tests


---

_Label `red-knot` added by @sharkdp on 2025-04-23 08:17_

---

_Comment by @github-actions[bot] on 2025-04-23 08:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/recipes.py:98:27: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[zip]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/recipes.py:98:27: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[zip]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/recipes.py:897:22: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[slice]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/recipes.py:897:22: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[slice]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/more.py:2441:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | Literal[bool]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/more.py:2441:38: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Unknown | Literal[bool]`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/more.py:2997:44: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[repeat]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/more-itertools/more_itertools/more.py:2997:44: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[repeat]`

anyio (https://github.com/agronholm/anyio)
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:625:43: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def readlink(path: @Todo(Support for `typing.TypeAlias`), *, dir_fd: int | None = None) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_core/_fileio.py:625:43: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def readlink(path: @Todo(Support for `typing.TypeAlias`), *, dir_fd: int | None = None) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`

attrs (https://github.com/python-attrs/attrs)
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:70:15: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:70:22: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:72:18: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:72:23: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:88:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:104:52: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:120:25: Argument `public` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:120:35: Argument `_private` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:120:47: Argument `__dunder__` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:121:13: Too many positional arguments to bound method `__init__`: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:137:50: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:154:50: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:202:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:262:13: Argument `a` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:262:21: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:263:13: Argument `b` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:263:22: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:264:13: Argument `c` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:264:30: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:265:13: Argument `d` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:296:23: Argument `a` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:296:31: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:296:35: Argument `b` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:296:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:296:49: Argument `c` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:296:66: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_converters.py:131:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_converters.py:131:15: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_converters.py:291:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_converters.py:291:15: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_converters.py:339:16: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_converters.py:339:16: Too many positional arguments: expected 0, got 1
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:70:15: Argument to this function is incorrect: Expected `int`, found `Literal["3"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:70:22: Argument to this function is incorrect: Expected `int | float`, found `Literal["3.14"]`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:88:42: No argument provided for required parameter `y`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:88:44: Argument to this function is incorrect: Expected `int`, found `float`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:21:7: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:104:55: Too many positional arguments: expected 1, got 2
+ error[lint:missing-argument] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:202:40: No argument provided for required parameter `y`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:22:3: Argument `a` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:48:9: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:49:4: Argument `a` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:83:4: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:83:11: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:91:4: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:91:11: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:91:17: Argument `z` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:145:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:146:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:175:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:176:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:177:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:178:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:179:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:180:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:181:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:386:17: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:262:21: Argument to this function is incorrect: Expected `datetime`, found `Literal[1]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:263:22: Argument to this function is incorrect: Expected `datetime`, found `Literal[2]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_hooks.py:264:30: Argument to this function is incorrect: Expected `datetime`, found `Literal[3]`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:21:7: Too many positional arguments: expected 0, got 1
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:22:3: Argument `a` does not match any known parameter
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:177:13: Argument to this function is incorrect: Expected `int`, found `Literal["on"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:178:13: Argument to this function is incorrect: Expected `int`, found `Literal["yes"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/typing_example.py:181:13: Argument to this function is incorrect: Expected `int`, found `Literal["n"]`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:170:18: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:172:22: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:174:22: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:176:22: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:187:21: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:199:25: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:221:22: Too many positional arguments: expected 0, got 1
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((@Todo(Support for `typing.TypeVar` instances in type expressions), @Todo(Support for `typing.TypeVar` instances in type expressions), int, /) -> @Todo(specialized non-generic class) | None) | None`, found `() -> Unknown`
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:838:16: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:857:20: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:913:16: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:932:20: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:984:16: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:1003:20: Too many positional arguments: expected 0, got 1
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:182:38: Argument `x` does not match any known parameter
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:326:23: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:356:18: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:368:29: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:393:44: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:394:36: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:401:30: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:401:51: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:407:34: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:407:59: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:522:17: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:576:25: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:576:47: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:614:18: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:632:15: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:632:22: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:779:19: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:781:19: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:782:19: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:804:19: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:40:20: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:61:30: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:86:33: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:135:19: Too many positional arguments: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:169:15: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:184:15: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:244:28: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:281:31: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:330:11: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:337:39: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:346:37: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:428:15: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:116:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:missing-argument] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:452:13: No argument provided for required parameter `y`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:449:15: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:452:15: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:454:15: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:454:20: Argument `y` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_annotations.py:472:23: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:40:20: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:61:30: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:86:33: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:135:19: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:169:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:184:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:244:28: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:281:31: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:330:11: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:337:39: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:346:37: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_setattr.py:428:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:214:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:214:22: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:232:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:232:22: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:251:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:277:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:455:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:481:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:618:18: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:618:18: Too many positional arguments: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:618:35: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:618:35: Too many positional arguments: expected 0, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:702:22: Argument `a` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:702:22: Argument `a` does not match any known parameter
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:716:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:716:25: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:719:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:719:22: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:722:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:722:22: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:734:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:734:25: Too many positional arguments: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:751:22: Argument `param2` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:751:22: Argument `param2` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:752:22: Argument `param2` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:752:22: Argument `param2` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:754:22: Argument `param1` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:754:22: Argument `param1` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:756:21: Argument `param1` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:756:21: Argument `param1` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:756:33: Argument `param2` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:756:33: Argument `param2` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:775:22: Argument `param2` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:775:22: Argument `param2` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:778:22: Argument `param1` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:778:22: Argument `param1` does not match any known parameter
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:780:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_funcs.py:780:21: Too many positional arguments: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:182:38: Argument `x` does not match any known parameter of bound method `__init__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:326:23: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:356:18: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:368:29: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:393:44: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:394:36: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:401:30: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:401:51: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:407:34: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:407:59: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:522:17: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:632:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:632:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:779:19: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:781:19: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:782:19: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_functional.py:804:19: Too many positional arguments to bound method `__init__`: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:362:39: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:362:39: Too many positional arguments: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:431:19: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:677:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:677:15: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:734:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:734:25: Too many positional arguments: expected 0, got 1
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:1008:13: Attribute `__code__` on type `Unknown | (() -> None)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:1020:13: Attribute `__code__` on type `Unknown | (() -> None)` is possibly unbound
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/attrs/tests/test_dunders.py:1032:13: Type `() -> None` has no attribute `__code__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:170:18: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:172:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:174:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:176:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:187:21: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:199:25: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:221:22: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((Unknown, Unknown, int, /) -> @Todo(specialized non-generic class) | None) | None`, found `() -> Unknown`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:446:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:838:16: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:857:20: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:913:16: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:932:20: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:984:16: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_validators.py:1003:20: Too many positional arguments to bound method `__init__`: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:82:28: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:82:28: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:82:33: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:82:33: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:83:29: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:83:29: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:83:34: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:83:34: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:111:17: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:111:17: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:111:22: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:111:22: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:112:17: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:112:17: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:112:22: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:112:22: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:113:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:113:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:113:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:113:23: Argument `y` does not match any known parameter
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:116:12: Operator `>` is not supported for types `C1Slots` and `C1Slots`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:141:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:141:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:141:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:141:23: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:141:28: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:141:28: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:157:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:157:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:157:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:157:23: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:157:28: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:157:28: Argument `z` does not match any known parameter
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:159:12: Operator `>` is not supported for types `C2Slots` and `C2Slots`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:161:19: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:161:19: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:161:24: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:161:24: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:161:29: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:161:29: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:241:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:241:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:241:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:241:23: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:241:28: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:241:28: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:255:28: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:255:28: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:255:33: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:255:33: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:255:38: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:255:38: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:259:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:259:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:259:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:259:23: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:259:28: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:259:28: Argument `z` does not match any known parameter
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:260:12: Operator `>` is not supported for types `C2Slots` and `C2Slots`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:261:19: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:261:19: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:261:24: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:261:24: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:261:29: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:261:29: Argument `z` does not match any known parameter
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:292:18: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:292:18: Too many positional arguments: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:299:24: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:299:24: Too many positional arguments: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:327:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:327:15: Too many positional arguments: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:387:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:387:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:387:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:387:23: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:387:28: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:387:28: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:399:28: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:399:28: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:399:33: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:399:33: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:399:38: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:399:38: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:403:18: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:403:18: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:403:23: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:403:23: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:403:28: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:403:28: Argument `z` does not match any known parameter
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:404:12: Operator `>` is not supported for types `C2Slots` and `C2Slots`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:405:19: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:405:19: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:405:24: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:405:24: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:405:29: Argument `z` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:405:29: Argument `z` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:422:32: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:422:32: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:422:37: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:422:37: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:423:33: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:423:33: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:423:38: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:423:38: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:449:32: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:449:32: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:449:37: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:449:37: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:450:33: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:450:33: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:450:38: Argument `y` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:450:38: Argument `y` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:608:7: Argument `field` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:608:7: Argument `field` does not match any known parameter
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:628:22: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:628:22: Too many positional arguments: expected 0, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:662:41: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:662:41: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:662:53: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:662:53: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:696:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:696:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:697:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:697:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:698:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:698:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:699:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:699:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:721:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:721:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:722:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:722:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:738:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:738:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:806:11: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:806:11: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:841:11: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:841:11: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:867:11: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:867:11: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:890:11: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:890:11: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:920:11: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:920:11: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:944:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:944:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:945:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:945:14: Too many positional arguments: expected 0, got 1
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:976:13: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:976:13: Argument `x` does not match any known parameter
- error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:998:14: Argument `x` does not match any known parameter of bound method `__init__`
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:998:14: Argument `x` does not match any known parameter
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1020:14: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1020:14: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1041:14: Too many positional arguments to bound method `__init__`: expected 0, got 2
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1041:17: Too many positional arguments: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1061:7: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1061:7: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1081:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1081:13: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1100:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1100:15: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1101:15: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1101:15: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1124:13: Too many positional arguments to bound method `__init__`: expected 0, got 1
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1124:13: Too many positional arguments: expected 0, got 1
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1142:11: Too many positional arguments to bound method `__init__`: expected 0, got 3
+ error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/tests/test_slots.py:1142:11: Too many positional arguments: expected 0, got 3
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/attrs/te...*[Comment body truncated]*

---

_Renamed from "[red-knot] Trust undeclared symbols in stubs" to "[red-knot] Trust module-level undeclared symbols in stubs" by @sharkdp on 2025-04-23 09:51_

---

_Marked ready for review by @sharkdp on 2025-04-23 12:24_

---

_Review requested from @carljm by @sharkdp on 2025-04-23 12:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-23 12:24_

---

_Review requested from @dcreager by @sharkdp on 2025-04-23 12:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:596 on 2025-04-23 12:31_

why only at the module level? Typeshed does things like this as well, and I think there's an argument to be made that the public type for `DeprecatedNameForThing` should also not be unioned with `Unknown` here:

```py
class Foo:
    THING: Final = 42
    DeprecatedNameForThing = THING
```

it feels consistent with the idea behind this PR.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:598 on 2025-04-23 12:35_

nit: _type_ aliases are pretty consistently annotated with `: TypeAlias` in typeshed. But by annotating a symbol with `TypeAlias`, you're signalling to the type checker that the right-hand side of the assignment should be interpreted as a type expression and that the symbol should only be usable in contexts where type expressions are accepted. If the `IOError = OSError` assignment in typeshed were chnaged to `IOError: TypeAlias = OSError`, for example, type checkers might start disallowing uses of `IOError` in contexts where value expressions are expected, such as `X = IOError(42)`; that wouldn't be correct.

So arguably the issue being fixed here doesn't concern type aliases at all: it concerns all aliases that are _not_ type aliases!

---

_@AlexWaygood approved on 2025-04-23 12:37_

Yeah, this makes sense to me.

I do still think a lot of these assignments should probably be annotated with `Final` in typeshed anyway! This would benefit both us and other type checkers.

But your ecosystem analysis indicates that this is also a broader problem than just typeshed: `attrs` also appears to use bare assignments without declarations in its stub files, etc.

---

_@sharkdp reviewed on 2025-04-23 12:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:596 on 2025-04-23 12:50_

I didn't have this restriction initially, but then added it because Carl suggested this as a rule here: https://github.com/astral-sh/ruff/issues/17032#issuecomment-2762217959. What was your reasoning @carljm?

---

_@sharkdp reviewed on 2025-04-23 14:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:598 on 2025-04-23 14:13_

Thank you, I will adapt my explanation.

---

_@sharkdp reviewed on 2025-04-23 17:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:596 on 2025-04-23 17:31_

I'm going to do the following: I'm going to merge this as-is, and then re-open another PR with that restriction lifted. This should allow us to see the impact in a better way.

---

_Merged by @sharkdp on 2025-04-23 17:31_

---

_Closed by @sharkdp on 2025-04-23 17:31_

---

_Branch deleted on 2025-04-23 17:31_

---

_@AlexWaygood reviewed on 2025-04-23 17:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:596 on 2025-04-23 17:32_

Sounds good!

---

_@carljm reviewed on 2025-04-23 17:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:596 on 2025-04-23 17:56_

I do think the same rationale applies; I don't think I carefully thought through the class-scope case as even a thing that would happen in stub files.

---
