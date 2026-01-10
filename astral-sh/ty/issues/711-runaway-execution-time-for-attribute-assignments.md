```yaml
number: 711
title: Runaway execution time for attribute assignments with cyclic reachability constraints
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - hang
assignees: []
created_at: 2025-06-26T13:35:08Z
updated_at: 2025-07-01T12:38:38Z
url: https://github.com/astral-sh/ty/issues/711
synced_at: 2026-01-10T02:07:36Z
```

# Runaway execution time for attribute assignments with cyclic reachability constraints

---

_Issue opened by @sharkdp on 2025-06-26 13:35_

The following example causes the `ty` run time to grow exponentially with the number of `isinstance` checks. This is very similar to the problematic case in https://github.com/astral-sh/ruff/pull/18669, with the only difference that the attributes are actually defined, circumventing the patch in https://github.com/astral-sh/ruff/pull/18669.


```py
class C:
    def f(self: "C"):
        self.a = ""
        self.b = ""
    
        if isinstance(self.a, str):
            return
    
        if isinstance(self.b, str):
            return
        if isinstance(self.b, str):
            return
        # [repeat >10 times]
```


For some reason, this scales even worse than the previous example. We end up doing `16 × 3^N - 38` `member_lookup_with_policy` calls, where `N` is the number of `isinstance(self.b, str)` checks. The previous case "only" scaled with `2^N`.


<details>

<summary>patch with a micro benchmark</summary>


```diff
diff --git a/crates/ruff_benchmark/benches/ty.rs b/crates/ruff_benchmark/benches/ty.rs
index 3093bd6b7a..a4d9ed2536 100644
--- a/crates/ruff_benchmark/benches/ty.rs
+++ b/crates/ruff_benchmark/benches/ty.rs
@@ -348,10 +348,10 @@ fn benchmark_many_tuple_assignments(criterion: &mut Criterion) {
     });
 }
 
-fn benchmark_many_attribute_assignments(criterion: &mut Criterion) {
+fn benchmark_complex_constrained_attributes_1(criterion: &mut Criterion) {
     setup_rayon();
 
-    criterion.bench_function("ty_micro[many_attribute_assignments]", |b| {
+    criterion.bench_function("ty_micro[complex_constrained_attributes_1]", |b| {
         b.iter_batched_ref(
             || {
                 // This is a regression benchmark for https://github.com/astral-sh/ty/issues/627.
@@ -400,6 +400,48 @@ fn benchmark_many_attribute_assignments(criterion: &mut Criterion) {
     });
 }
 
+fn benchmark_complex_constrained_attributes_2(criterion: &mut Criterion) {
+    setup_rayon();
+
+    criterion.bench_function("ty_micro[complex_constrained_attributes_2]", |b| {
+        b.iter_batched_ref(
+            || {
+                // This is is similar to the case above, but now the attributes are actually defined
+                setup_micro_case(
+                    r#"
+                    class C:
+                        def f(self: "C"):
+                            self.a = ""
+                            self.b = ""
+
+                            if isinstance(self.a, str):
+                                return
+
+                            if isinstance(self.b, str):
+                                return
+                            if isinstance(self.b, str):
+                                return
+                            if isinstance(self.b, str):
+                                return
+                            if isinstance(self.b, str):
+                                return
+                            if isinstance(self.b, str):
+                                return
+                            if isinstance(self.b, str):
+                                return
+                    "#,
+                )
+            },
+            |case| {
+                let Case { db, .. } = case;
+                let result = db.check();
+                assert_eq!(result.len(), 0);
+            },
+            BatchSize::SmallInput,
+        );
+    });
+}
+
 struct ProjectBenchmark<'a> {
     project: InstalledProject<'a>,
     fs: MemoryFileSystem,
@@ -535,7 +577,8 @@ criterion_group!(
     micro,
     benchmark_many_string_assignments,
     benchmark_many_tuple_assignments,
-    benchmark_many_attribute_assignments,
+    benchmark_complex_constrained_attributes_1,
+    benchmark_complex_constrained_attributes_2,
 );
 criterion_group!(project, anyio, attrs, hydra);
 criterion_main!(check_file, micro, project);
```

</details>

---

_Label `bug` added by @sharkdp on 2025-06-26 13:35_

---

_Label `hang` added by @sharkdp on 2025-06-26 13:35_

---

_Comment by @sharkdp on 2025-06-27 10:27_

https://github.com/astral-sh/ruff/pull/18955 fixes this, I only need to understand why.

---

_Comment by @sharkdp on 2025-07-01 09:59_

It's actually relatively clear why https://github.com/astral-sh/ruff/pull/18955 fixes this. What's harder to understand is why this was a problem in the first place.

Both of the attribute assignments previously acquired complex reachability constraints for an end-of-scope use (which is what we previously used for instance attributes).

With the change in https://github.com/astral-sh/ruff/pull/18955, we use all reachable definitions (`UseDefMapBuilder::reachable_definitions`), where both of these attribute assignments have trivial reachability constraints of `ALWAYS_TRUE`.

---

I have not attempted to do a full analysis here, but it's clear that there is some kind of combinatorial explosion here if we only look at the first few steps:

Let's say we want to look up `self.a`. To do this, we look at an end-of-scope use of `self.a` in `f`. The visibility of the `self.a = ""` binding at this position is guarded by a reachability constraint of
```
~[isinstance(self.a, str)] AND ~[isinstance(self.b, str)] AND … AND ~[isinstance(self.b, str)]
```
While evaluating this constraint, we need to infer the types `self.a` and N occurrences of `self.b`. The use of `self.a` here has a trivial reachability constraint (and would hit a cycle). But the uses of `self.b` each have different reachability constraints (with a growing number of `isinstance` checks). So we need to evaluate O(N) other reachability constraints to solve this:
```
~[isinstance(self.a, str)]
~[isinstance(self.a, str)] AND ~[isinstance(self.b, str)]
~[isinstance(self.a, str)] AND ~[isinstance(self.b, str)] AND ~[isinstance(self.b, str)] 

…

~[isinstance(self.a, str)] AND ~[isinstance(self.b, str)] AND … AND ~[isinstance(self.b, str)] 
```
Each of these have more uses of `self.a` and `self.b`, leading to further recursion. This expansion is eventually stopped by fixpoint iteration and salsa caching, but apparently there are many different variations of "the type of `self.b` at this particular position under these particular constraints in this specific iteration of fixpoint iteration".

If someone would find it valuable to track this down further, I'm happy to do it, but otherwise I would skip the full analysis for now.

---

_Closed by @sharkdp on 2025-07-01 12:38_

---
