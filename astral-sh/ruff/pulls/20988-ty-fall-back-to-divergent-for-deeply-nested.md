```yaml
number: 20988
title: "[ty] Fall back to `Divergent` for deeply nested specializations"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1383
created_at: 2025-10-20T10:40:52Z
updated_at: 2025-10-22T12:29:13Z
url: https://github.com/astral-sh/ruff/pull/20988
synced_at: 2026-01-12T15:57:13Z
```

# [ty] Fall back to `Divergent` for deeply nested specializations

---

_@sharkdp_

## Summary

Fall back to `C[Divergent]` if we are trying to specialize `C[T]` with a type that itself already contains deeply nested specialized generic classes. This is a way to prevent infinite recursion for cases like `self.x = [self.x]` where type inference for the implicit instance attribute would not converge.

closes https://github.com/astral-sh/ty/issues/1383
closes https://github.com/astral-sh/ty/issues/837

## Test Plan

Regression tests.


---

_Label `ty` added by @sharkdp on 2025-10-20 10:40_

---

_Closed by @sharkdp on 2025-10-20 12:39_

---

_Reopened by @sharkdp on 2025-10-20 12:39_

---

_@sharkdp reviewed on 2025-10-20 13:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1623 on 2025-10-20 13:03_

This still needs to be fine-tuned (using ecosystem impact and benchmarks).

---

_@sharkdp reviewed on 2025-10-20 13:04_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-20 13:04_

I haven't understood the `scope` parameter of `Type::Divergent`. When we see an infinitely wrapped `list[list[list[…]]]` in a method of class `C`, is it still correct to set the scope parameter to the body scope of `list` (not of `C` or the method) here?

---

_Marked ready for review by @sharkdp on 2025-10-20 13:07_

---

_Review requested from @carljm by @sharkdp on 2025-10-20 13:07_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-20 13:07_

---

_Review requested from @dcreager by @sharkdp on 2025-10-20 13:07_

---

_Closed by @sharkdp on 2025-10-20 14:56_

---

_Reopened by @sharkdp on 2025-10-20 14:56_

---

_Comment by @github-actions[bot] on 2025-10-20 14:58_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-20 15:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@mtshiba reviewed on 2025-10-20 17:02_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-20 17:02_

Your use of `Divergent` in this PR seems to differ slightly from the original intended use. The intended use is to make functions with the potential for divergent type inference tracked, and to set the initial cycle value to `Divergent` instead of `Never`. If the resulting type contains `Divergent`, replace that type with `Divergent` itself to converge the inference.

https://github.com/astral-sh/ruff/pull/17371/files#diff-20b910c6e20faa962bb1642e111db1cbad8e66ace089bdd966ac9d7f9fa99ff2R649-R665

When I first introduced `Divergent` in #20312, I included `scope` to prevent `Divergent` from propagating to other types of inference. This was based on the use case in #17371.

https://github.com/astral-sh/ruff/pull/17371/commits/e3b896df046afbea1674c302617f44ac2ea9cda8#diff-fdebb876f3c3f16730728018e4b1223f844d9493cd7caa7ee94e4a7a7b02184f

`call_divergent` calls `divergent`, but it does not diverge itself. Knowing where the divergence occurs can help prevent excessive type replacement.

#20333 and #20359 may also be helpful in understanding `Divergent`.

---

_Closed by @sharkdp on 2025-10-20 17:45_

---

_Reopened by @sharkdp on 2025-10-20 17:45_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 07:50_

---

_Comment by @github-actions[bot] on 2025-10-21 07:56_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-fix-1383.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1383.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-21 08:02_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 08:02_

---

_Converted to draft by @sharkdp on 2025-10-21 08:50_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-21 10:10_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 10:10_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-21 10:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 10:25_

---

_Comment by @codspeed-hq[bot] on 2025-10-21 10:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1383?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #20988 will **degrade performances by 4.99%**

<sub>Comparing <code>david/fix-1383</code> (438706a) with <code>main</code> (2c94337)</sub>



### Summary

`❌ 2 (👁 2)` regressions  
`✅ 19` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1383?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 32.6 s | 34 s | -4.11% |
| 👁 | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1383?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 8.7 s | 9.2 s | -4.99% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1383?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @sharkdp on 2025-10-21 11:51_

---

_@sharkdp reviewed on 2025-10-21 14:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-21 14:59_

I currently don't see how that approach scales. We would need to add dedicated "turn this type into `Divergent` if it contains `Divergent`" logic everywhere. We can do this for tuples, but we also need to do it for sets, lists, and dicts. And it doesn't really stop with those syntactical constructs. What about something like
```py
from typing import TypeVar, Literal

T = TypeVar("T")

def make_tuple(a: T) -> tuple[T, Literal[1]]:
    return (a, 1)

class D:
    def f(self, other: "D"):
        self.x = make_tuple(other.x)

reveal_type(D().x)
```

Do we also specialize all call expressions to detect nested `Divergent` types? At that point, we could probably add it to `infer_expression_type` (and in fact, that might work, but done naively, we would get `Divergent` instead of `tuple[Divergent, …]` etc).

What I'm proposing here is certainly not very elegant, I agree. But it does solve all of these cases (and more) without any special casing to certain syntactical forms.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-21 15:18_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 15:18_

---

_@MichaReiser reviewed on 2025-10-21 16:12_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/visitor.rs`:377 on 2025-10-21 16:12_

Would that mean that we also fall back to `DIVERGENT` for cases where a user combines complex types, but without any fixpoint involved? 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/visitor.rs`:308 on 2025-10-21 16:13_

This is a very helpful comment.

---

_@MichaReiser reviewed on 2025-10-21 16:13_

---

_@AlexWaygood reviewed on 2025-10-21 16:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:360 on 2025-10-21 16:15_

nit: derive `Default`? then this could be

```suggestion
    let visitor = SpecializationDepthVisitor::default();
```

---

_@AlexWaygood reviewed on 2025-10-21 16:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:322 on 2025-10-21 16:16_

can you optimize and return early here, avoiding the `match` entirely, if `self.max_depth == usize::MAX`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:386 on 2025-10-21 16:22_

I'd be tempted to define some helpers to make this test more readable, e.g.

```diff
diff --git a/crates/ty_python_semantic/src/types/visitor.rs b/crates/ty_python_semantic/src/types/visitor.rs
index d3f848dbbd..9644c932c1 100644
--- a/crates/ty_python_semantic/src/types/visitor.rs
+++ b/crates/ty_python_semantic/src/types/visitor.rs
@@ -388,62 +388,45 @@ mod tests {
     fn test_generics_layering_depth() {
         let db = setup_db();
 
-        let list_of_int =
-            KnownClass::List.to_specialized_instance(&db, [KnownClass::Int.to_instance(&db)]);
+        let int = || KnownClass::Int.to_instance(&db);
+        let list = |element| KnownClass::List.to_specialized_instance(&db, [element]);
+        let dict = |key, value| KnownClass::Dict.to_specialized_instance(&db, [key, value]);
+        let set = |element| KnownClass::Set.to_specialized_instance(&db, [element]);
+        let str = || KnownClass::Str.to_instance(&db);
+        let bytes = || KnownClass::Bytes.to_instance(&db);
+        let union = |elements| UnionType::from_elements(&db, elements);
+
+        let list_of_int = list(int());
         assert_eq!(specialization_depth(&db, list_of_int), 1);
 
-        let list_of_list_of_int = KnownClass::List.to_specialized_instance(&db, [list_of_int]);
+        let list_of_list_of_int = list(list_of_int);
         assert_eq!(specialization_depth(&db, list_of_list_of_int), 2);
 
-        let list_of_list_of_list_of_int =
-            KnownClass::List.to_specialized_instance(&db, [list_of_list_of_int]);
+        let list_of_list_of_list_of_int = list(list_of_list_of_int);
         assert_eq!(specialization_depth(&db, list_of_list_of_list_of_int), 3);
 
-        let set_of_dict_of_str_and_list_of_int = KnownClass::Set.to_specialized_instance(
-            &db,
-            [KnownClass::Dict
-                .to_specialized_instance(&db, [KnownClass::Str.to_instance(&db), list_of_int])],
-        );
+        let set_of_dict_of_str_and_list_of_int = set(dict(str, list_of_int));
         assert_eq!(
             specialization_depth(&db, set_of_dict_of_str_and_list_of_int),
             3
         );
 
-        let union_type_1 =
-            UnionType::from_elements(&db, [list_of_list_of_list_of_int, list_of_list_of_int]);
+        let union_type_1 = union([list_of_list_of_list_of_int, list_of_list_of_int]);
         assert_eq!(specialization_depth(&db, union_type_1), 3);
 
-        let union_type_2 =
-            UnionType::from_elements(&db, [list_of_list_of_int, list_of_list_of_list_of_int]);
+        let union_type_2 = union([list_of_list_of_int, list_of_list_of_list_of_int]);
         assert_eq!(specialization_depth(&db, union_type_2), 3);
 
-        let tuple_of_tuple_of_int = Type::heterogeneous_tuple(
-            &db,
-            [Type::heterogeneous_tuple(
-                &db,
-                [KnownClass::Int.to_instance(&db)],
-            )],
-        );
+        let tuple_of_tuple_of_int =
+            Type::heterogeneous_tuple(&db, [Type::heterogeneous_tuple(&db, [int()])]);
         assert_eq!(specialization_depth(&db, tuple_of_tuple_of_int), 2);
 
-        let tuple_of_list_of_int_and_str = KnownClass::Tuple
-            .to_specialized_instance(&db, [list_of_int, KnownClass::Str.to_instance(&db)]);
+        let tuple_of_list_of_int_and_str =
+            KnownClass::Tuple.to_specialized_instance(&db, [list_of_int, str()]);
         assert_eq!(specialization_depth(&db, tuple_of_list_of_int_and_str), 1);
 
-        let list_of_union_of_lists = KnownClass::List.to_specialized_instance(
-            &db,
-            [UnionType::from_elements(
-                &db,
-                [
-                    KnownClass::List
-                        .to_specialized_instance(&db, [KnownClass::Int.to_instance(&db)]),
-                    KnownClass::List
-                        .to_specialized_instance(&db, [KnownClass::Str.to_instance(&db)]),
-                    KnownClass::List
-                        .to_specialized_instance(&db, [KnownClass::Bytes.to_instance(&db)]),
-                ],
-            )],
-        );
+        let list_of_union_of_lists = list(union([list(int()), list(str()), list(bytes())]));
+
         assert_eq!(specialization_depth(&db, list_of_union_of_lists), 2);
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:430 on 2025-10-21 16:22_

Should this be `Type::homogeneous_tuple()` rather than `KnownClass::Tuple.to_instance()`?

---

_@AlexWaygood reviewed on 2025-10-21 16:22_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-21 18:04_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 18:04_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-21 18:24_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-21 18:24_

---

_@sharkdp reviewed on 2025-10-21 18:25_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:430 on 2025-10-21 18:25_

Yes. I actually fell into this trap where I thought I could just run `KnownClass::Tuple.to_specialized_instance(…)` to get a `tuple` type. I don't know *what* it generates, but it's not a tuple, apparently. Maybe we should prevent other developers from doing that by adding some kind of check?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:430 on 2025-10-21 18:35_

Yes, some kind of debug assertion in `.to_instance()` that checks you're not trying to create a tuple type using that method would probably be great

---

_@AlexWaygood reviewed on 2025-10-21 18:35_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:377 on 2025-10-21 18:35_

That is the "not so elegant" part of this PR, exactly. But 10 already seems to be deep enough not to cause any ecosystem changes here. Well, that's a bit hard to tell, of course, because `Divergent` acts like a dynamic type. But I *did* see changes when the limit was too low (or rather: when I was accidentally computing depths that were too large). Because introducing dynamic types all over the place typically leads to a reduction of diagnostics.

---

_@sharkdp reviewed on 2025-10-21 18:35_

---

_@mtshiba reviewed on 2025-10-22 03:15_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-22 03:15_

I don't actually think it's necessary to apply this logic everywhere. That's work in progress in #20566.

The only place where this logic needs to be applied is within tracked functions that may diverge (e.g. `implicit_attribute_inner`, `infer_expression_type_impl`). That means that a `Divergent`-type suppression needs to be performed by the end of a cycle, and it would be most natural to do this at the end of the tracked function. Other places, such as `infer_tuple_expression`, don't actually need this logic. In fact, it's been removed in https://github.com/astral-sh/ruff/pull/20566/files#diff-5ade3020ce233d81ee50c1ec2debd673afc8ba08ede8ec664ca62e62aefbb2f7L5273-L5279, https://github.com/astral-sh/ruff/pull/20566/files#diff-411da1a5c9e0560d6b43091ca27247357a1e94a9077f9adb2cd6a23f495578b6L24-L28.

cc (as author of #20333, etc.): @carljm 

---

_Converted to draft by @sharkdp on 2025-10-22 09:26_

---

_@sharkdp reviewed on 2025-10-22 11:11_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:322 on 2025-10-22 11:11_

We probably could, but the `self.max_depth == usize::MAX` cases are hopefully *not* what we need to optimize for. It's not important that this is extremely fast for (too) deeply nested generic types. But it is important that this is fast for literally everything else.

---

_@AlexWaygood reviewed on 2025-10-22 11:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:322 on 2025-10-22 11:14_

Thanks, that's helpful. It might be good to put that in a comment (that this "optimization" could actually be a pessimisation) -- I'm used to seeing that kind of "return early" clause in ad-hoc AST visitors like this that we write for Ruff rules, and was surprised not to see it here

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:430 on 2025-10-22 11:16_

For posterity: we did this in https://github.com/astral-sh/ruff/pull/21027

---

_@AlexWaygood reviewed on 2025-10-22 11:16_

---

_@sharkdp reviewed on 2025-10-22 11:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-22 11:26_

I had completely missed https://github.com/astral-sh/ruff/pull/20566 — thank you for the reference. It looks like that still requires adding divergent-handling in lots of places, but I'm certainly not opposed to that. I would very much like there to be a more principled solution.

My problem is that I want something that fixes these panics *now* (to finally merge the type-of-`self` PR). I rolled back most of the changes here to leave the `CycleRecovery` setup intact and to be less invasive. I am proposing that we merge this PR to solve the immediate problem, and I'll be happy to remove all of this once we have a better solution (that still solves all of the test cases that are introduced in this PR).

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:322 on 2025-10-22 11:27_

It looks like we'll soon remove all of this code anyway, so I'll leave it as is for now.

---

_@sharkdp reviewed on 2025-10-22 11:27_

---

_Marked ready for review by @sharkdp on 2025-10-22 11:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:375 on 2025-10-22 11:33_

10 does feel quite conservative here. My instinct would be to go with something a _bit_ larger (32?) if it's possible. But it also probably doesn't matter that much if we're planning to get rid of this soon anyway. This is probably fine for a stopgap measure.

---

_@AlexWaygood approved on 2025-10-22 11:33_

Thank you

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-22 11:36_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-22 11:36_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:1275 on 2025-10-22 11:39_

Nit: I don't think this assertion is necessary. Rust always panics on out of bound index (even in release builds as not doing so would be UB)

---

_@sharkdp reviewed on 2025-10-22 11:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:375 on 2025-10-22 11:39_

I made it 32 for now. I think we can only really fine-tune it once we combine this with type-of-`self`, which introduces those cases. I was worried that there are examples like
```py
from typing import Self

def duplicate[T](x: T) -> tuple[T, T]:
    return (x, x)

class C:
    def f(self: Self):
        self.x = duplicate(self.x)

reveal_type(C().x)
```
where the type expands *in breadth* exponentially at each layer. 

---

_@MichaReiser approved on 2025-10-22 11:40_

---

_@sharkdp reviewed on 2025-10-22 11:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:375 on 2025-10-22 11:46_

Actually, I'm setting it back to 14, because that seems to be at the edge of what we can still handle in that particular scenario. I added this as a test case.

---

_@AlexWaygood reviewed on 2025-10-22 11:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:375 on 2025-10-22 11:47_

Thanks for trying it out

---

_@mtshiba reviewed on 2025-10-22 11:49_

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-22 11:49_

Yes, as I said [here](https://github.com/astral-sh/ruff/pull/20566#issuecomment-3431449139), I'm in favor of provisionally merging the PR  anyway; the lack of self types means that a lot of recursive type inference is hidden by dynamic types.

---

_@sharkdp reviewed on 2025-10-22 11:51_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/visitor.rs`:375 on 2025-10-22 11:51_

And here's an example of a case that we "can't handle" anymore with this PR (in the sense that we would lose precise type inference for that dict): https://play.ty.dev/e9b94a3d-704c-4078-90fc-ed9915cfb5e7

---

_@sharkdp reviewed on 2025-10-22 12:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1635 on 2025-10-22 12:17_

Thank you for chiming in here, very much appreciated!

---

_Merged by @sharkdp on 2025-10-22 12:29_

---

_Closed by @sharkdp on 2025-10-22 12:29_

---

_Branch deleted on 2025-10-22 12:29_

---
