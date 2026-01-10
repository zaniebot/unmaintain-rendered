```yaml
number: 18060
title: "[ty] Allow classes to inherit from `type[Any]` or `type[Unknown]`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/type-any-inheritance
created_at: 2025-05-13T00:06:01Z
updated_at: 2025-05-13T00:30:23Z
url: https://github.com/astral-sh/ruff/pull/18060
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Allow classes to inherit from `type[Any]` or `type[Unknown]`

---

_Pull request opened by @AlexWaygood on 2025-05-13 00:06_

## Summary

This continues our recent work to pay more respect to the gradual guarantee when inferring the MROs of classes. Classes are now permitted to inherit from `type[Any]` and `type[Unknown]`: since these are [actually intersection types](https://github.com/astral-sh/ty/issues/222), the same principle applies to these types as was established in https://github.com/astral-sh/ruff/pull/18055.

## Test Plan

mdtests added.

I wish I could tell you why the order of the diagnostics in the snapshot is changing as a result of these changes, but -- alas! -- I cannot. It doesn't seem like a massive problem, though.


---

_Review requested from @carljm by @AlexWaygood on 2025-05-13 00:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-13 00:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-13 00:06_

---

_Label `ty` added by @AlexWaygood on 2025-05-13 00:06_

---

_Comment by @github-actions[bot] on 2025-05-13 00:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
ignite (https://github.com/pytorch/ignite)
+ warning[unused-ignore-comment] ignite/distributed/auto.py:351:75: Unused blanket `type: ignore` directive
- Found 2276 diagnostics
+ Found 2277 diagnostics

operator (https://github.com/canonical/operator)
- error[invalid-base] ops/_private/harness.py:430:26: Invalid class base with type `type[Unknown]` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- Found 203 diagnostics
+ Found 202 diagnostics

mypy (https://github.com/python/mypy)
+ warning[unused-ignore-comment] mypy/stubtest.py:435:35: Unused blanket `type: ignore` directive
- Found 3371 diagnostics
+ Found 3372 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-super-argument] ddtrace/settings/_config.py:768:20: `type[Unknown]` is not a valid class
- error[invalid-super-argument] ddtrace/settings/_config.py:773:20: `type[Unknown]` is not a valid class
- Found 8604 diagnostics
+ Found 8602 diagnostics

scipy (https://github.com/scipy/scipy)
- error[invalid-super-argument] scipy/stats/_continuous_distns.py:68:20: `type[Unknown]` is not a valid class
- error[invalid-super-argument] scipy/stats/_continuous_distns.py:6765:20: `type[Unknown]` is not a valid class
- error[invalid-super-argument] scipy/stats/_continuous_distns.py:8568:20: `type[Unknown]` is not a valid class
- error[invalid-super-argument] scipy/stats/tests/test_distributions.py:295:21: `type[Unknown]` is not a valid class
- Found 7733 diagnostics
+ Found 7729 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-05-13 00:20_

That's quite interesting -- a lot of the primer reports are to do with `super()` rather than inheritance, as our `BoundSuper` type also users `ClassBase` as its inner data:

https://github.com/astral-sh/ruff/blob/41fa082414c7277082d7e19b307baea0a4771d4c/crates/ty_python_semantic/src/types.rs#L8100-L8106

I think the same arguments about the gradual guarantee apply to `super()` usage on `type[Any]` as they do to inheritance from `type[Any]`, though. So this doesn't seem like a problem to me.

(The hits in operator and mypy are both inheritance-related; not _all_ the primer hits are `super()`-related.)

---

_@carljm approved on 2025-05-13 00:26_

---

_Merged by @AlexWaygood on 2025-05-13 00:30_

---

_Closed by @AlexWaygood on 2025-05-13 00:30_

---

_Branch deleted on 2025-05-13 00:30_

---
