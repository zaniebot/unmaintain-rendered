```yaml
number: 18731
title: "[ty] Report when a dataclass contains more than one `KW_ONLY` field"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: ty-dataclasses-duplicate-kw-only
created_at: 2025-06-17T17:43:25Z
updated_at: 2025-06-20T02:42:31Z
url: https://github.com/astral-sh/ruff/pull/18731
synced_at: 2026-01-12T15:56:24Z
```

# [ty] Report when a dataclass contains more than one `KW_ONLY` field

---

_@InSyncWithFoo_

## Summary

Part of [#111](https://github.com/astral-sh/ty/issues/111).

After this change, dataclasses with two or more `KW_ONLY` field will be reported as invalid. The duplicate fields will simply be ignored when computing `__init__`'s signature.

## Test Plan

Markdown tests.


---

_Comment by @github-actions[bot] on 2025-06-17 17:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-06-17 17:52_

---

_Marked ready for review by @InSyncWithFoo on 2025-06-18 21:45_

---

_Review requested from @carljm by @InSyncWithFoo on 2025-06-18 21:45_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-06-18 21:45_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-06-18 21:45_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-06-18 21:45_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-06-18 21:45_

---

_Comment by @InSyncWithFoo on 2025-06-18 21:48_

Currently the diagnostic doesn't look quite as nice as I want it to be. I think it should show all offending fields in subdiagnostics, but `.fields()` doesn't contain AST information.

`.fields()` also deduplicates fields, so this is not reported, but I think it should:

```python
@dataclass
class C:
	a: KW_ONLY
	a: KW_ONLY  # Same name
```

---

_Comment by @AlexWaygood on 2025-06-18 22:11_

> `.fields()` also deduplicates fields, so this is not reported, but I think it should:

That doesn't cause a `TypeError` at runtime, so I think it should at least be a different rule if we want to complain about it. It feels more like a lint rule to me.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/dataclasses.md_-_Dataclasses_-_`dataclasses.KW_ONLY…_(dd1b8f2f71487f16).snap`:44 on 2025-06-18 22:59_

This is kind of useless noise in the snapshot -- can we separate the tests using `reveal_type` from the tests snapshotting diagnostics, or else import `reveal_type` for this test?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/dataclasses.md_-_Dataclasses_-_`dataclasses.KW_ONLY…_(dd1b8f2f71487f16).snap`:115 on 2025-06-18 23:01_

This seems a bit more verbose than necessary; why do we need to list the first one separately? Couldn't it just be "`KW_ONLY` fields: `b`, `d`"?

---

_@carljm approved on 2025-06-18 23:09_

Thanks!

---

_Review comment by @abhijeetbodas2001 on `crates/ty_python_semantic/src/types/infer.rs`:1128 on 2025-06-19 05:10_

This looks pretty similar to the block at:
https://github.com/astral-sh/ruff/blob/e352a50b74329268589cdc18eafab123832559ac/crates/ty_python_semantic/src/types/class.rs#L1320-L1322

@AlexWaygood I'm not sure this justifies a new method on `Type` (I can't imagine us needing this a third time). But if not a method, I feel at least having the style similar to the above block would be nice.

---

_@abhijeetbodas2001 reviewed on 2025-06-19 05:32_

Thanks! For me, this diff is also helpful as a tutorial on how to add a new diagnostic :)


---

_Comment by @abhijeetbodas2001 on 2025-06-19 05:35_

> It feels more like a lint rule to me.

Another idea for a lint rule here could be to recommend using `_` instead of an actual variable name for the `KW_ONLY` annotated field.

---

_@InSyncWithFoo reviewed on 2025-06-19 11:32_

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/snapshots/dataclasses.md_-_Dataclasses_-_`dataclasses.KW_ONLY…_(dd1b8f2f71487f16).snap`:115 on 2025-06-19 11:32_

Fixed, but, as stated above, I wanted these to be displayed in subdiagnotics.

---

_@AlexWaygood reviewed on 2025-06-19 11:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/dataclasses.md_-_Dataclasses_-_`dataclasses.KW_ONLY…_(dd1b8f2f71487f16).snap`:115 on 2025-06-19 11:50_

When we add the infra for goto-definition, it might make that a lot easier

---

_@AlexWaygood reviewed on 2025-06-19 12:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:1128 on 2025-06-19 12:24_

Eh, InSync's code feels fine to me here :-) it's nice not to use closures where we don't have to

---

_Merged by @carljm on 2025-06-20 02:42_

---

_Closed by @carljm on 2025-06-20 02:42_

---
