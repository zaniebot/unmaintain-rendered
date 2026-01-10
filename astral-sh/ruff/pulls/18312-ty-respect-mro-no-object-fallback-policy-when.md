```yaml
number: 18312
title: "[ty] Respect `MRO_NO_OBJECT_FALLBACK` policy when looking up symbols on `type` instances"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/no-object-fallback
created_at: 2025-05-26T08:11:51Z
updated_at: 2025-05-26T10:03:32Z
url: https://github.com/astral-sh/ruff/pull/18312
synced_at: 2026-01-10T18:51:02Z
```

# [ty] Respect `MRO_NO_OBJECT_FALLBACK` policy when looking up symbols on `type` instances

---

_Pull request opened by @sharkdp on 2025-05-26 08:11_

## Summary

This should address a problem that came up while working on https://github.com/astral-sh/ruff/pull/18280. When looking up an attribute (typically a dunder method) with the `MRO_NO_OBJECT_FALLBACK` policy, the attribute is first looked  up on the meta type. If the meta type happens to be `type`, we go through the following branch in `find_name_in_mro_with_policy`:

https://github.com/astral-sh/ruff/blob/97ff015c88354b5d45b52a9b1460c30978b5c29b/crates/ty_python_semantic/src/types.rs#L2565-L2573

The problem is that we now look up the attribute on `object` *directly* (instead of just having `object` in the MRO). In this case, `MRO_NO_OBJECT_FALLBACK` has no effect in `class_member_from_mro`:

https://github.com/astral-sh/ruff/blob/c3feb8ce27350a4c7a560671f4668dd47761260e/crates/ty_python_semantic/src/types/class.rs#L1081-L1082

So instead, we need to explicitly respect the `MRO_NO_OBJECT_FALLBACK` policy here by returning `Symbol::Unbound`.

## Test Plan

Added new Markdown tests that explain the ecosystem changes that we observe.

---

_Label `ty` added by @sharkdp on 2025-05-26 08:11_

---

_Renamed from "[ty] Respect MRO_NO_OBJECT_FALLBACK policy when looking up symbols on…" to "[ty] Respect `MRO_NO_OBJECT_FALLBACK` policy when looking up symbols on `type` instances" by @sharkdp on 2025-05-26 08:12_

---

_Closed by @sharkdp on 2025-05-26 08:24_

---

_Reopened by @sharkdp on 2025-05-26 08:24_

---

_Comment by @github-actions[bot] on 2025-05-26 08:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- error[unresolved-attribute] src/attr/_make.py:1183:9: Can not assign object of `str` to attribute `__qualname__` on type `(...) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] src/attr/_make.py:1183:9: Unresolved attribute `__qualname__` on type `(...) -> Unknown`.
- error[unresolved-attribute] src/attr/_make.py:1199:13: Can not assign object of `str` to attribute `__qualname__` on type `(...) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] src/attr/_make.py:1199:13: Unresolved attribute `__qualname__` on type `(...) -> Unknown`.

trio (https://github.com/python-trio/trio)
- error[unresolved-attribute] src/trio/_deprecate.py:137:5: Can not assign object of `str` to attribute `__qualname__` on type `(...) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] src/trio/_deprecate.py:137:5: Unresolved attribute `__qualname__` on type `(...) -> Unknown`.
- error[unresolved-attribute] src/trio/_deprecate.py:138:5: Can not assign object of `str` to attribute `__name__` on type `(...) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] src/trio/_deprecate.py:138:5: Unresolved attribute `__name__` on type `(...) -> Unknown`.
- error[unresolved-attribute] src/trio/_util.py:191:9: Can not assign object of `str` to attribute `__name__` on type `CallT` with custom `__setattr__` method.
+ error[unresolved-attribute] src/trio/_util.py:191:9: Unresolved attribute `__name__` on type `CallT`.
- error[unresolved-attribute] src/trio/_util.py:192:9: Can not assign object of `str` to attribute `__qualname__` on type `CallT` with custom `__setattr__` method.
+ error[unresolved-attribute] src/trio/_util.py:192:9: Unresolved attribute `__qualname__` on type `CallT`.

pwndbg (https://github.com/pwndbg/pwndbg)
- error[unresolved-attribute] pwndbg/wrappers/__init__.py:29:9: Can not assign object of `Unknown` to attribute `cmd` on type `(...) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] pwndbg/wrappers/__init__.py:29:9: Unresolved attribute `cmd` on type `(...) -> Unknown`.

static-frame (https://github.com/static-frame/static-frame)
- error[unresolved-attribute] static_frame/core/util.py:571:5: Can not assign object of `Any` to attribute `__name__` on type `(rhs, lhs, func=Any) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] static_frame/core/util.py:571:5: Unresolved attribute `__name__` on type `(rhs, lhs, func=Any) -> Unknown`.

cloud-init (https://github.com/canonical/cloud-init)
- error[unresolved-attribute] tests/unittests/sources/test_azure.py:2248:9: Can not assign object of `list[Unknown]` to attribute `return_value` on type `(...) -> Unknown` with custom `__setattr__` method.
+ error[unresolved-attribute] tests/unittests/sources/test_azure.py:2248:9: Unresolved attribute `return_value` on type `(...) -> Unknown`.

```
</details>


---

_Marked ready for review by @sharkdp on 2025-05-26 10:01_

---

_Review requested from @carljm by @sharkdp on 2025-05-26 10:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-26 10:01_

---

_Review requested from @dcreager by @sharkdp on 2025-05-26 10:01_

---

_Comment by @sharkdp on 2025-05-26 10:03_

I'm merging this without review to unblock some work on https://github.com/astral-sh/ruff/pull (I want to see the updated ecosystem results). It also seems relatively uncontroversial, but a post-merge review wouldn't hurt, probably.

---

_Merged by @sharkdp on 2025-05-26 10:03_

---

_Closed by @sharkdp on 2025-05-26 10:03_

---

_Branch deleted on 2025-05-26 10:03_

---
