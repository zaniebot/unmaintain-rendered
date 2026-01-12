```yaml
number: 2026
title: Nested Pydantic schema causes slow performance
type: issue
state: closed
author: zsol
labels:
  - performance
assignees: []
created_at: 2025-12-17T18:07:56Z
updated_at: 2025-12-20T04:18:09Z
url: https://github.com/astral-sh/ty/issues/2026
synced_at: 2026-01-12T15:54:26Z
```

# Nested Pydantic schema causes slow performance

---

_@zsol_

### Summary

For example checking the following takes 11s, but the real world example this is taken from takes many minutes (and is a bit more nested):

```python
from pydantic_core import core_schema

core_schema.union_schema(
    [
        core_schema.is_instance_schema(...),
        core_schema.chain_schema(
            [
                core_schema.no_info_after_validator_function(
                    function=lambda data: data,
                    schema=core_schema.list_schema(
                        core_schema.list_schema(
                            core_schema.list_schema(),
                        )
                    ),
                ),
            ]
        ),
    ],
)
```

### Version

0.0.2 with #22030 applied

---

_Label `performance` added by @carljm on 2025-12-17 18:09_

---

_Added to milestone `Stable` by @carljm on 2025-12-17 18:10_

---

_Comment by @oconnor663 on 2025-12-17 18:13_

Yeah the performance of that specific `CoreSchema` union in Pydantic has been a known problem in https://github.com/astral-sh/ruff/pull/21467 and https://github.com/astral-sh/ruff/pull/21784. I'm working on `TypedDict` tagged union support right now, and the hope is that it'll help with this problem.

---

_Comment by @zanieb on 2025-12-19 01:22_

I just verified that https://github.com/astral-sh/ruff/pull/22052 and https://github.com/astral-sh/ruff/pull/22048 do not appear to help with this particular snippet. Here's the flamegraph (with those pull requests combined) https://share.firefox.dev/3KErPOX

---

_Comment by @zanieb on 2025-12-19 02:02_

The pattern seems to be

```
  infer_all_argument_types                                                                                                                                                                                           
    → infer_and_check_argument_types::{{closure}} (try_narrow)                                                                                                                                                       
      → infer_call_expression                                                                                                                                                                                        
        → infer_all_argument_types  (recursive)                                                                                                                                                                     
```

Claude claims

> When the type context is a union (52-element `CoreSchema`), it tries each union element via `try_narrow`. And for each attempt, nested call expressions also go through the same process. This gives `O(52^depth)` complexity.

---

_Comment by @zanieb on 2025-12-19 02:12_

I pursued various caching approaches before attempting to fix the root cause of the explosion.

I then had Claude look at a way to prevent the recursion and a patch like

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index ac6d092c08..5123f06cf0 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -7008,12 +7008,18 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         let db = self.db();
 
         // If the type context is a union, attempt to narrow to a specific element.
-        let narrow_targets: &[_] = match call_expression_tcx.annotation {
-            // TODO: We could theoretically attempt to narrow to every element of
-            // the power set of this union. However, this leads to an exponential
-            // explosion of inference attempts, and is rarely needed in practice.
-            Some(Type::Union(union)) => union.elements(db),
-            _ => &[],
+        // However, skip narrowing if we're already inside a multi-inference context
+        // (i.e., inside a nested call during union narrowing) to avoid exponential blowup.
+        let narrow_targets: &[_] = if self.context.is_in_multi_inference() {
+            &[]
+        } else {
+            match call_expression_tcx.annotation {
+                // TODO: We could theoretically attempt to narrow to every element of
+                // the power set of this union. However, this leads to an exponential
+                // explosion of inference attempts, and is rarely needed in practice.
+                Some(Type::Union(union)) => union.elements(db),
+                _ => &[],
+            }
         };
 
         // We silence diagnostics until we successfully narrow to a specific type.
```

completely resolves the issue.

```
  | Nesting | Baseline | With caching | With narrowing fix |                                                                                                                                                         
  |---------|----------|--------------|--------------------|                                                                                                                                                         
  | 2-level | 0.66s    | 0.12s        | 0.02s              |                                                                                                                                                         
  | 3-level | 13s      | 2.1s         | 0.02s              |                                                                                                                                                         
  | 4-level | >120s    | 40.5s        | 0.02s              |     
```

I don't feel qualified to say if this is appropriate.


---

_Comment by @zanieb on 2025-12-19 02:33_

I felt it was probably not appropriate, so I pursued avoiding redundant narrowing of repeated unions instead as an alternative. https://github.com/astral-sh/ruff/pull/22066 tracks ancestor unions to avoid recursive analysis. This still gives a 250x improvement on the 4-level snippet. I can get another 4x on top of that (i.e., 1000x) with various caching changes.

---

_Comment by @AlexWaygood on 2025-12-19 08:26_

I don't think we should pursue any micro-optimisations that only benefit pydantic (especially not ones that regress memory usage) until we see the performance impact of @oconnor663's upcoming work to implement tagged-union narrowing for unions of TypedDicts. (We know the pathological performance on pydantic is caused by the fact that they have a huge union of TypedDicts, and it currently never gets narrowed by us.) E.g. the reported pydantic speedup on https://github.com/astral-sh/ruff/pull/22066 is currently very impressive, but it's possible the number changes a lot after Jack's work has landed.

I continue to find https://github.com/astral-sh/ruff/pull/22065 interesting, however, since it's a very targeted change that also shows speedups on non-pydantic benchmarks, and actually shows a memory-usage _improvement_.

---

_Closed by @ibraheemdev on 2025-12-20 04:18_

---
