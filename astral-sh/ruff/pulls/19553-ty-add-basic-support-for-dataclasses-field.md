```yaml
number: 19553
title: "[ty] Add basic support for `dataclasses.field`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/dataclasses-field
created_at: 2025-07-25T08:27:56Z
updated_at: 2025-07-25T12:56:06Z
url: https://github.com/astral-sh/ruff/pull/19553
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Add basic support for `dataclasses.field`

---

_@sharkdp_

## Summary

Add basic support for `dataclasses.field`:
* remove fields with `init=False` from the signature of the synthesized `__init__` method
* infer correct default value types from `default` or `default_factory` arguments

```py
from dataclasses import dataclass, field

def default_roles() -> list[str]:
    return ["user"]

@dataclass
class Member:
    name: str
    roles: list[str] = field(default_factory=default_roles)
    tag: str | None = field(default=None, init=False)

# revealed: (self: Member, name: str, roles: list[str] = list[str]) -> None
reveal_type(Member.__init__)
```

Support for `kw_only` has **not** been added.

part of https://github.com/astral-sh/ty/issues/111

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-07-25 08:27_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-25 08:28_

---

_Comment by @github-actions[bot] on 2025-07-25 08:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:3812:62: error[missing-argument] No argument provided for required parameter `CBuildsFn`
- Found 569 diagnostics
+ Found 568 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/console/exceptions.py:211:46: error[invalid-argument-type] Argument is incorrect: Expected `ConsoleMessage`, found `CalledProcessError`
- Found 932 diagnostics
+ Found 931 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/godbg.py:671:54: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `list[Unknown] | Unknown`
- pwndbg/aglib/godbg.py:686:50: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `list[Unknown]`
- pwndbg/aglib/godbg.py:707:60: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `list[Unknown] | Unknown`
- Found 2259 diagnostics
+ Found 2256 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cargo/interpreter.py:428:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, dict[str, Dependency]]`, found `str`
- Found 757 diagnostics
+ Found 756 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-25 08:37_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 5 | 0 |
| `missing-argument` | 0 | 1 | 0 |
| **Total** | **0** | **6** | **0** |

**[Full report with detailed diff](https://david-dataclasses-field.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-25 08:38_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-25 08:38_

---

_Renamed from "[ty] Add support for `dataclasses.field`" to "[ty] Add basic support for `dataclasses.field`" by @sharkdp on 2025-07-25 09:18_

---

_Marked ready for review by @sharkdp on 2025-07-25 09:24_

---

_Review requested from @carljm by @sharkdp on 2025-07-25 09:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-25 09:24_

---

_Review requested from @dcreager by @sharkdp on 2025-07-25 09:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:41 on 2025-07-25 11:13_

I think this is a slightly confusing signature for us to display in on-hover tooltips and the like. The reason why the `default_factory` parameter exists is to avoid this classic footgun in Python, where multiple instances of the same class end up inintentionally sharing the _same_ list as an instance attribute:

```pycon
>>> class Foo:
...     def __init__(self, x: list[int] = []) -> None:
...         self.x = x
...         
>>> f1 = Foo()
>>> f2 = Foo()
>>> f1.x.append(42)
>>> f1.x
[42]
>>> f2.x
[42]
```

The reason why this happens is because the default value of the `x` parameter is evaluated at class-creation time, not at method-call time, so if you don't pass an argument explicitly for the `x` parameter then an instance of `Foo` stores a reference to that list instance that was created at class-creation time for its `x` attribute.

This is typically not what you want, so the usual recommendation to work around this for a non-dataclass is to do something like this:

```pycon
>>> class Bar:
...     def __init__(self, x: list[int] | None = None) -> None:
...         self.x: list[int] = x or []
...         
>>> b1 = Bar()
>>> b2 = Bar()
>>> b1.x.append(42)
>>> b1.x
[42]
>>> b2.x
[]
```

And indeed, at runtime, we see that dataclasses uses a special sentinel as the default value for the synthesized `__init__` method, rather than a list:

```pycon
>>> from dataclasses import dataclass, field
>>> @dataclass
... class Baz:
...     x: list[int] = field(default_factory=list)
...     
>>> help(Baz.__init__)
Help on function __init__ in module __main__:

__init__(self, x: list[int] = <factory>) -> None
    Initialize self.  See help(type(self)) for accurate signature.
```

I think it would be great if we could show something similar in our synthesized signature, so that it's clear to users that the `__init__` method synthesized by the `dataclasses` module does not fall into the class trap of mutable default values. It's really very central to the design of the dataclasses module to try to guide users away from this footgun: if you attempt to naively provide a mutable default value for a field, an exception is raised that tells you to use `default_factory` instead:

```pycon
>>> from dataclasses import dataclass
>>> @dataclass
... class Spam:
...     x: list[int] = []
...     
Traceback (most recent call last):
  File "<python-input-22>", line 1, in <module>
    @dataclass
     ^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1305, in dataclass
    return wrap(cls)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1295, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
                          frozen, match_args, kw_only, slots,
                          weakref_slot)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 1008, in _process_class
    cls_fields.append(_get_field(cls, name, type, kw_only))
                      ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/dataclasses.py", line 860, in _get_field
    raise ValueError(f'mutable default {type(f.default)} for field '
                     f'{f.name} is not allowed: use default_factory')
ValueError: mutable default <class 'list'> for field x is not allowed: use default_factory
```

(Ideally we would catch this error and emit a diagnostic on it, but we'd need to be quite careful to avoid false positives. At runtime, any type is rejected if `__hash__` is set to `None` on the type, indicating that it is not hashable (and therefore probably mutable). But statically determining whether a type is hashable is surprisingly difficult, because unhashable types often have hashable subclasses in Python -- the Liskov principle is a poor fit for this area of the language.)

---

_@AlexWaygood approved on 2025-07-25 11:21_

---

_@sharkdp reviewed on 2025-07-25 12:44_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:41 on 2025-07-25 12:44_

> I think this is a slightly confusing signature for us to display in on-hover tooltips and the like. The reason why the `default_factory` parameter exists is to avoid this classic footgun in Python

Right. FWIW, I think there are also other very valid use cases for `default_factory`, like demonstrated in this example. Or when initialization of a field is expensive.

> I think it would be great if we could show something similar in our synthesized signature, so that it's clear to users that the `__init__` method synthesized by the `dataclasses` module does not fall into the class trap of mutable default values.

Hm. I agree that it would be concerning if we displayed something like `content: list[int] = []`, which rings alarm bells. But we display `content: list[int] = list[Unknown]`, i.e. a type and not a list literal. Other type checkers simply show `content: list[int] =` (mypy) or `content: list[int] = ...` (pyrefly) instead, but I like ty's output better. It's sometimes helpful to see the default value: `max_retries: int = Literal[3]`. pyright also shows `content: list[int] = list`, which seems very similar to what we do.

---

_@AlexWaygood reviewed on 2025-07-25 12:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:41 on 2025-07-25 12:53_

> But we display `content: list[int] = list[Unknown]`, i.e. a type and not a list literal.

Hm, fair point. I think this in general is pretty confusing TBH: at runtime, the default value is obviously an instance of list, not the type `list` itself. So why do we tell the user that the default value is the type `list` itself when they hover over a function? And we don't actually use the type of the default value for anything anywhere; we only ever care that the parameter _has_ a default value -- so why do we store the type of the default value on the `Signature` at all? _Users_ care about the default value, but they generally care about the _value_ of the default value, not the _type_ of the default value -- it would be much more useful if we could display the former (as it appears in the source code) rather than the latter, in the tooltip.

But none of that's relevant to your PR here! So I guess I withdraw my objection :-)

---

_@sharkdp reviewed on 2025-07-25 12:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/fields.md`:41 on 2025-07-25 12:55_

> but they generally care about the _value_ of the default value, not the _type_ of the default value -- it would be much more useful if we could display the former (as it appears in the source code) rather than the latter, in the tooltip.

Agree!

---

_Merged by @sharkdp on 2025-07-25 12:56_

---

_Closed by @sharkdp on 2025-07-25 12:56_

---

_Branch deleted on 2025-07-25 12:56_

---
