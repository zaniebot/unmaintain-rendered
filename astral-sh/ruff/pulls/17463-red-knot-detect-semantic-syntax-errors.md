```yaml
number: 17463
title: "[red-knot] Detect semantic syntax errors"
type: pull_request
state: merged
author: ntBre
labels:
  - ty
assignees: []
merged: true
base: main
head: brent/semantic-errors-red-knot
created_at: 2025-04-18T15:37:03Z
updated_at: 2025-05-07T15:19:36Z
url: https://github.com/astral-sh/ruff/pull/17463
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Detect semantic syntax errors

---

_@ntBre_

Summary
--

This PR extends semantic syntax error detection to red-knot. The main changes here are:

1. Adding `SemanticSyntaxChecker` and `Vec<SemanticSyntaxError>` fields to the `SemanticIndexBuilder`
2. Calling `SemanticSyntaxChecker::visit_stmt` and `visit_expr` in the `SemanticIndexBuilder`'s `visit_stmt` and `visit_expr` methods
3. Implementing `SemanticSyntaxContext` for `SemanticIndexBuilder`
4. Adding new mdtests to test the context implementation and show diagnostics

(3) is definitely the trickiest and required (I think) a minor addition to the `SemanticIndexBuilder`. I tried to look around for existing code performing the necessary checks, but I definitely could have missed something or misused the existing code even when I found it.

There's still one TODO around `global` statement handling. I don't think there's an existing way to look this up, but I'm happy to work on that here or in a separate PR. This currently only affects detection of one error (`LoadBeforeGlobalDeclaration` or [PLE0118](https://docs.astral.sh/ruff/rules/load-before-global-declaration/) in ruff), so it's not too big of a problem even if we leave the TODO.

Test Plan
--

New mdtests, as well as new errors for existing mdtests


---

_Label `red-knot` added by @ntBre on 2025-04-18 15:37_

---

_@ntBre reviewed on 2025-04-18 15:38_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:132 on 2025-04-18 15:38_

This actually helped to reveal a bug in the first error here. I opened a separate PR to fix this in #17460, so this and the following case will be reduced to the second error once that lands.

---

_Comment by @github-actions[bot] on 2025-04-18 15:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@ntBre reviewed on 2025-04-18 15:39_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:132 on 2025-04-18 15:39_

That PR does not fix the backticks around "asynchronous comprehension." I can just fix that here since red-knot will be the only place this message is actually used.

---

_Comment by @ntBre on 2025-04-18 15:43_

The `mypy_primer` results look like true positives to me, although it's a bit unfortunate because it seems clear that this file is meant to be split up somehow: 

https://github.com/laowantong/paroxython/blob/44e14b17965c6b02871db4c9ed6acc8716c2740d/examples/idioms/programs_with_labels.py#L690-L695

---

_Comment by @github-actions[bot] on 2025-04-18 16:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @ntBre on 2025-04-18 16:50_

---

_Review requested from @carljm by @ntBre on 2025-04-18 16:50_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-18 16:50_

---

_Review requested from @sharkdp by @ntBre on 2025-04-18 16:50_

---

_Review requested from @dcreager by @ntBre on 2025-04-18 16:50_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-18 16:50_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-18 16:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:76 on 2025-04-18 16:55_

maybe let's just avoid the syntax errors by putting the entire `_` function inside an inner function? I think the syntax-error diagnostics here distract a bit from what this is meant to be testing

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:133 on 2025-04-18 16:57_

similarly here, I don't think the intent here was for this to test our resilience to invalid syntax (the test case is titled "Basic" 😄), so I'd be inclined to do this to avoid the syntax error:

```suggestion
async def _():
    [reveal_type(x) async for x in AsyncIterable()]
```

although it probably _is_ a good idea for us to add a _separate_ test case that demonstrates that we still infer types correctly even if there is invalid syntax!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:153 on 2025-04-18 16:58_

here too

```suggestion
async def _():
    # revealed: @Todo(async iterables/iterators)
    [reveal_type(x) async for x in Iterable()]
```

---

_@AlexWaygood reviewed on 2025-04-18 16:59_

🥳

---

_@ntBre reviewed on 2025-04-18 17:06_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:76 on 2025-04-18 17:06_

Sounds good! I was on the fence between marking the errors or editing the test cases and went for leaving the test code untouched, but this definitely makes sense, especially for these easy-to-fix cases.

What did you think about the `match` cases in `star.md`? I actually started there and was going to edit them so that the name capture was last, but I wasn't sure if the `or` pattern was also important.

```diff
diff --git a/a.py b/b.py
index f39fe59..2dd1a04 100644
--- a/a.py
+++ b/b.py
@@ -5,7 +5,7 @@ match 42:
         ...
     case [O]:
         ...
-    case P | Q:  # error: [invalid-syntax] "name capture `P` makes remaining patterns unreachable"
-        ...
     case object(foo=R):
         ...
+    case P:
+        ...
```

---

_@AlexWaygood reviewed on 2025-04-18 17:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:76 on 2025-04-18 17:09_

Ooh, thanks, I didn't spot that! Yes, it is important that that's an `Or` pattern. The LHS and RHS of the `|` don't necessarily need to be `Name`s though -- you could add two classes to the mdtest:

```py
class Foo: ...
class Bar: ...
```

and then change the pattern to `Foo() | Bar()`?

---

_@ntBre reviewed on 2025-04-18 20:50_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:76 on 2025-04-18 20:50_

Actually in the `P` and `Q` case, it does look important for them to be `Name`s? They're used down below in in a `possibly-unresolved-reference` error:

https://github.com/astral-sh/ruff/blob/454ad15aee13e5ac2fc9b67ff5907658764207df/crates/red_knot_python_semantic/resources/mdtest/import/star.md?plain=1#L244-L246

The shortest diff preserving that later assertion would probably be nesting them inside something else like `case object(p=P) | object(q=Q)`, but I'm happy to do whatever best preserves your original intentions with the tests.

Actually that suggestion causes yet another syntax error in CPython that I didn't cover:

```pycon
>>> match 42:
...     case object(p=P) | object(q=Q): ...
...
  File "<python-input-0>", line 2
    case object(p=P) | object(q=Q): ...
         ^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: alternative patterns bind different names
```

and also causes an `unresolved-reference` to `R` instead of a `possibly-unresolved-reference`.

https://playknot.ruff.rs/616e62da-c1b8-4c2b-9c12-1a7c1bc34851

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:133 on 2025-04-18 21:00_

Updated! Leaving this unresolved for now in case we also want to add the separate test case. I'm guessing that could go in `invalid_syntax.md` next to this file? But maybe somewhere else is better.

---

_@ntBre reviewed on 2025-04-18 21:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2332 on 2025-04-18 21:45_

we already have a method for this one ;)

```suggestion
        self.current_scope_is_global_scope()
```

but maybe we should just delete that method, move the implementation of that method to the trait method here, and refactor the callsites of the method to use the trait method? If you do that, I would simplify this implementation to:

```suggestion
        self.scope_stack.len() == 1
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2348 on 2025-04-18 21:49_

```suggestion
        self.file.source_type(self.db.upcast()).is_ipynb()
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:76 on 2025-04-18 21:50_

Ah, feel free to leave `star.md` as you have it then, don't worry too much about it! Would be great to open a followup issue about this and https://github.com/astral-sh/ruff/pull/17463#discussion_r2050872442 so we don't forget about it, though. I can look at it next week.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:85 on 2025-04-18 21:54_

hmm, yeah, ideally we would get rid of the `invalid-type-form` error here; it's just redundant noise to the user to have two diagnostics for the same problem. I think @MichaReiser has mentioned in the past that he wanted a global solution to make sure we didn't emit any red-knot diagnostics from type inference on nodes that were already known to be invalid syntax. Would be another thing that would be good to file a followup issue for IMO.

---

_@AlexWaygood approved on 2025-04-18 21:55_

This is great, thank you!

---

_@ntBre reviewed on 2025-04-21 13:14_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/annotations/invalid.md`:76 on 2025-04-21 13:14_

Opened #17522 and #17523 and added the new syntax error to #17412!

---

_@ntBre reviewed on 2025-04-21 13:17_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2332 on 2025-04-21 13:17_

I figured I would miss at least one existing method, thanks!

Hmm, I was worried about having to import the trait everywhere this method is used, but so far it's only used in this file. I'll move the implementation to the trait then!

---

_@ntBre reviewed on 2025-04-21 13:21_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:85 on 2025-04-21 13:21_

Opened #17524!

---

_@dhruvmanila reviewed on 2025-04-21 15:41_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:1601 on 2025-04-21 15:41_

Yeah, I wonder if we can use `#[salsa::tracked(return_ref)]` on the `source_text` salsa query instead so that it returns a `&SourceText` instead and `SourceText` is cheap clonable because the inner data is behind an `Arc` so we can just clone wherever the owned `SourceText` is required. (cc @MichaReiser)

---

_@dhruvmanila reviewed on 2025-04-21 15:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:1 on 2025-04-21 15:44_

Does this cover all syntax errors that we detect or does it only cover a subset of it? I think it'd be useful to have at least one test case to cover each syntax error (could be done in a follow-up) so that any changes to the red-knot's semantic model is tested against these checks.

---

_@dhruvmanila reviewed on 2025-04-21 15:45_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:1 on 2025-04-21 15:45_

Is it possible to avoid raising the syntax errors in this file by e.g., separating out the match statements to avoid unreachable patterns? cc @AlexWaygood

---

_@AlexWaygood reviewed on 2025-04-21 15:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:1 on 2025-04-21 15:47_

yes, should definitely be possible -- I'll do it as a followup (https://github.com/astral-sh/ruff/issues/17523)

---

_@dhruvmanila reviewed on 2025-04-21 15:47_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:92 on 2025-04-21 15:47_

At some point in the future, it would be useful to combine these flags into a `bitflags` or similar to avoid multiple fields.

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:1 on 2025-04-21 15:50_

I believe this subset should cover every context method, which should be the main interaction with the semantic model. But it definitely wouldn't hurt to cover every error variant too!

---

_@ntBre reviewed on 2025-04-21 15:50_

---

_@dhruvmanila reviewed on 2025-04-21 15:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2257 on 2025-04-21 15:52_

nit: should this method be named `seen_module_docstring_boundary` to explicitly state that it's for the module docstring and not other (function, class, etc.) docstring?

---

_@dhruvmanila reviewed on 2025-04-21 15:55_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:1 on 2025-04-21 15:55_

Makes sense. This is not urgent and doesn't need to block this PR but something good to have.

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2257 on 2025-04-21 15:56_

Ah, that is what the ruff `SemanticModel`'s method is called too. I'll update it!

---

_@ntBre reviewed on 2025-04-21 15:56_

---

_@dhruvmanila reviewed on 2025-04-21 16:00_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2286 on 2025-04-21 16:00_

We should probably `expect_function` here as otherwise there's some internal error if the scope kind doesn't originate from the function node.

---

_@dhruvmanila approved on 2025-04-21 16:01_

Looks good!

---

_@ntBre reviewed on 2025-04-21 16:29_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:1 on 2025-04-21 16:29_

I might create a help-wanted issue, if that's not too lazy of me :laughing: Only a subset of errors are covered by ruff integration tests too. The rest rely on inline tests.

---

_Comment by @ntBre on 2025-04-21 17:16_

Thanks for the reviews! I think I've addressed all of the feedback we wanted to handle here and opened issues for the other follow-ups. I'll leave this open a bit longer in case anyone else wants to have a look, especially Micha since he's been pinged a couple of times here.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md`:85 on 2025-04-22 11:44_

Yes, there's a TODO in the `InferContext` to suppress diagnostics in that case. I'm not entirely sure yet how that should work but isn't something that should block this PR.

---

_@MichaReiser reviewed on 2025-04-22 11:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/snapshots/semantic_syntax_errors.md_-_Semantic_syntax_error_diagnostics_-_`async`_comprehensions_in_synchronous_comprehensions_-_Python_3.10.snap`:41 on 2025-04-22 11:46_

I'm a bit confused by this message. Isn't `f` async?

It's a bit a shame that we can't make use of the multi range diagnostics because it would be nice to have a second frame outlining "why" the context isn't async (which would address my confusion). I don't think this is something we should solve as part of this PR but maybe something to come back once we also use the new diagnostics in Ruff.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2264 on 2025-04-22 11:48_

It could make sense to cache this lookup in a field if this method is called frequently (`::get` and `.python_version` are roughly as expensive as a hash map lookup)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2342 on 2025-04-22 11:50_

I think it's better to use `source_text().is_notebook` here because that's what we have as source

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2271 on 2025-04-22 11:52_

I think I'd prefer to store a `LazyCell` on `SemanticIndexBuilder` that fetches the `SourceText` for the current file when needed (and stores it). This allows you to return a `&str` here and also allows you to reuse the `source_text` when testing if the current file is a notebook. 

(You can skip the `LazyCell` if both `with_source` and `is_notebook` are called rarely)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 11:54_

Unlike Ruff, Red Knot also runs semantic indexing on third party files. We should avoid reporting any syntax errors for those files. You can check if a file is open by calling `db.is_file_open(file))`. Ideally, we would skip the entire `SemanticSyntaxChecker` for those files but we should at least avoiding pushing any errors.

https://github.com/astral-sh/ruff/blob/7d11ef156439ee63f2ae065cc2336e7fdabb24af/crates/red_knot_python_semantic/src/types/context.rs#L360-L362

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:181 on 2025-04-22 11:56_

This could be a good use case for a `ThinVec` to shrink the size of this struct, considering that most files won't have any `SemanticSyntaxError`. I'm okay making this change as a separate PR and also applying it to `TypeCheckDiagnostics`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:93 on 2025-04-22 11:58_

You could use `extend` here so that it can reserve the right amount of elements (without having to check on every `push` call)

```rust
diagnostics.extend(index.semantic_syntax_errors().iter().map(|diagnostic| create_semantic_syntax_diagnostic(file, error))
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:859 on 2025-04-22 12:00_

I don't think it's good practice to create diagnostics without a main message. I suggest passing `err.to_string()` to the `Diagnostic::new` (instead of "") and instead use no message in the annotation (the annotation's message should describe the annotated part). 

We should make the same change for unsupported syntax and parse diagnostics but we can do this as a seprate PR (unless @BurntSushi thinks what we have now is better)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:1601 on 2025-04-22 12:04_

I don't fully remember the motivation for having `SourceText` and `SourceTextInner` instead of annotating the query with `return_ref` and replacing `SourceText` with `SourceTextInner`. 

We could try changing the definition as a separate PR. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:91 on 2025-04-22 12:07_

Adding the diagnostic sementically fits better into `check_file_impl` because the diagnsotics aren't related to type checking. On the other hand, it doesn't really make a difference. That's why I think either way is fine (up to you)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1056 on 2025-04-22 12:09_

This seems so simple that, arguably, we might just want to move this logic to the `SemanticSyntaxChecker` (the integration complexity is higher)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2368 on 2025-04-22 12:11_

You can call https://github.com/astral-sh/ruff/blob/be54b840e98a35069ab53746996684c887cc102c/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L159-L161

---

_@MichaReiser approved on 2025-04-22 12:11_

This looks great. I've a few smaller suggestions but I think this is good to go

---

_@AlexWaygood reviewed on 2025-04-22 12:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/snapshots/semantic_syntax_errors.md_-_Semantic_syntax_error_diagnostics_-_`async`_comprehensions_in_synchronous_comprehensions_-_Python_3.10.snap`:41 on 2025-04-22 12:17_

I _think_ the "synchronous function" that the async list comprehension is immediately nested inside of here is the function-like scope created by the synchronous dict comprehension. But I agree that it would be great if we could have a clearer error and a multi-span diagnostic

---

_@AlexWaygood reviewed on 2025-04-22 12:19_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2342 on 2025-04-22 12:19_

oops, I think that's what @ntBre had originally but I suggested doing it this way. Sorry @ntBre! (Micha almost certainly knows best here)

---

_@ntBre reviewed on 2025-04-22 12:19_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/resources/mdtest/snapshots/semantic_syntax_errors.md_-_Semantic_syntax_error_diagnostics_-_`async`_comprehensions_in_synchronous_comprehensions_-_Python_3.10.snap`:41 on 2025-04-22 12:19_

Yes, `f` is async, but the version-related part is that you can't use an async comprehension inside of a sync comprehension before 3.11. This is the error message I'm trying to clean up in https://github.com/astral-sh/ruff/pull/17460, so I definitely agree that it's confusing!

That's a good point about the multi-span diagnostic too. We could definitely highlight the outer sync comprehension, as well as the inner async one.

---

_@AlexWaygood reviewed on 2025-04-22 12:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2368 on 2025-04-22 12:21_

Brent is deleting that method as part of this PR; current callsites of that method are being switched over to use the `in_module_scope` method being added here

---

_@ntBre reviewed on 2025-04-22 12:27_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/types.rs`:93 on 2025-04-22 12:27_

`diagnostics.extend` takes an `other: &TypeCheckDiagnostics` argument rather than an `Iterator<Item = Diagnostic>` or similar. That's why I went with `push` instead. Should I try to change some types in the `TypeCheckDiagnostics` implementation? What I really want is just `diagnostics.diagnostics.extend`, but the field is private too.

I could add an `extend_diagnostics` method maybe?

---

_@MichaReiser reviewed on 2025-04-22 12:29_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:93 on 2025-04-22 12:29_

Oh, I assumed it's a vec. 

Would it work if you implement the `Extend` trait?

---

_@ntBre reviewed on 2025-04-22 12:34_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/types.rs`:91 on 2025-04-22 12:34_

Ah, is the `semantic_index` call cached with salsa? I thought I had to do it here because this is where the 
 `SemanticIndex` holding the errors was available, but if I can just call `semantic_index` again in `check_file_impl`, I can definitely move it.

---

_@ntBre reviewed on 2025-04-22 12:35_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1056 on 2025-04-22 12:35_

I think I ran into trouble moving this out of the ruff `Checker`, which led to it being a `Context` method in the first place, but I'll double check. It does look silly here

---

_@ntBre reviewed on 2025-04-22 12:37_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2368 on 2025-04-22 12:37_

Alex actually suggested deleting that method in favor of this one and inlining its implementation here. Happy to revert if you prefer the old one, though.

---

_@ntBre reviewed on 2025-04-22 12:42_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 12:42_

Oh that's good to know! Maybe I can put this check in `with_semantic_checker` and return without visiting anything in those cases. Is this a cheap check to run on ever statement/expression, or do I need to cache it somehow?

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2271 on 2025-04-22 12:46_

I haven't profiled to be sure, but they don't look like they're called particularly often. `is_notebook` is usually gated by `in_module_scope`, but it is used in the `async` checks in global scope. `with_source` is only used for extracting a duplicate `match` mapping key, so it's almost never called.

But I still like the idea of the `LazyCell` and restoring the simple signature for the `source` method, so I think I'll do that.

---

_@ntBre reviewed on 2025-04-22 12:46_

---

_@ntBre reviewed on 2025-04-22 12:53_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2342 on 2025-04-22 12:53_

No worries, I think `source_text` makes more sense in combination with the suggestion to cache it down below than it did when I wrote it!

---

_@ntBre reviewed on 2025-04-22 12:55_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/types.rs`:93 on 2025-04-22 12:55_

Me too until `extend` didn't work :laughing: I'll try `Extend`, thanks!

---

_@ntBre reviewed on 2025-04-22 12:55_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2368 on 2025-04-22 12:55_

Wow I should have refreshed before commenting! Thanks @AlexWaygood!

---

_@ntBre reviewed on 2025-04-22 13:22_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2271 on 2025-04-22 13:22_

I'm actually having some trouble with the `LazyCell`. With this straightforward approach:

```rust
        let mut builder = Self {
            db,
            file,
            source_text: LazyCell::new(|| source_text(db, file)),
            // ...
        };
```

the compiler is complaining that it found a closure capturing variables instead of an `fn` item. Am I doing something silly here, or should I try something else?

Obviously `LazyCell` closures can generally capture variables because there's an example in ruff, but maybe it's because I'm trying to return it.

---

_@ntBre reviewed on 2025-04-22 14:41_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/types.rs`:93 on 2025-04-22 14:41_

I went with the `extend_diagnostics` method approach. The implementation is the same as `impl Extend<Diagnostic>`, but I had to call it in the qualified `Extend::extend` form to differentiate from the current `extend` method, so I thought it looked nicer this way.

---

_@ntBre reviewed on 2025-04-22 15:37_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/types.rs`:91 on 2025-04-22 15:37_

Oh, `check_file_impl` is actually in a different crate, and `semantic_index` is `pub(crate)`. I think I'll just leave it here.

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1056 on 2025-04-22 15:47_

I moved this field onto the `SemanticSyntaxChecker`. I think the previous problem was from also trying to delete the flag from ruff's `SemanticModel`. So there's a slight duplication with ruff tracking it separately, but it's just a single `bool` and allows deleting a whole context method.

---

_@ntBre reviewed on 2025-04-22 15:47_

---

_Comment by @codspeed-hq[bot] on 2025-04-22 15:54_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fsemantic-errors-red-knot)

### Merging astral-sh/ruff#17463 will **not alter performance**

<sub>Comparing <code>brent/semantic-errors-red-knot</code> (931fcf1) with <code>main</code> (5407249)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_Comment by @ntBre on 2025-04-22 16:07_

I'm guessing (hoping) the horrible performance degradation was from my naive use of `is_file_open`. I started caching that on the builder and also merged main in case it was something else.

---

_Comment by @ntBre on 2025-04-22 16:15_

One more guess and then I'll set up for local benchmarks :sweat_smile: I'm not getting much from the salsa flamegraphs on codspeed.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2388 on 2025-04-22 16:51_

You could consider changing the `semantic_checker` field on the `SemanticIndexBuilder` to be an `Option<SemanticSyntaxChecker>` and only initialize it with `Some` if the file is open. `with_semantic_checker` is a no-op if the checker is `None`. 

---

_@MichaReiser reviewed on 2025-04-22 16:52_

---

_@MichaReiser reviewed on 2025-04-22 16:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 16:52_

You want to cache it. It's at least as expensive as a hash set lookup

---

_@MichaReiser reviewed on 2025-04-22 16:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:108 on 2025-04-22 16:54_

We should move those fields into their own group (or to the `builder state` group at the top). The fields at the bottom are reserved for fields that match the fields on `SemanticIndex`

---

_@ntBre reviewed on 2025-04-22 16:55_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 16:55_

Is this the caching you would do? I was confused why this didn't fix the perf regression: https://github.com/astral-sh/ruff/pull/17463/commits/5e172cc80125f788b93a826be2553ccb712b21cb.

I'll try the `Option` suggestion below, but I'm afraid it might cause the same issue if I do the check here again.

---

_@MichaReiser reviewed on 2025-04-22 17:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2271 on 2025-04-22 17:00_

I think the probleme here is more that you can't name the closure type when declaring `SemanticIndexBuilder`. You can try to use `OnceCell` instead

---

_@MichaReiser reviewed on 2025-04-22 17:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 17:08_

The regression went away, no? I no longer see the 33% regression. A 1% regresssion doesn't sound that unreasonable to me

---

_@ntBre reviewed on 2025-04-22 17:11_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 17:11_

It went away after I reverted my first two attempts and moved the check into `report_semantic_error`. The benchmark just finished on the `Option` case and it seems to have brought back the regression. Do you have an intuition why calling `is_file_open` in `SemanticIndexBuilder::new` is such a disaster?

---

_@MichaReiser reviewed on 2025-04-22 17:26_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 17:26_

Not really. `is_file_open` is a "cheap" hash set lookup in the benchmark case 

https://github.com/astral-sh/ruff/blob/9c47b6dbb0f29369fdf38d5048276994b2e09e49/crates/red_knot_project/src/lib.rs#L316-L324

It also doesn't introduce any new query dependencies that would be expensive to validate.

I can try to debug this tomorrow.



---

_@ntBre reviewed on 2025-04-22 17:29_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 17:29_

Thanks, and sorry for the trouble. I'll push my other changes, I think everything else should be resolved.

---

_@MichaReiser reviewed on 2025-04-22 17:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-22 17:53_

Okay. I think I know what's happening but I don't have a solution for this yet. 

Salsa assigns values to different durabilities. This allows it to short-circuit some expensive checks if nothing of that durability has changed. In our case, we use a *higher* durability for files from the standard library because they can never change (all files in typeshed are shipped as part of the red knot binary). 

That means, the `semantic_index` query for files from typeshed currently have a high durability. However, this is no longer the case if we call `is_file_open` because it reads `open_files` which has a low durability -> Making the entire query having a low durability. 

The easiest fix is to change `is_file_open` to always return `false` if `file.path(db).is_vendored()`. However, this still has the downside that it reduces the durability of `semantic_index` for all third-party files to `low`. 

I've to think a bit more about what the proper fix is here. Maybe going back to when you emit the diagnostic isn't that bad after all.

---

_Review request for @carljm removed by @carljm on 2025-04-23 05:46_

---

_@MichaReiser reviewed on 2025-04-23 06:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-23 06:44_

I went ahead and created an issue for this "downgrading" problem as this might is also a problem for type inference diagnostics. To unblock this PR, I suggest to go back to your implementation where you lazily check if the file is open before reporting the diagnostic.

---

_@ntBre reviewed on 2025-04-23 13:26_

---

_Review comment by @ntBre on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2346 on 2025-04-23 13:26_

Thanks for tracking down the issue! I've reverted to having the check in `report_semantic_error`.

I'll make sure the benchmarks pass this time and then get ready to merge, if that's good with you!

---

_Merged by @ntBre on 2025-04-23 13:52_

---

_Closed by @ntBre on 2025-04-23 13:52_

---

_Branch deleted on 2025-04-23 13:53_

---
