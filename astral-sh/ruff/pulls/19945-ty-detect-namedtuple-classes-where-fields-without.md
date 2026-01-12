```yaml
number: 19945
title: "[ty] Detect `NamedTuple` classes where fields without default values follow fields with default values"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/namedtuple-default-values
created_at: 2025-08-16T22:40:54Z
updated_at: 2025-08-19T08:56:35Z
url: https://github.com/astral-sh/ruff/pull/19945
synced_at: 2026-01-12T15:56:51Z
```

# [ty] Detect `NamedTuple` classes where fields without default values follow fields with default values

---

_@AlexWaygood_

(Stacked on top of https://github.com/astral-sh/ruff/pull/19943 and https://github.com/astral-sh/ruff/pull/19944; review them first.)

## Summary

This raises a `TypeError` at runtime, and the typing spec states that type checkers must detect this in order to be compliant:

```pytb
>>> from typing import NamedTuple
>>> class Foo(NamedTuple):
...     x: int = 0
...     y: str
...     
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    class Foo(NamedTuple):
        x: int = 0
        y: str
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 3007, in __new__
    raise TypeError(f"Non-default namedtuple field {field_name} "
    ...<2 lines>...
                    f"{', '.join(default_names)}")
TypeError: Non-default namedtuple field y cannot follow default field x
```

Closes https://github.com/astral-sh/ty/issues/545. After this PR has landed, the only significant remaining piece of work for `NamedTuple` support is the functional syntax for defining a `NamedTuple` class. I don't think it's essential that we get that landed before the beta, and I'd also prefer to hold off until the in-progress work on `NewType` has landed, since I think some of the refactors being done for that will make supporting functional syntax for `NamedTuple`s easier.

## Test Plan

Mdtests/snapshots.


---

_Label `ty` added by @AlexWaygood on 2025-08-16 22:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-16 22:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-16 22:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-16 22:40_

---

_Comment by @github-actions[bot] on 2025-08-16 22:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-19 08:54:25.988519402 +0000
+++ new-output.txt	2025-08-19 08:54:28.460538226 +0000
@@ -668,6 +668,7 @@
 namedtuples_define_class.py:47:18: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Literal[3]`
 namedtuples_define_class.py:48:22: error[too-many-positional-arguments] Too many positional arguments: expected 4, got 5
 namedtuples_define_class.py:49:23: error[unknown-argument] Argument `other` does not match any known parameter
+namedtuples_define_class.py:59:5: error[invalid-named-tuple] NamedTuple field without default value cannot follow field(s) with default value(s): Field `latitude` defined here without a default value
 namedtuples_define_class.py:94:1: error[type-assertion-failure] Argument does not have asserted type `Property[int | float]`
 namedtuples_define_class.py:95:1: error[type-assertion-failure] Argument does not have asserted type `int | float`
 namedtuples_define_class.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int | float`
@@ -859,5 +860,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 860 diagnostics
+Found 861 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @AlexWaygood on 2025-08-16 22:44_

The new diagnostic on the typing conformance suite (https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/namedtuples_define_class.py#L52-L59) is correct!

---

_Comment by @github-actions[bot] on 2025-08-16 22:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @MichaReiser by @AlexWaygood on 2025-08-16 22:45_

---

_Comment by @AlexWaygood on 2025-08-17 18:02_

Hmm, I just realised that there's a considerable overlap here with #19825. The only differences really are:
- Dataclasses are more complicated due to the `KW_ONLY` sentinel, the `kw_only=True` class decorator argument, and the `kw_only=True` field argument.
- I've been a bit fancier here with trying to display subdiagnostics to point to the previous field that had a default value. We could probably do similar with the dataclasses diagnostic, though it's not essential.

#19825 probably deserves to be merged first given it's been open longer! Whichever one gets merged first, I'm happy to look into combining the implementations to reduce the extent to which logic is duplicated between two locations.

---

_@carljm approved on 2025-08-19 01:29_

Nice!

---

_Comment by @carljm on 2025-08-19 01:30_

I just looked at #19825 and I don't think it's quite ready yet, so I think you can go ahead with this; we can revisit to combine the implementations once they are both landed.

---

_Review request for @sharkdp removed by @sharkdp on 2025-08-19 08:36_

---

_Merged by @AlexWaygood on 2025-08-19 08:56_

---

_Closed by @AlexWaygood on 2025-08-19 08:56_

---

_Branch deleted on 2025-08-19 08:56_

---
