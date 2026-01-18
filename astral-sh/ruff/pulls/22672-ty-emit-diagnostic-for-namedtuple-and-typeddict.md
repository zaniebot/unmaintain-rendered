```yaml
number: 22672
title: "[ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/data-decorator
created_at: 2026-01-18T01:17:41Z
updated_at: 2026-01-18T13:09:26Z
url: https://github.com/astral-sh/ruff/pull/22672
synced_at: 2026-01-18T13:20:54Z
```

# [ty] Emit diagnostic for `NamedTuple` and `TypedDict` decorated with dataclass

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2515.

Closes https://github.com/astral-sh/ty/issues/2527.


---

_Label `ty` added by @AlexWaygood on 2026-01-18 01:19_

---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-18 01:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- Found 1822 diagnostics
+ Found 1821 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14497 diagnostics
+ Found 14496 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @carljm by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-18 01:45_

---

_Review requested from @MichaReiser by @charliermarsh on 2026-01-18 01:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2710 on 2026-01-18 11:58_

This method is called `is_named_tuple()`, but it doesn't just return `true` if `self` is a namedtuple class -- it also returns `true` if `self` inherits from a namedtuple class. I think this method should be called `has_named_tuple_class_in_mro()` or similar. 

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:595 on 2026-01-18 11:59_

(same comment here)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1471 on 2026-01-18 12:01_

I find the parenthetical here slightly confusing, because inheriting from `NamedTuple` just creates a special `tuple` subclass at runtime, and so neither a `NamedTuple` class nor a subclass of a `NamedTuple` class actually has `NamedTuple` in its MRO

```suggestion
Classes that inherit from a `NamedTuple` class also cannot be decorated with `@dataclass`:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:1480 on 2026-01-18 12:02_

maybe the error code should actually be `invalid-dataclass`? `Child` here inherits from a `NamedTuple` class, but it isn't a `NamedTuple` class itself.

We could possibly use `invalid-dataclass` for both the `NamedTuple` case and the `TypedDict` case? They both involve invalid applications of the `@dataclass` decorator

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:2382 on 2026-01-18 12:07_

I'm not sure it'll lead to an `AttributeError`, but it can definitely lead to other kinds of errors. Whether it does depends on exactly how you try to instantiate it:

```pycon
>>> from typing import TypedDict
>>> from dataclasses import dataclass
>>> @dataclass
... class Foo(TypedDict):
...     x: str
...     
>>> Foo(x='x')
{'x': 'x'}
>>> Foo('x')
Traceback (most recent call last):
  File "<python-input-6>", line 1, in <module>
    Foo('x')
    ~~~^^^^^
ValueError: dictionary update sequence element #0 has length 1; 2 is required
```

I think the better rationale when it comes to `TypedDict`s is possibly just that it's conceptually incoherent to apply `@dataclass` to a `TypedDict` class -- `TypedDict`s are abstract structural types that have no effect on the runtime class ("instantiating" a `TypedDict` always gives you a `dict` at runtime), whereas `@dataclass` is a tool for easily customising the creation of new nominal types/classes at runtime

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:701 on 2026-01-18 12:14_

you want to use the `header_range` of the class rather than the full range of the class here -- your diagnostics are currently very verbose if the `TypedDict` or `NamedTuple` class definition spans many lines of code 😄

<img width="2390" height="1426" alt="image" src="https://github.com/user-attachments/assets/0276d72e-6036-4ec3-81d7-4c29b8616f59" />


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:706 on 2026-01-18 12:15_

this is quite a long diagnostic message; I'd split the second half into a subdiagnostic

```suggestion
                    let mut diagnostic = builder.into_diagnostic(format_args!(
                        "Class `{}` inherits from `NamedTuple` and is decorated with `@dataclass`",
                        class.name(self.db()),
                    ));
                    diagnostic.info("An exception will be raised when instantiating the class at runtime");
```

and similar for the `TypedDict` case below

---

_@AlexWaygood reviewed on 2026-01-18 12:18_

Nice. I wonder if we should also flag enums and protocols decorated with `@dataclass`. Those also indicate that the user is probably very confused. And the enum docs explicitly lay out that adding `@dataclass` to an enum class is [not supported](https://docs.python.org/3/howto/enum.html#dataclass-support)

---
