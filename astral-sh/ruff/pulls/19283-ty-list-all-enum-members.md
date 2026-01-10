```yaml
number: 19283
title: "[ty] List all `enum` members"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/enums
created_at: 2025-07-11T13:20:59Z
updated_at: 2025-07-14T11:18:19Z
url: https://github.com/astral-sh/ruff/pull/19283
synced_at: 2026-01-10T18:33:12Z
```

# [ty] List all `enum` members

---

_Pull request opened by @sharkdp on 2025-07-11 13:20_

## Summary

Adds a way to list all members of an `Enum` and implements almost all of the mechanisms by which members are distinguished from non-members ([spec](https://typing.python.org/en/latest/spec/enums.html#defining-members)). This has no effect on actual enums, so far.

If we're not comfortable merging this without having an actual impact, I can also just use this as a basis for further features.

## Test Plan

New Markdown tests using `ty_extensions.enum_members`.

---

_Review requested from @carljm by @sharkdp on 2025-07-11 13:21_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-11 13:21_

---

_Label `ty` added by @sharkdp on 2025-07-11 13:21_

---

_Review requested from @dcreager by @sharkdp on 2025-07-11 13:21_

---

_Review requested from @MichaReiser by @sharkdp on 2025-07-11 13:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/enums.rs`:11 on 2025-07-11 13:22_

This will certainly have a richer return type in the future. For now, just return a list of strings to power the `ty_extensions.enum_members` functionality.

---

_@sharkdp reviewed on 2025-07-11 13:22_

---

_@sharkdp reviewed on 2025-07-11 13:24_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/enums.rs`:29 on 2025-07-11 13:24_

This would benefit from a `Type::ListLiteral` variant. Alternatively, we could also look at the RHS of the definition and get the information from the AST directly. `_ignore_` is not used a lot in the ecosystem. And the list-variant of `_ignore_` seems to be used even less. So I don't think it's a priority for now.

---

_Comment by @github-actions[bot] on 2025-07-11 13:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:19 on 2025-07-11 13:28_

It's actually already `Color`! Which isn't _wrong_...

```suggestion
# TODO: `Literal[Color.RED]` would be more precise
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:40 on 2025-07-11 13:45_

if you had `enum_members` return a `set`, then this could be

```suggestion
# revealed: set[Literal["RED", "GREEN", "BLUE"]]
```

which might be slightly easier to read in a test suite?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:81 on 2025-07-11 13:46_

yes please!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:121 on 2025-07-11 13:46_

could also test `enum.property` and `types.DynamicClassAttribute`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:98 on 2025-07-11 13:51_

Remind me what exactly the rule is here? Is it that any callable object is not considered an enum member, or any descriptor object is not considered an enum member?

Here's a fun thing: whether or not `functools.partial` is considered an enum member or not appears to depend on which Python version you're on...

```pycon
Python 3.13.1 (main, Jan  3 2025, 12:04:03) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import enum
>>> import functools
>>> class Foo(enum.Enum):
...     X = lambda self: 42
...     Y = functools.partial(lambda self: 42)
...     
<python-input-2>:3: FutureWarning: functools.partial will be a method descriptor in future Python versions; wrap it in enum.member() if you want to preserve the old behavior
  Y = functools.partial(lambda self: 42)
>>> Foo.__members__
mappingproxy({})
>>> Foo._member_map_
{}
>>> list(Foo)
[]
>>> Foo.X
<function Foo.<lambda> at 0x100fe5f80>
>>> Foo.Y
functools.partial(<function Foo.<lambda> at 0x100fe5ee0>)
>>> exit
~/dev/ruff (alex/typealias-autocomplete)⚡ % uvx python3.8        
Python 3.8.20 (default, Oct  2 2024, 16:12:59) 
[Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import enum, functools
>>> class Foo(enum.Enum):
...     X = lambda self: 42
...     Y = functools.partial(lambda self: 42)
... 
>>> Foo.X
<function Foo.<lambda> at 0x102329160>
>>> Foo.Y
<Foo.Y: functools.partial(<function Foo.<lambda> at 0x10236c4c0>)>
>>> list(Foo)
[<Foo.Y: functools.partial(<function Foo.<lambda> at 0x10236c4c0>)>]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:224 on 2025-07-11 13:54_

You could possibly use the term "class-private" name here (which is what's used at https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers -- and you could maybe link to that page?)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:245 on 2025-07-11 14:00_

I think at runtime the string is just whitespace-split -- worth adding a test where they're separated by multiple spaces, and/or by tabs?

```pycon
>>> class Foo(Enum):
...     _ignore_ = "A         B"
...     A = 42
...     B = 56
...     C = 72
...     
>>> Foo.A
Traceback (most recent call last):
  File "<python-input-22>", line 1, in <module>
    Foo.A
AttributeError: type object 'Foo' has no attribute 'A'
>>> Foo.B
Traceback (most recent call last):
  File "<python-input-23>", line 1, in <module>
    Foo.B
AttributeError: type object 'Foo' has no attribute 'B'
>>> Foo.C
<Foo.C: 72>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:384 on 2025-07-11 14:03_

You could also add a test that demonstrates that e.g. `Color & int` is simplified to `Never` -- the finality of the enum class makes it disjoint from any class not already in its MRO. And maybe a test that demonstrates that an enum class which defines methods but no properties is _not_ final. E.g. `CoerciveEnum` here is used as a base class for all other enums in that file: https://github.com/py-pdf/fpdf2/blob/master/fpdf/enums.py

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/enums.rs`:29 on 2025-07-11 14:06_

We have a similar problem elsewhere where we do not currently understand the value of `__slots__` if it's a list of string literals in the source code, and we should

---

_@AlexWaygood approved on 2025-07-11 14:07_

---

_Label `testing` added by @AlexWaygood on 2025-07-11 14:07_

---

_@sharkdp reviewed on 2025-07-14 09:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:40 on 2025-07-14 09:25_

I think the order might not be irrelevant for things like `auto()`, so I'll probably use a tuple for now.

---

_@sharkdp reviewed on 2025-07-14 09:38_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:224 on 2025-07-14 09:38_

Class-private names seem to have the pattern `__*` though?

---

_@AlexWaygood reviewed on 2025-07-14 09:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/enums.md`:224 on 2025-07-14 09:41_

Yes! Nothing you said here is incorrect, I'm just suggesting that you use the "class-private" term from the language reference to describe the `__*` pattern (and possibly link to the reference as well), since `_*` names are also often referred to as "private" names in the Python context

---

_@sharkdp reviewed on 2025-07-14 10:22_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:224 on 2025-07-14 10:22_

Sorry, I was confused because I misread my own writing. Updated!

---

_@sharkdp reviewed on 2025-07-14 10:37_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/enums.md`:98 on 2025-07-14 10:37_

> Remind me what exactly the rule is here? Is it that any callable object is not considered an enum member, or any descriptor object is not considered an enum member?

Both. Thanks for asking!

The spec says: "Methods, callables, descriptors (including properties), and nested classes that are defined in the class are not treated as enum members". I was actually missing the "descriptors" part. Fixed and added a test, also restructured the tests so it's clear we test all of these variants.

---

_Merged by @sharkdp on 2025-07-14 11:18_

---

_Closed by @sharkdp on 2025-07-14 11:18_

---

_Branch deleted on 2025-07-14 11:18_

---
