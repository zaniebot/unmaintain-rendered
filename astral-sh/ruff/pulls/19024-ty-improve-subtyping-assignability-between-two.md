```yaml
number: 19024
title: "[ty] Improve subtyping/assignability between two protocol types"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/transitive-protocols
created_at: 2025-06-29T11:45:50Z
updated_at: 2025-10-24T16:25:04Z
url: https://github.com/astral-sh/ruff/pull/19024
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Improve subtyping/assignability between two protocol types

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/720. A protocol `T` is always a subtype of a protocol `U`, even if it does not explicitly contain all members present on `U`, iff `U` is equivalent to `object`.

This PR also starts taking into account the types of mutable attribute protocol members when doing subtype/assignability checks between protocol types. (The improvement here was implemented between all other kinds of types in #18847, but not between two types if they were _both_ protocols.)

## Test Plan

Mdtests updated and added


---

_Label `ty` added by @AlexWaygood on 2025-06-29 11:45_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-29 11:45_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-29 11:45_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-29 11:45_

---

_Comment by @github-actions[bot] on 2025-06-29 11:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~142MB

trio (https://github.com/python-trio/trio)
+ error[unresolved-attribute] src/trio/_deprecate.py:49:19: Type `<Protocol with members '__qualname__'>` has no attribute `__module__`
- Found 882 diagnostics
+ Found 883 diagnostics

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

strawberry (https://github.com/strawberry-graphql/strawberry)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- warning[redundant-cast] pymongo/asynchronous/cursor.py:987:47: Value is already of type `SupportsItems[Unknown, Unknown]`
- warning[redundant-cast] pymongo/synchronous/cursor.py:985:47: Value is already of type `SupportsItems[Unknown, Unknown]`
- Found 456 diagnostics
+ Found 454 diagnostics

ignite (https://github.com/pytorch/ignite)
- warning[unused-ignore-comment] ignite/engine/engine.py:934:35: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] ignite/handlers/time_profilers.py:99:65: Unused blanket `type: ignore` directive
- Found 2143 diagnostics
+ Found 2141 diagnostics

mkosi (https://github.com/systemd/mkosi)
+ error[invalid-argument-type] mkosi/config.py:1817:27: Argument to function `load` is incorrect: Expected `SupportsRead[str | bytes]`, found `(dict[str, Any] & <Protocol with members 'read'> & ~dict[Unknown, Unknown]) | (SupportsRead[str] & ~str & ~dict[Unknown, Unknown])`
+ error[invalid-argument-type] mkosi/config.py:2380:27: Argument to function `load` is incorrect: Expected `SupportsRead[str | bytes]`, found `(dict[str, Any] & <Protocol with members 'read'> & ~dict[Unknown, Unknown]) | (SupportsRead[str] & ~str & ~dict[Unknown, Unknown])`
- Found 130 diagnostics
+ Found 132 diagnostics

jinja (https://github.com/pallets/jinja)
+ error[call-non-callable] src/jinja2/async_utils.py:91:16: Object of type `object` is not callable
- warning[redundant-cast] src/jinja2/filters.py:142:17: Value is already of type `HasHTML`
- warning[redundant-cast] src/jinja2/filters.py:1050:17: Value is already of type `HasHTML`
- Found 199 diagnostics
+ Found 198 diagnostics
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB

vision (https://github.com/pytorch/vision)
-     memo fields = ~304MB
+     memo fields = ~276MB

schemathesis (https://github.com/schemathesis/schemathesis)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~156MB

werkzeug (https://github.com/pallets/werkzeug)
+ error[no-matching-overload] src/werkzeug/datastructures/file_storage.py:126:19: No overload of function `fspath` matches arguments
- warning[redundant-cast] src/werkzeug/utils.py:419:24: Value is already of type `PathLike[str] | str`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ error[no-matching-overload] tests/__init__.py:77:13: No overload of function `iter` matches arguments
- Found 2779 diagnostics
+ Found 2780 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- warning[unused-ignore-comment] static_frame/core/container_util.py:1582:83: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/index_base.py:480:62: Unused blanket `type: ignore` directive
+ error[invalid-argument-type] static_frame/core/index_datetime.py:150:25: Argument to function `len` is incorrect: Expected `Sized`, found `(Any & <Protocol with members '__len__'> & ~Index[Any] & ~str) | (Any & <Protocol with members '__len__'>) | (datetime64[date | int | None] & <Protocol with members '__len__'>)`
- warning[unused-ignore-comment] static_frame/core/util.py:3237:58: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/core/util.py:3357:56: Unused blanket `type: ignore` directive
- Found 1808 diagnostics
+ Found 1805 diagnostics
-     memo fields = ~304MB
+     memo fields = ~334MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ error[invalid-argument-type] sklearn/datasets/_samples_generator.py:1124:48: Argument to function `len` is incorrect: Expected `Sized`, found `(Unknown & <Protocol with members '__len__'>) | (float & <Protocol with members '__len__'>)`
- Found 2233 diagnostics
+ Found 2234 diagnostics

```
</details>


---

_Converted to draft by @AlexWaygood on 2025-06-29 11:53_

---

_Comment by @AlexWaygood on 2025-06-29 19:30_

Note to self: I think we have to synthesise the descriptor protocol when accessing instance attributes on synthesised protocols

---

_Comment by @AlexWaygood on 2025-09-12 19:27_

This is now hopelessly merge-conflicted, and I'd like to try a different approach on some details

---

_Closed by @AlexWaygood on 2025-09-12 19:27_

---

_Branch deleted on 2025-10-24 16:25_

---
