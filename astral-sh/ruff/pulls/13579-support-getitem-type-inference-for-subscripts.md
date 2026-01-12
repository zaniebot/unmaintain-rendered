```yaml
number: 13579
title: "Support `__getitem__` type inference for subscripts"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/getitem
created_at: 2024-10-01T03:41:46Z
updated_at: 2024-10-01T18:13:16Z
url: https://github.com/astral-sh/ruff/pull/13579
synced_at: 2026-01-12T15:55:44Z
```

# Support `__getitem__` type inference for subscripts

---

_@charliermarsh_

## Summary

Follow-up to https://github.com/astral-sh/ruff/pull/13562, to add support for "arbitrary" subscript operations.


---

_Review requested from @carljm by @charliermarsh on 2024-10-01 03:41_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-01 03:41_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-01 03:41_

---

_@charliermarsh reviewed on 2024-10-01 03:42_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2639 on 2024-10-01 03:42_

Very hard to test these right now given various TODO types that are returned.

---

_@charliermarsh reviewed on 2024-10-01 03:42_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2633 on 2024-10-01 03:42_

Is there anything to do here yet? E.g., I _could_ "manually" validate that `"abc"[()]` is not a valid operation, but I guess I'd rather get this "for free" from typeshed?

---

_Label `red-knot` added by @charliermarsh on 2024-10-01 03:42_

---

_Renamed from "Support __getitem__ type inference for subscripts" to "Support `__getitem__` type inference for subscripts" by @charliermarsh on 2024-10-01 03:42_

---

_Comment by @github-actions[bot] on 2024-10-01 03:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-10-01 04:16_

We're getting new errors in the benchmark due to expressions like `frozenset[str]` or `dict[str]`:

```
Dunder method `__class_getitem__` is not callable on type 'Literal[frozenset]'
```

---

_Comment by @charliermarsh on 2024-10-01 04:18_

Those seem to have a type that's a union of `Unknown` and the `__class_getitem__` function definition.

---

_Comment by @charliermarsh on 2024-10-01 04:19_

Okay, I think (?) that's because in typeshed they're defined as:

```
if sys.version_info >= (3, 9):
    def __class_getitem__(cls, item: Any, /) -> GenericAlias: ...
```

---

_@AlexWaygood reviewed on 2024-10-01 07:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2633 on 2024-10-01 07:30_

Correct, nothing really to do here at this point!

---

_@AlexWaygood reviewed on 2024-10-01 07:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2667 on 2024-10-01 07:31_

nit:

```suggestion
                // Otherwise, emit a diagnostic.
```

---

_Comment by @AlexWaygood on 2024-10-01 10:23_

> Those seem to have a type that's a union of `Unknown` and the `__class_getitem__` function definition.

Hmm, that's kinda odd that it's creating issues, though. Both `Unknown` and the `__class_getitem__` function should be treated as callable, because `Unknown` should have all possible attributes

---

_@AlexWaygood reviewed on 2024-10-01 10:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2613 on 2024-10-01 10:25_

I wrote these docs! Glad they're coming in useful 😃

---

_@AlexWaygood reviewed on 2024-10-01 10:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6794 on 2024-10-01 10:26_

nit (since a literal `str` is inferred as a different `Type` variant to an arbitrary `str` in our semantic model):

```suggestion
    fn subscript_str_literal() -> anyhow::Result<()> {
```

---

_@AlexWaygood reviewed on 2024-10-01 10:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2639 on 2024-10-01 10:32_

Something like this seems to pass for me locally:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 15b2626a8..f74c833ef 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -6829,6 +6829,30 @@ mod tests {
         Ok(())
     }
 
+    #[test]
+    fn subscript_not_callable_getitem() -> anyhow::Result<()> {
+        let mut db = setup_db();
+
+        db.write_dedented(
+            "/src/a.py",
+            "
+                class NotSubscriptable:
+                    __getitem__ = None
+
+                a = NotSubscriptable()[0]
+            ",
+        )?;
+
+        assert_public_ty(&db, "/src/a.py", "a", "Unknown");
+        assert_file_diagnostics(
+            &db,
+            "/src/a.py",
+            &["Dunder method `__getitem__` is not callable on type 'NotSubscriptable'."],
+        );
+
+        Ok(())
+    }
```

---

_Comment by @AlexWaygood on 2024-10-01 14:13_

> > Those seem to have a type that's a union of `Unknown` and the `__class_getitem__` function definition.
> 
> Hmm, that's kinda odd that it's creating issues, though. Both `Unknown` and the `__class_getitem__` function should be treated as callable, because `Unknown` should have all possible attributes

Okay @charliermarsh I think this is because you're doing this:

```rs
                    let CallOutcome::Callable { return_ty } =
                        dunder_getitem_method.call(self.db, &[slice_ty])
                    else
```

But the result of `Union[Unknown, <some function>].call()` is not `CallOutcome::Callable` (even though all items in the union are actually callable), it's `CallOutcome::Union`

---

_Comment by @AlexWaygood on 2024-10-01 14:15_

I think you can fix this by making use of one of these two helper methods we have on `CallOutcome`, both of which abstract away the difference between `CallOutcome::Callable` and `CallOutcome::Union`:

https://github.com/astral-sh/ruff/blob/cfd5d639176730a6d4a299864e7606d024a0f55a/crates/red_knot_python_semantic/src/types.rs#L851-L971

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1335 on 2024-10-01 16:51_

Nit: this feels a bit less verbose, and more consistent with our other diagnostics:
```suggestion
                "Cannot subscript object of type '{}' with no `__getitem__` method.",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1352 on 2024-10-01 16:53_

Nit: I don't like using "dunder" in diagnostic messages, it feels a little too casual to me. It's snuck into the Python docs here and there, but it's not used in the data model docs.
```suggestion
                "Method `{dunder}` is not callable on object of type '{}'.",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2633 on 2024-10-01 16:55_

I actually would remove this TODO comment, because we already have a TODO for it inside `Type::call`, and that's where the validation should actually happen. I don't think there's anything more to do at this particular location.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2636 on 2024-10-01 17:00_

As @AlexWaygood mentioned in another comment, matching directly on `CallOutcome` this way doesn't work correctly for calling unions. It also bypasses the diagnostics emitted by `CallOutcome::unwrap_with_diagnostic`, meaning the user gets no feedback about what the type of `__getitem__` is, that isn't callable.

The intended way to use `CallOutcome` is to just call `unwrap_with_diagnostic` on it, and not necessarily emit your own diagnostic on top of that. In this case, that means the user won't get the context that it's the `__getitem__` that wasn't callable, so we may want to add a way to pass additional context into `CallOutcome::unwrap_with_diagnostic` further identifying what it is that we tried and failed to call. (This would then mean we don't need `dunder_not_callable_diagnostic` anymore.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2639 on 2024-10-01 17:01_

Yeah, a test like that should work; I'm curious what forms of test you tried, and what Todo types you ran into.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2648 on 2024-10-01 17:08_

I think we should add `Type::is_class` (true if it's a `Type::Class` or a union of them) instead of directly matching on the `Type` variant this way. The reason is because we should also support `__class_getitem__` on a union of classes. `Type::member` and `Type::call` both already support unions, so it should "just work" if we don't prevent it here.

And lets also add a test for this case.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2654 on 2024-10-01 17:09_

Same comment as above about using `CallOutcome::unwrap_with_diagnostic` instead of directly matching on the `CallOutcome`.

---

_@carljm reviewed on 2024-10-01 17:10_

---

_@AlexWaygood reviewed on 2024-10-01 17:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2648 on 2024-10-01 17:15_

When we add support for `type[T]`, we'll need to remember to modify `Type::is_class()` so that it returns `true` for `type[T]` as well. `x.__class_getitem__` should not be an error if the type of `x` is `type[dict]` (since all subclasses of `dict` have `__class_getitem__` methods as well as `dict`).

Nothing to do for now, but a TODO comment in `Type::is_class` might be good

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2639 on 2024-10-01 17:20_

I honestly can't remember what I tried but it led to me adding `__call__` support. I think I was looking to resolve a type annotation in a class member and it was returning `Todo`, but I can't remember the details.

---

_@charliermarsh reviewed on 2024-10-01 17:20_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2648 on 2024-10-01 17:33_

So the test case here would be "The class is a union" rather than "`__class_getitem` is a union", right?

---

_@charliermarsh reviewed on 2024-10-01 17:33_

---

_@carljm reviewed on 2024-10-01 17:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2648 on 2024-10-01 17:35_

Yeah -- in part because the former will necessarily imply the latter (when you get a member of a union, you get the union of the members of all the constituent types of the union)

---

_Comment by @charliermarsh on 2024-10-01 17:37_

@carljm -- I believe all the feedback is resolved with the exception that the dunder diagnostic regressed due to the use of `unwrap_with_diagnostic` (as you mentioned). I may look at that in a separate PR since it doesn't necessarily need to be coupled to these changes.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:410 on 2024-10-01 17:39_

Per Alex's comment:
```suggestion
            // TODO also type[X], once we add that type
            _ => false,
```

---

_@carljm approved on 2024-10-01 17:41_

Looks great!

---

_@charliermarsh reviewed on 2024-10-01 17:41_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:410 on 2024-10-01 17:41_

Thank you missed that.

---

_@AlexWaygood reviewed on 2024-10-01 17:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 17:45_

I'm not sure we'll do the right thing with this for something like

```py
flag = True

if flag:
    class Foo:
        def __class_getitem__(self, x: int) -> str:
            pass
else:
    class Foo:
        pass

Foo[42]
```

Here we want to:
1. Infer a union type for the `Foo` variable (it could be the first class definition, or the second)
2. Emit a diagnostic because not all items in the union support being subscripted
3. Infer `str | Unknown` for the result of the subscription

---

_@charliermarsh reviewed on 2024-10-01 17:50_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 17:50_

I think that is working as described? I added a test.

---

_@carljm reviewed on 2024-10-01 17:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 17:51_

Yeah, I was debating whether to bring this up earlier, or whether it's adequate to just say "not subscriptable" in this case. I agree the behavior you're describing is probably more ideal. It might require explicitly handling unions as a special case here, and calling the class-getitem resolution method per element of the union? I'd also be fine with a TODO for this so this PR can land.

---

_@charliermarsh reviewed on 2024-10-01 17:54_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 17:54_

Are you instead thinking of something like this:

```python
flag = True

if flag:
    class Foo:
        def __class_getitem__(self, x: int) -> str:
            pass
else:
    Foo = 1

Foo[42]
```

Where not all members of the union are classes?

---

_@AlexWaygood reviewed on 2024-10-01 17:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 17:57_

Huh, your tests are pretty convincing. Thanks for adding them!

---

_@AlexWaygood approved on 2024-10-01 17:57_

Thanks!

---

_@charliermarsh reviewed on 2024-10-01 17:59_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 17:59_

I added a test for the case I described above, which _does_ resolve to `Unknown` (with a TODO).

---

_@AlexWaygood reviewed on 2024-10-01 18:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:412 on 2024-10-01 18:01_

Ahh, I think I _did_ spot a bug but didn't accurately identify where the bug would manifest? Anyway, thanks again, a TODO for now is fine by me

---

_Merged by @charliermarsh on 2024-10-01 18:04_

---

_Closed by @charliermarsh on 2024-10-01 18:04_

---

_Branch deleted on 2024-10-01 18:04_

---

_@carljm reviewed on 2024-10-01 18:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7019 on 2024-10-01 18:07_

This case also highlights that perhaps our "not subscriptable" diagnostic should mention `__class_getitem__` instead of `__getitem__` if `Type::is_class` is true? But I don't think that's critical to fix now.

---

_@charliermarsh reviewed on 2024-10-01 18:11_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:7019 on 2024-10-01 18:11_

https://github.com/astral-sh/ruff/pull/13596

---
