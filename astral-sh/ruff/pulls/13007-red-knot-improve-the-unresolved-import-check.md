```yaml
number: 13007
title: "[red-knot] Improve the `unresolved-import` check"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-import-from
created_at: 2024-08-20T12:55:18Z
updated_at: 2024-08-22T19:27:56Z
url: https://github.com/astral-sh/ruff/pull/13007
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Improve the `unresolved-import` check

---

_Pull request opened by @AlexWaygood on 2024-08-20 12:55_

## Summary

This PR improves the `unresolved-import` check so that it once again is able to recognise unresolved `import from` imports. It adds the same tests as #12986, but doesn't add the `UnknownTypeKind` enum that that PR introduces.

I've had to comment out some assertions in the tests for now. One case that passes with #12986 but doesn't work with this PR is this:

- `src/a.py`: `import foo as foo`
- `src/b.py`: `from a import foo`

This should only cause an `unresolved-import` diagnostic to be emitted on `a.py`, and this is the case with #12986, but with this PR `unresolved-import` diagnostics are emitted on both files.

Another case that passes with #12986 but not with this PR is the case where a symbol that exists, but has an inferred type of `Unknown`, is imported from another module. This should not trigger an `unresolved-import` diagnostic, but does with this PR, as it's hard to determine the root cause of the `Unknown` type.

Overall, this PR feels to me like it's an improvement on the status quo, but inferior to #12986. It's also a less significant change than #12986, however, since it's not a significant change to the design of how we think about types in red-knot, so the idea is that it should be possible to land this and then rebase #12986 on top of it.

## Test Plan

Several tests added to `crates/red_knot_python_semantic/src/types.rs` and `crates/red_knot_python_semantic/src/types/infer.rs`. The assertions in the benchmarks have also been updated.


---

_Label `red-knot` added by @AlexWaygood on 2024-08-20 12:55_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-20 12:55_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-20 12:55_

---

_Comment by @github-actions[bot] on 2024-08-20 13:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-08-20 14:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1791 on 2024-08-20 14:56_

```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
```

---

_@MichaReiser reviewed on 2024-08-20 14:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 14:57_

I still find it odd that we have to do the "unbound" check everywhere where we call `.member`. Do we not always want to emit a diagnostic when failing to resolve a member?

---

_@AlexWaygood reviewed on 2024-08-20 15:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 15:11_

We do, but it will need to be different error codes. It would be very counterintuitive for users if `import foo` triggers an `unresolved-import` error code and `from foo import bar` triggers an `unresolved-member` error code. Users will expect all import-related failures to be the same error code and all attribute-access-related failures to be a different error code.

---

_@MichaReiser reviewed on 2024-08-20 15:18_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 15:18_

That makes sense. We might just want to parameterize the `member` method. But maybe that's not worth it? I don't know. Have you looked at how mypy/pyright do this?

---

_@AlexWaygood reviewed on 2024-08-20 15:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 15:32_

> We might just want to parameterize the `member` method.

Not sure exactly what you mean; could you clarify?

> Have you looked at how mypy/pyright do this?

Mypy has a very different architecture to the one we're building. It resolves all imports eagerly in a first pass before it does any other type checking; `module-not-found` errors are emitted during this first pass here: <https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/build.py#L2600-L2700>.

I'm not as familiar with pyright's codebase but I can dig in now

---

_@AlexWaygood reviewed on 2024-08-20 15:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 15:50_

Hmm... it seems both mypy and pyright in fact do what you'd prefer here -- they both use "attribute-access" error codes for `from x import y` imports:
- pyright: <https://pyright-play.net/?pyrightVersion=1.1.368&code=GYJw9gtgBAxmA28CmMAuBLMA7AzgOgEMAjGKdCABzBFSmDDCA>
- mypy: <https://mypy-play.net/?mypy=latest&python=3.12&gist=3110174b575f8d8eba319462c45a8ddf>

Pyright's error code is `reportAttributeAccessIssue`; mypy's is `attr-defined`, which both seem equally bad. In terms of error messages, pyright definitely wins, though: pyright has:

```
"foo" is unknown import symbol
```

whereas mypy has

```
Module "collections.abc" has no attribute "foo" 
```

I'm somewhat surprised by this. But given this precedent, I'm okay with emitting the diagnostic from `.member()`, if you'd prefer. Though I hope we still aspire to be more user-friendly in our error messages than either mypy or pyright.

---

_@MichaReiser reviewed on 2024-08-20 16:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 16:04_

I'm not suggesting that we should use a simple error message. I'm only raising the question if we should change `member` so that it always requires handling the error where the member might be unbound by either returning a `Result` or having a closure or similar to create a diagnostic if the member is unbound.

---

_@AlexWaygood reviewed on 2024-08-20 16:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 16:05_

Oh, I agree that that makes a lot of sense! I can make that change

---

_@AlexWaygood reviewed on 2024-08-20 16:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 16:20_

Hmmm... if the inferred type of a member access is a union, e.g. `int | Unbound`, would you expect that to be represented as an `Ok()` or an `Err()` variant (if we return an `Result` from `.member()`?

---

_@AlexWaygood reviewed on 2024-08-20 16:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-20 16:31_

> having a closure or similar to create a diagnostic if the member is unbound.

This might be doable, but after looking at it for a little bit I think it would still be quite a large refactor and design change. I think it's out of the scope of this PR.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:432 on 2024-08-21 10:50_

Hmm, I'm not so sure about this and getting predictable diagnostic if we omit some might be difficult to achieve (or at least, requires a post-processing step). 

The reason why I'm unsure if that's indeed the desired behavior is that diagnostics that are a consequence of the import are missing then become harder to understand if the missing import violation only comes later in the list of diagnostics (because of how paths get sorted). One such example could be a strict mode that creates a warning whenever the type checker infers `Unknown`. 

The other reason why I think we should remove this TODO is because the elimination of the second diagnostic has to happen at a higher level than `check_types`. Calling `check_types` should return all diagnostics for that file. It's up to `workspace.check` to decide if some diagnostics should be suppressed across files.

---

_@MichaReiser approved on 2024-08-21 10:54_

I would still have preferred to implement a holistic solution to how we handle unresolved members rather than implementing a one off solution because I'm worried that we (I for example) end up copying this pattern. That was also the reason why I intentionally didn't implement this diagnostic in my original PR.

I'm okay with merging if you find the parity important but I ask you to at least add a TODO comment to make it clear that this pattern *should not* be copied.



---

_@AlexWaygood reviewed on 2024-08-21 11:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:432 on 2024-08-21 11:12_

I'm not sure that producing a bogus diagnostic here and then filtering it out at a higher level would be appropriate. I don't think we should produce an `unresolved-import` diagnostic here at all, because there is no unresolved import here. The import is fully resolved to a symbol that exists in the symbol table of `a.py`. The fact that the symbol in `a.py` has been inferred as having type `Unknown` is immaterial to whether the import of that symbol from `a.py` into `b.py` can be resolved.

Note that this a general problem with the lint currently for any symbol that has a type inferred as `Unknown`, not just symbols that are inferred as `Unknown` because of unresolved imports specifically. This is the cause of six of the new false-positive errors in the benchmark. It's one of the problems that would be fixed if we adopted something like #12986.

For now, I'll change the comment from a TODO into an explanatory note, so that it's clear that we don't yet have agreement on where the problem should be solved

---

_Comment by @AlexWaygood on 2024-08-21 11:14_

> I would still have preferred to implement a holistic solution to how we handle unresolved members

Well, so would I! #12986 was my attempt 😆 The idea behind this PR was to try to implement as many of the fixes implemented there as possible without fundamentally reworking red-knot's design, so that it would be clearer exactly what the pros and cons of doing so would be.

> I ask you to at least add a TODO comment to make it clear that this pattern _should not_ be copied.

I'll add this

---

_@MichaReiser reviewed on 2024-08-21 11:21_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:432 on 2024-08-21 11:21_

Oh I see. I misunderstood the test case. 

I assumed that we only wanted to emit one diagnostic for:

```python
# a .py
import random # let's pretend this import doesn't exist

# b.py
import random
```



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:432 on 2024-08-21 11:22_

It could make sense to split this out into its own test and mark it with `ignore`. 

---

_@MichaReiser reviewed on 2024-08-21 11:22_

---

_@AlexWaygood reviewed on 2024-08-21 11:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:432 on 2024-08-21 11:23_

> It could make sense to split this out into its own test and mark it with `ignore`.

Sounds good

---

_Comment by @codspeed-hq[bot] on 2024-08-21 13:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/redknot-import-from)

### Merging #13007 will **improve performances by 4.16%**

<sub>Comparing <code>alex/redknot-import-from</code> (2c1fb1d) with <code>main</code> (785c399)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `alex/redknot-import-from` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[unicode/pypinyin.py]` | 354.7 µs | 340.6 µs | +4.16% |


---

_Merged by @AlexWaygood on 2024-08-21 13:44_

---

_Closed by @AlexWaygood on 2024-08-21 13:44_

---

_Branch deleted on 2024-08-21 13:44_

---

_@carljm reviewed on 2024-08-22 00:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-22 00:08_

I'm not convinced there is any problem with reporting an error on `from foo import bar` (where the module `foo` does exist, but has no top-level symbol `bar`) as an attribute-access error code. If I had to pick between the pyright or mypy error messages above, I would prefer the mypy one (the pyright one gives less useful context). The fact that the attribute access occurred as part of a from-import will be clear from the error location; it doesn't have to be part of the error message or code. I wouldn't want to make design decisions based on the hypothesis that this is confusing to users without clear evidence that it is (e.g. a history of users filing issues on mypy about that error code or message); personally I find it intuitive.

---

_@carljm reviewed on 2024-08-22 00:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1022 on 2024-08-22 00:42_

I don't think we should return an `Option` or `Result` from `member()`, for the reason @AlexWaygood  mentions above: we can have a possibly-unbound name (currently represented as a union with Unbound), and it's not clear how that should look in a binary representation.

We could design our own representation for "Type plus definitely-bound/maybe-unbound/definitely-unbound state" and use that instead of `Type::Unbound`. This will complicate some APIs and I think be somewhat less efficient, but it would have the advantage that it would force handling Unbound early, which I think is desirable. In general I don't think we want Unbound escaping from the scope where it originated; it should instead result in a diagnostic and be eliminated from the type (replaced with `Unknown` if we have no other type information; probably just disappear from the type if we do.) I do think this alternative to `Type::Unbound` is worth exploring and seeing what kind of impact it will have.

I was thinking that we would push diagnostics inside `member()` to simplify the handling of Unbound, and I think I still favor that approach. If we do want to make distinctions in error codes/messages based on context from the caller of `member()`, I think we can always pass in context to make that possible.

---

_@carljm reviewed on 2024-08-22 00:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:441 on 2024-08-22 00:49_

Since there's been some discussion in this PR of error message wording and the implications it might have for where we push diagnostics: I think we should avoid passive voice here, aim for simple statements of fact in error messages, and avoid more complex words like "resolved" that don't add clarity. E.g. I think this error message should be something like "Module 'bar' not found." or "Module 'bar' does not exist."

---

_@carljm reviewed on 2024-08-22 00:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:455 on 2024-08-22 00:55_

Here I think describing it as a failed attribute access would actually be much clearer than the vague "Could not resolve" which doesn't really communicate anything! This message also doesn't currently clarify that `'a'` is a module (though it could be taken as implied.) I actually can't think of an error message here that I think would be better than "Module `a` has no attribute `thing`."

---

_@carljm reviewed on 2024-08-22 00:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:461 on 2024-08-22 00:57_

I think when we issue the error on the import in `a.py`, we should set the type of `foo` in `a` to `Unknown`, not `Unbound`, which should avoid this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:940 on 2024-08-22 00:59_

Why not just emit the diagnostics inside this function, return an `Option`, and avoid the need for `ModuleResolutionError`?

---

_@carljm reviewed on 2024-08-22 00:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1035 on 2024-08-22 01:01_

I don't think we should be emitting a diagnostic at all if the imported symbol is of type `Unknown`. `Unknown` might _result_ from an error in the other module, but in that case the diagnostic should occur in the other module; importing a value of unknown type is not an error.

I think just removing this clause would fix the ignored test; what would it break?

---

_@carljm reviewed on 2024-08-22 01:01_

---

_@carljm reviewed on 2024-08-22 01:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1064 on 2024-08-22 01:04_

I don't see the advantage of returning a `Result` vs just emitting the diagnostic directly in this method, as the previous code did.

---

_@carljm reviewed on 2024-08-22 01:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1035 on 2024-08-22 01:26_

Yeah, the only reason we have to check for Unknown here is that we intentionally replaced Unbound with Unknown just a few lines above. If we wait to do it after, the bug in the ignored test goes away, and all tests pass:

```
diff --git a/crates/red_knot_python_semantic/src/types.rs b/crates/red_knot_python_semantic/src/types.rs
index 173c957d1..c4ff40220 100644
--- a/crates/red_knot_python_semantic/src/types.rs
+++ b/crates/red_knot_python_semantic/src/types.rs
@@ -451,9 +451,6 @@ mod tests {
         );
     }

-    #[ignore = "\
-A spurious second 'Unresolved import' diagnostic message is emitted on `b.py`, \
-despite the symbol existing in the symbol table for `a.py`"]
     #[test]
     fn resolved_import_of_symbol_from_unresolved_import() {
         let mut db = setup_db();
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 9b183727e..28dc8f2e3 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -1012,15 +1012,9 @@ impl<'db> TypeInferenceBuilder<'db> {
             asname: _,
         } = alias;

-        // If a symbol is unbound in the module the symbol was originally defined in,
-        // when we're trying to import the symbol from that module into "our" module,
-        // the runtime error will occur immediately (rather than when the symbol is *used*,
-        // as would be the case for a symbol with type `Unbound`), so it's appropriate to
-        // think of the type of the imported symbol as `Unknown` rather than `Unbound`
         let member_ty = module_ty
             .unwrap_or(Type::Unbound)
-            .member(self.db, &Name::new(&name.id))
-            .replace_unbound_with(self.db, Type::Unknown);
+            .member(self.db, &Name::new(&name.id));

         if matches!(module_ty, Err(ModuleResolutionError::UnresolvedModule)) {
             self.add_diagnostic(
@@ -1032,7 +1026,7 @@ impl<'db> TypeInferenceBuilder<'db> {
                     module.unwrap_or_default()
                 ),
             );
-        } else if module_ty.is_ok() && member_ty.is_unknown() {
+        } else if module_ty.is_ok() && member_ty.is_unbound() {
             self.add_diagnostic(
                 AnyNodeRef::Alias(alias),
                 "unresolved-import",
@@ -1044,6 +1038,8 @@ impl<'db> TypeInferenceBuilder<'db> {
             );
         }

+        let member_ty = member_ty.replace_unbound_with(self.db, Type::Unknown);
+
         self.types.definitions.insert(definition, member_ty);
     }
```

---

_Comment by @carljm on 2024-08-22 01:35_

While I'm making notes on this PR of things that I'd like to address in this area: we've now started to accumulate some tests in `types.rs` that (as far as I can see) are indistinguishable in purpose and kind from the tests we already have in `infer.rs`. Maybe all of the tests should be moved out to `types.rs` instead of `infer.rs`, or should actually live in their own file (or better should be in a more concise testing framework), but I don't think we should be accumulating these tests in two different files without a clear rationale for which tests go in which file.

---

_Comment by @AlexWaygood on 2024-08-22 11:02_

> While I'm making notes on this PR of things that I'd like to address in this area: we've now started to accumulate some tests in `types.rs` that (as far as I can see) are indistinguishable in purpose and kind from the tests we already have in `infer.rs`. Maybe all of the tests should be moved out to `types.rs` instead of `infer.rs`, or should actually live in their own file (or better should be in a more concise testing framework), but I don't think we should be accumulating these tests in two different files without a clear rationale for which tests go in which file.

I think the idea is that the tests in `types.rs` are specifically testing which diagnostics are emitted, whereas the tests in `infer.rs` are (currently) solely focussed on which types are inferred

---

_@AlexWaygood reviewed on 2024-08-22 11:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1035 on 2024-08-22 11:30_

Thanks, I think you're right that this fixes the tests I added (and skipped) in this PR. Here's an interesting edge case that you might not have considered, though. What should we do for something like this?

```py
# foo.py
for x in range(0):
    pass

# bar.py
from foo import x
```

Running `python bar.py` fails with `ImportError` at runtime because `x` is not defined in `foo.py` (due to the iterable being empty), but neither mypy nor pyright catches this. Arguably we should infer the type of `x` after the `for` loop has completed as being `int | Unbound` -- should we flag any union that includes `Unbound` as one of the elements with an `unresolved-import` diagnostic? Probably not, I don't think -- the import _might_ succeed, it just also might not. So let's say (for argument's sake) we use the error code `import-possibly-unbound` for that.

So then what if we have something like this, where the symbol definitely exists in `foo.py`, but is also definitely unbound? (Mypy and pyright _do_ both catch this one.) Should that have the error code `unresolved-import`, or `import-possibly-unbound`?

```py
# foo.py
x: int

# bar.py
from foo import x
```

These are somewhat obscure edge cases, so I think what you propose here is a strict improvement on what I implemented in this PR; I'll make a fixup PR to address your suggestions. But my point is that we may still at some point need some context propagated regarding the cause of the error -- whether that error is represented as `Unknown` or `Unbound`.

---

_Comment by @MichaReiser on 2024-08-22 11:37_

> I think the idea is that the tests in types.rs are specifically testing which diagnostics are emitted, whereas the tests in infer.rs are (currently) solely focussed on which types are inferred

My initial idea was to only test that `typecheck` emits diagnostic and I picked the only existing diagnostic. My intention isn't to test specific diagnostics.


---

_@AlexWaygood reviewed on 2024-08-22 11:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:455 on 2024-08-22 11:49_

Do people really think of `from collections import deque` as an "attribute access" of `deque` from the `collections` module? I certainly don't. Of course that's what's happening under the hood, but even at runtime this detail is hidden -- the import fails with `ImportError` rather than `AttributeError`, and there's no mention of attempting to access an attribute:

```
% python -c "from collections import foo"                
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ImportError: cannot import name 'foo' from 'collections' (/Users/alexw/.pyenv/versions/3.12.4/lib/python3.12/collections/__init__.py)
```

---

_@AlexWaygood reviewed on 2024-08-22 11:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:441 on 2024-08-22 11:51_

(FWIW, I didn't change this error message in this PR)

---

_Comment by @carljm on 2024-08-22 19:05_

> I think the idea is that the tests in `types.rs` are specifically testing which diagnostics are emitted, whereas the tests in `infer.rs` are (currently) solely focussed on which types are inferred

Ah! Yes, I see that distinction does seem to apply currently. I think longer-term it doesn't necessarily make sense to split tests according to this distinction, though. At some point I suspect ~all of our tests will be tests of diagnostics, which can include a diagnostic from `reveal_type()` as one edge case.

---

_@carljm reviewed on 2024-08-22 19:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1035 on 2024-08-22 19:27_

Yeah, great points and great questions! I had the `x: int` case in mind last night when thinking again about representation of unbound-ness in a different comment on this PR.

I agree that maybe-unbound is a different error class than definitely-unbound (and arguable whether maybe-unbound should trigger an error on import at all.)

I think we do want a representation of "definitely unbound and typed" that we use for imports, so e.g. we can error if you import from `x: int` (as long as it's not a stub), but we can still type check the importing module treating `x` as `int` rather than `Unknown`. (Not sure how important this is, but it seems marginally better.) I think this can be handled by declared vs inferred types.

What's less clear to me is if we need a representation of "definitely unbound and typed" for inferred types. It would mean that within a scope we could also check code following `x: int` and emit diagnostics both for "x is not bound" and for use of x that isn't valid for an int. Maybe that's useful?

Like I mentioned above in a different comment, I'm pretty open to exploring other representations of unbound to make it orthogonal to types; the main question for me is if we can avoid it regressing in perf, and if not, do we gain enough from it to be worth the regression?





---
