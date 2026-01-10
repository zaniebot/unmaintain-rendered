```yaml
number: 16597
title: "[red-knot] Add support for calling `type[…]`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/call-subclass-of-dynamic
created_at: 2025-03-10T10:34:08Z
updated_at: 2025-03-10T12:24:15Z
url: https://github.com/astral-sh/ruff/pull/16597
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Add support for calling `type[…]`

---

_Pull request opened by @sharkdp on 2025-03-10 10:34_

## Summary

This fixes the non-diagnostics part of https://github.com/astral-sh/ruff/issues/15948, but is mostly an experiment to see if the new ecosystem checks work as intended (see failing [mypy primer run here](https://github.com/astral-sh/ruff/actions/runs/13762732294/job/38482228022?pr=16597)). I saw a few false positives when expressions of type `type[<dynamic base>]` were called, so this seemed rather easy to fix.

## Test Plan

New Markdown tests.

Negative diff on the ecosystem checks:

```diff
zipp (https://github.com/jaraco/zipp)
- error: lint:call-non-callable
-    --> /tmp/mypy_primer/projects/zipp/zipp/__init__.py:393:16
-     |
- 392 |     def _next(self, at):
- 393 |         return self.__class__(self.root, at)
-     |                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Object of type `type[Unknown]` is not callable
- 394 |
- 395 |     def is_dir(self):
-     |
- 
- Found 9 diagnostics
+ Found 8 diagnostics

arrow (https://github.com/arrow-py/arrow)
+     |
+     |
+ warning: lint:unused-ignore-comment
+    --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:576:66
+ 574 |                 values.append(1)
+ 575 |
+ 576 |             floor = self.__class__(*values, tzinfo=self.tzinfo)  # type: ignore[misc]
+     |                                                                  -------------------- Unused blanket `type: ignore` directive
+ 577 |
+ 578 |             if frame_absolute == "week":
- error: lint:call-non-callable
-     --> /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1080:16
-      |
- 1078 |           dt = self._datetime.astimezone(tz)
- 1079 |
- 1080 |           return self.__class__(
-      |  ________________^
- 1081 | |             dt.year,
- 1082 | |             dt.month,
- 1083 | |             dt.day,
- 1084 | |             dt.hour,
- 1085 | |             dt.minute,
- 1086 | |             dt.second,
- 1087 | |             dt.microsecond,
- 1088 | |             dt.tzinfo,
- 1089 | |             fold=getattr(dt, "fold", 0),
- 1090 | |         )
-      | |_________^ Object of type `type[Unknown]` is not callable
- 1091 |
- 1092 |       # string output and formatting
-      |

black (https://github.com/psf/black)
- 
-     |
-     |
- error: lint:call-non-callable
-    --> /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/grammar.py:135:15
- 133 |         Copy the grammar.
- 134 |         """
- 135 |         new = self.__class__()
-     |               ^^^^^^^^^^^^^^^^ Object of type `type[@Todo]` is not callable
- 136 |         for dict_attr in (
- 137 |             "symbol2number",
- Found 328 diagnostics
+ Found 327 diagnostics
```

---

_Label `red-knot` added by @sharkdp on 2025-03-10 10:34_

---

_Renamed from "[red-knot] Add support for calling type[<dynamic base>]" to "[red-knot] Add support for calling `type[<dynamic base>]`" by @sharkdp on 2025-03-10 10:34_

---

_Marked ready for review by @sharkdp on 2025-03-10 10:52_

---

_Review requested from @carljm by @sharkdp on 2025-03-10 10:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-10 10:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-10 10:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2685 on 2025-03-10 11:27_

I think it's simpler just to fix all of #15948 at once? The behaviour we want to have when calling `type[Foo]` is simply to treat it exactly the same as calling `Literal[Foo]`

```diff
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index f47cdf360..9041af59e 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -2678,11 +2678,12 @@ impl<'db> Type<'db> {
                 )))
             }
 
-            Type::SubclassOf(subclass_of) if subclass_of.is_dynamic() => {
-                Ok(CallOutcome::Single(CallBinding::from_return_type(
-                    Type::Dynamic(subclass_of.into_dynamic().expect("checked above")),
-                )))
-            }
+            Type::SubclassOf(subclass_of_type) => match subclass_of_type.subclass_of() {
+                ClassBase::Dynamic(dynamic_type) => Ok(CallOutcome::Single(
+                    CallBinding::from_return_type(Type::Dynamic(dynamic_type)),
+                )),
+                ClassBase::Class(class) => Type::class_literal(class).try_call(db, arguments),
+            },
 
             instance_ty @ Type::Instance(_) => {
                 instance_ty
diff --git a/crates/red_knot_python_semantic/src/types/subclass_of.rs b/crates/red_knot_python_semantic/src/types/subclass_of.rs
index ade6bac8c..8d0873a96 100644
--- a/crates/red_knot_python_semantic/src/types/subclass_of.rs
+++ b/crates/red_knot_python_semantic/src/types/subclass_of.rs
@@ -1,5 +1,4 @@
 use crate::symbol::SymbolAndQualifiers;
-use crate::types::DynamicType;
 
 use super::{ClassBase, ClassLiteralType, Db, KnownClass, Type};
 
@@ -63,13 +62,6 @@ impl<'db> SubclassOfType<'db> {
         subclass_of.is_dynamic()
     }
 
-    pub fn into_dynamic(self) -> Option<DynamicType> {
-        match self.subclass_of {
-            ClassBase::Dynamic(dynamic) => Some(dynamic),
-            ClassBase::Class(_) => None,
-        }
-    }
-
     pub const fn is_fully_static(self) -> bool {
         !self.is_dynamic()
     }
```

---

_@AlexWaygood reviewed on 2025-03-10 11:27_

The diff in the mypy_primer run looks great! It's unfortunate that it shows up as a "failed check" on the PR, though -- is that deliberate?

---

_@sharkdp reviewed on 2025-03-10 12:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2685 on 2025-03-10 12:04_

Oh — Yes, thank you!

---

_@AlexWaygood reviewed on 2025-03-10 12:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2685 on 2025-03-10 12:07_

I guess this PR doesn't _quite_ close #15948, since there's still this bit to do from the issue description:

> This is not actually sound, because Liskov compatibility is not enforced on type constructor methods (`__init__` and `__new__`) when subclassing, so you can't be sure that a constructor call that works for a class `C` will actually work for a subclass of `C`. But it's widely relied on, and we will have to support it.
> 
> I would like to have an opt-in diagnostic emitted whenever you call an object of `type[C]`, so users who want to avoid this unsoundness have that option.

---

_Comment by @sharkdp on 2025-03-10 12:07_

> It's unfortunate that it shows up as a "failed check" on the PR, though -- is that deliberate?

Signalling a non-empty diff as a 'failure' (that you need to look at) seems fine to me? Once we add commenting on PRs, we could potentially turn that into a check that always succeeds.

---

_@AlexWaygood reviewed on 2025-03-10 12:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/subclass_of.md`:1 on 2025-03-10 12:07_

Could you also add a test or two for calling `type[<class>]`, now that we're doing that too?

---

_Renamed from "[red-knot] Add support for calling `type[<dynamic base>]`" to "[red-knot] Add support for calling `type[…]`" by @sharkdp on 2025-03-10 12:07_

---

_@sharkdp reviewed on 2025-03-10 12:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2685 on 2025-03-10 12:09_

Right, I just updated the PR description to say that as well :+1: 

---

_Comment by @AlexWaygood on 2025-03-10 12:10_

> > It's unfortunate that it shows up as a "failed check" on the PR, though -- is that deliberate?
> 
> Signalling a non-empty diff as a 'failure' (that you need to look at) seems fine to me? Once we add commenting on PRs, we could potentially turn that into a check that always succeeds.

hmm, I suspect this will be quite confusing for new contributors. Though having said that, contributors have also been confused over at typeshed by the mypy_primer comments. (A "sea of red" in the comment can be a good thing, since it shows that diagnostics are going away! But red usually connotes "bad" 😄)

---

_@sharkdp reviewed on 2025-03-10 12:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/subclass_of.md`:26 on 2025-03-10 12:14_

These will be fixed by https://github.com/astral-sh/ruff/pull/16512

---

_@AlexWaygood approved on 2025-03-10 12:17_

I think overall I'd much prefer a "comment on the PR" mypy_primer check rather than have the check fail. But the changes this PR makes look great!

---

_Comment by @sharkdp on 2025-03-10 12:22_

> I think overall I'd much prefer a "comment on the PR" mypy_primer check rather than have the check fail

Ok, I'll look into it.

---

_Merged by @sharkdp on 2025-03-10 12:24_

---

_Closed by @sharkdp on 2025-03-10 12:24_

---

_Branch deleted on 2025-03-10 12:24_

---
