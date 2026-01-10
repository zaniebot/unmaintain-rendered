```yaml
number: 679
title: pytest.skip(...) triggers call-non-callable
type: issue
state: closed
author: pawamoy
labels: []
assignees: []
created_at: 2025-06-18T14:37:24Z
updated_at: 2025-06-18T14:56:11Z
url: https://github.com/astral-sh/ty/issues/679
synced_at: 2026-01-10T02:08:20Z
```

# pytest.skip(...) triggers call-non-callable

---

_Issue opened by @pawamoy on 2025-06-18 14:37_

### Summary

pytest defines the `skip` function like this:

```python
_F = TypeVar("_F", bound=Callable[..., object])
_ET = TypeVar("_ET", bound=type[BaseException])


class _WithException(Protocol[_F, _ET]):
    Exception: _ET
    __call__: _F


def _with_exception(exception_type: _ET) -> Callable[[_F], _WithException[_F, _ET]]:
    def decorate(func: _F) -> _WithException[_F, _ET]:
        func_with_exception = cast(_WithException[_F, _ET], func)
        func_with_exception.Exception = exception_type
        return func_with_exception

    return decorate


@_with_exception(Skipped)
def skip(
    reason: str = "",
    *,
    allow_module_level: bool = False,
) -> NoReturn:
    __tracebackhide__ = True
    raise Skipped(msg=reason, allow_module_level=allow_module_level)
```

I looks like ty has trouble understanding that `_WithException.__call__` is a callable. I'm not sure this way of describing `__call__` in the protocol is officially supported by the typing spec though 🤔 

### Version

ty 0.0.1-alpha.11

---

_Comment by @AlexWaygood on 2025-06-18 14:40_

Thanks! This is the same underlying issue as #553

---

_Closed by @AlexWaygood on 2025-06-18 14:40_

---

_Comment by @pawamoy on 2025-06-18 14:48_

Oh thanks, and sorry for missing it! I searched for "pytest skip", got no result 😄 

---

_Comment by @AlexWaygood on 2025-06-18 14:56_

Great point -- `pytest.skip()` is probably the more commonly used decorator! I updated the title of the other issue to make it more discoverable 👍

---
