```yaml
number: 17286
title: "[red-knot] Improve handling of visibility constraints in external modules when resolving `*` imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/star-statically-known-2
created_at: 2025-04-07T20:21:41Z
updated_at: 2025-04-09T14:41:04Z
url: https://github.com/astral-sh/ruff/pull/17286
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Improve handling of visibility constraints in external modules when resolving `*` imports

---

_Pull request opened by @AlexWaygood on 2025-04-07 20:21_

## Summary

Further work towards https://github.com/astral-sh/ruff/issues/14169.

This adjusts our handling of `*` imports so that we pay attention to visibility constraints in the external module (the module symbols are being imported from) when we infer the boundness of symbols defined via a `*` import. I.e., for something like this:

````md
```toml
python-version = "3.9"
```

`a.py`:

```py
import sys

if sys.version_info >= (3, 10):
    X = True
```

`b.py`:

```py
from a import *

print(X)
```
````

We will now correctly emit an `[unresolved-reference]` diagnostic on the use of `X` in `b.py`: the visibility constraints in `a.py` resolve to `Truthiness::AlwaysFalse`, so no symbols are actually bound in `b.py` as a result of the `*` import.

## Implementation

When adding each `*`-import definition to the `UseDefMap`, we snapshot the `SymbolState` as it was prior to the `*` import definition (possibly) overriding any (possible) previous definitions. Then, in `symbol::symbol_from_bindings_impl`, we evaluate `*` import definitions to determine the boundness of the symbol in the external module. If the symbol is in fact unbound in the external module (due to visibility constraints resolving to `Truthiness::AlwaysFalse`), we determine that the `*` import did not in fact provide an additional binding for this symbol, and in the importing module we fall back to the `SymbolState` for that symbol prior to the `*`-import definition.

In order to be able to resolve `*` imports from `symbol.rs` as well as from `types/infer.rs`, several functions are pulled out of `infer.rs` into a new `import_resolution` module where they can be accessed from both `types/infer.rs` and `symbol.rs`.

## Problems with this approach

This approach works well for symbols in the external module that are statically known to either be definitely bound or definitely unbound. But the logic in `symbol.rs` is still somewhat naive, and doesn't work for possibly-unbound definitions in the external module. I've added a failing test in this PR to illustrate the issue: I don't know how to fix it properly, and would welcome ideas. In addition to the new failing test, the PR does not address any of these TODOs, and ideally it would:

https://github.com/astral-sh/ruff/blob/761749cb505f42b684e72f45b008402edc5a277b/crates/red_knot_python_semantic/resources/mdtest/import/star.md?plain=1#L217-L227

## Test Plan

Existing assertions in mdtests updated; new ones added.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-07 20:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-07 20:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-07 20:21_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-07 20:21_

---

_Comment by @github-actions[bot] on 2025-04-07 20:23_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-04-07 20:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:699 on 2025-04-07 20:30_

calling `.next()` here and only considering the _first_ definition of the definitions-before-the-`*`-import is just... obviously incorrect :-)

but I'm not sure what the correct solution is here

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/import_resolution.rs`:15 on 2025-04-07 20:36_

we could possibly add some caching to this function, since it's nontrivial and it's called from both `symbol.rs` and `infer.rs`?

There's a 1% slowdown on this PR according to codspeed, which isn't huge but also isn't nothing

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/import_resolution.rs`:71 on 2025-04-07 20:36_

not sure if we want the tracing every time the function is called or only when this function is called from `infer.rs`? I'm guessing probably the latter?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:316 on 2025-04-07 20:38_

deriving `salsa::Update` was required here or the compiler started giving me odd lifetime complaints due to the new `bindings_at_star_imports: FxHashMap<Definition<'db>, SymbolState>` fields on `UseDefMap` and `UseDefMapBuilder`

---

_@AlexWaygood reviewed on 2025-04-07 20:38_

---

_@MichaReiser reviewed on 2025-04-07 21:00_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/import_resolution.rs`:71 on 2025-04-07 21:00_

Debug is also user facing (if that helps making a decision)

---

_@carljm reviewed on 2025-04-08 03:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:699 on 2025-04-08 03:16_

I think the simplest approach here might be to make a recursive `symbol_from_bindings_impl` call here with all the prior bindings and constraints. This will give you the type and boundness at the point of the star import definition (if unbound just return `None` and go on to the next binding.) The type will then replace `binding_ty` below, and possibly-unbound will mean the overall symbol resolution also has to be possibly-unbound.

The alternative would be to flatten all these prior bindings into the current loop somehow, with the visibility and narrowing constraints of the star-import binding ANDed with each one of the prior bindings. This is probably doable, but seems more complex?

---

_@carljm reviewed on 2025-04-08 03:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:695 on 2025-04-08 03:17_

If the star-import result is possibly-unbound, then you would need to union the type from the star import resolution with the type obtained from the prior bindings (if you take the first option below) or keep both the star-import binding and the prior bindings in this loop (if you take the flatten option below).

---

_@carljm reviewed on 2025-04-08 03:18_

This is looking pretty good! Didn't do a full review, just commented on the main open issues I saw.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/import_resolution.rs`:33 on 2025-04-08 06:11_

I think the comment is outdated. The code here no longer emits a diagnostic

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/symbol.rs`:366 on 2025-04-08 06:14_

Nit: Can we align the argument ordering. `symbol_from_bindings` takes `scope` as second argument before `bindings` and `symbol_from_bindings_impl` takes `scope` as the last argument. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/import_resolution.rs`:15 on 2025-04-08 06:18_

I don't think this method on its own performs that much computation that it's worth caching because most its inner methods are cached (mainly `resolve_module`) and I assume it isn't called that frequently (only for star imports?)

I suspect that the perf regression is mainly due to us inferring more types. You could run the CLI in `-vvv` and see if the ingredient counts change.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/import_resolution.rs`:71 on 2025-04-08 06:20_

It would be nice if the tracing calls would be implicit from the API (where infer uses one API and the other call site the other). E.g. could `UnresolvedImportError` wrap `ModuleNameResolutionError`? Or we return a type that allows converting from the error returned here to `UnresolvedImportError` where it has two methods. 

Or just add a boolean flag 😆 

---

_@MichaReiser reviewed on 2025-04-08 06:20_

---

_@sharkdp reviewed on 2025-04-08 07:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:576 on 2025-04-08 07:49_

I think you probably want to reference https://github.com/astral-sh/ruff/issues/15797 here? In fact, this error might be gone if you rebase on main?

---

_@sharkdp reviewed on 2025-04-08 07:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:605 on 2025-04-08 07:52_

… also known in the literature as a desperate `*` import.

---

_@sharkdp reviewed on 2025-04-08 07:57_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:611 on 2025-04-08 07:57_

Is this what you referred to in the PR description considering possibly-unbound imports?

---

_@AlexWaygood reviewed on 2025-04-08 08:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:605 on 2025-04-08 08:18_

it's actually morse code for "help me, I'm stuck in a type checker and cannot get out" if you apply the right encoding to the file

---

_@AlexWaygood reviewed on 2025-04-08 08:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:611 on 2025-04-08 08:20_

Yup

---

_@sharkdp reviewed on 2025-04-08 08:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:699 on 2025-04-08 08:48_

A test case to capture this would be

`module.py`:

```py
if False:
    X = 3
```

`main.py`:
```py
def flag() -> bool:
    return False

if flag():
    X = 1
else:
    X = 2

from module import *

reveal_type(X)  # Should be Literal[1, 2], currently results in Literal[1]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:576 on 2025-04-08 09:24_

> I think you probably want to reference https://github.com/astral-sh/ruff/issues/15797 here?

oops, yes, thanks!

> In fact, this error might be gone if you rebase on main?

Indeed it is! It looks like we still infer `Unknown` rather than `Never`, though. That might be okay? Not sure -- WDYT?

---

_@AlexWaygood reviewed on 2025-04-08 09:24_

---

_@sharkdp reviewed on 2025-04-08 09:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/symbol.rs`:699 on 2025-04-08 09:36_

> I think the simplest approach here might be to make a recursive `symbol_from_bindings_impl` call here with all the prior bindings and constraints. This will give you the type and boundness at the point of the star import definition (if unbound just return `None` and go on to the next binding.) The type will then replace `binding_ty` below, and possibly-unbound will mean the overall symbol resolution also has to be possibly-unbound.
> 
> The alternative would be to flatten all these prior bindings into the current loop somehow, with the visibility and narrowing constraints of the star-import binding ANDed with each one of the prior bindings. This is probably doable, but seems more complex?

The way I believe this could be implemented is actually a third approach, I think?

The main problem (on `main`) seems to be that in a situation like the following, we consider `X` to be definitely overwritten by the star-import:
```py
X = 1

from module import *  # may define X
```
This is why you implemented the approach with storing the previous symbol state at the position of the star import, attempting to reinstate it if `X` is, in fact, not defined in `module` (given the evaluation of visibility constraints in `module`).

What if we did the following instead: we treat star-imports similar to a conditional branch. With the difference that we need to fill in the condition at a later point in time. Conceptually, we would replace `from module import *` with
```py
if <placeholder>:
    X = module.X
```
Instead of unconditionally overwriting the previous binding of `X`, we would keep the previous binding with a visibility constraint of `~<placeholder>`, and add the new binding "`X = module.X`" binding with a visibility constraint of `<placeholder>`.

During type inference, we would replace this placeholder with `AlwaysTrue` if `X` is definitely bound in `module`, with `AlwaysFalse` if `X` is definitely unbound in `module`, and with `Ambiguous` if `X` is possibly-bound in `module`. This replacement would be a TDD traversal, and the placeholders would need to include some information to which star-import they belong.

The advantage of this approach, I think, would be that we would not need the new `bindings_at_star_imports` field in the use-def map. And after the placeholders have been replaced, `symbol_from_bindings` should ideally just work as before.

I think it would be similar to Carl's flattening suggestion, but the actual operation would be performed by the usual visibility constraints merging in the semantic index builder (those placeholder constraints could be AND-ed and OR-ed with other constraints, it would just be a new atomic constraint).

I hope this all makes sense, I'm unfortunately not that familiar with your previous star-import work.

---

_@sharkdp reviewed on 2025-04-08 09:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:576 on 2025-04-08 09:37_

> Indeed it is! It looks like we still infer `Unknown` rather than `Never`, though. That might be okay? Not sure -- WDYT?

Yes, this is expected.

---

_Comment by @AlexWaygood on 2025-04-08 10:02_

Quite apart from any of the big things I was stuck on (thank you all for your review comments!), I realised after sleeping on it that I could significantly simplify things by reviving the idea I had previously in https://github.com/astral-sh/ruff/pull/16923#discussion_r2009436905. There's now no need for moving any logic from `infer.rs` into a new `import_resolution` submodule, and all the questions about "Where do we do tracing?" are now redundant 🥳

---

_@AlexWaygood reviewed on 2025-04-08 10:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:699 on 2025-04-08 10:15_

This is a really interesting idea, and potentially much more elegant than the solution Carl and I had been working on! I'm going to try it out.

---

_@carljm reviewed on 2025-04-08 14:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:699 on 2025-04-08 14:17_

Yes, that idea does sound very tidy! I think it will require an enum wrapper around `Predicate`(or similar)  in order to allow for these placeholder predicates?

---

_Comment by @codspeed-hq[bot] on 2025-04-08 15:03_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fstar-statically-known-2)

### Merging #17286 will **degrade performances by 12.49%**

<sub>Comparing <code>alex/star-statically-known-2</code> (9415521) with <code>main</code> (f1ba596)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fstar-statically-known-2)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[cold] `` | 96.7 ms | 110.5 ms | -12.49% |


---

_Comment by @AlexWaygood on 2025-04-08 15:15_

I tried out @sharkdp's suggested alternative implementation, and it appears to work beautifully... except that this PR is now triggering(?) a Salsa panic in some unrelated tests in a file I didn't touch:

```
  crates/red_knot_python_semantic/resources/mdtest/properties.md:203 panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/296a8c7/src/tracked_struct.rs:832:21
  crates/red_knot_python_semantic/resources/mdtest/properties.md:203 access to field whilst the value is being initialized
```

which looks like the same Salsa panic that @dcreager was running into in https://github.com/astral-sh/ruff/pull/17023 before he applied https://github.com/astral-sh/ruff/pull/17023/commits/ea125488ac59d27620144dafcced3050c1da2775 to that PR...

---

_@AlexWaygood reviewed on 2025-04-08 15:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/properties.md`:1 on 2025-04-08 15:40_

All changes to this file are just commenting out the test that causes the Salsa panic (I just applied @dcreager's commit https://github.com/astral-sh/ruff/commit/ea125488ac59d27620144dafcced3050c1da2775 to this branch 😆)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/predicate.rs`:144 on 2025-04-08 16:11_

This is a scoped symbol ID, meaning it is only valid in some specific scope, and can give "random" wrong results if interpreted as a symbol ID in some other scope.

It's made clear in a comment below that the scope is implicitly always "global scope of importing_file", but I think it would be worth mentioning that in a doc comment here as well.

---

_@carljm approved on 2025-04-08 16:34_

This is so nice and clean now! Love it.

I think given our upcoming deadlines I would be inclined to merge this even with a significant regression (especially if that regression gets smaller percentage-wise when we check a larger codebase), and prefer to have someone spend dedicated time later looking for performance wins. But it wouldn't hurt to at least look at some profiles (the CodSpeed ones are sadly useless, so they have to be generated locally) to understand which queries we spend more time in now.

---

_@sharkdp reviewed on 2025-04-08 20:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/predicate.rs`:111 on 2025-04-08 20:09_

Minor
```suggestion
/// Since we cannot know whether or not <condition> is true at semantic-index time,
```

---

_@sharkdp reviewed on 2025-04-08 20:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/predicate.rs`:96 on 2025-04-08 20:12_

This is a really minor nuisance, but I would have an easier time reading this doc comment if the modules were named `module.py` and `main.py` or similar, so I don't have to remember which one is the exporting and which one is the importing module :smile:. Sorry, it's late here...

---

_@sharkdp reviewed on 2025-04-08 20:14_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1211 on 2025-04-08 20:14_

Maybe worth mentioning again that we essentially model a
```py
if <placeholder>:
    from <referenced_module> import <symbol>
else:
    # (implicit empty else branch with negated visibility constraints)
```
control flow here.

---

_@sharkdp approved on 2025-04-08 20:17_

Very nice — Thank you!

---

_@carljm reviewed on 2025-04-09 14:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/predicate.rs`:143 on 2025-04-09 14:32_

nit
```suggestion
    /// to the global scope of the importing file.
```

---

_Comment by @AlexWaygood on 2025-04-09 14:33_

> Merging #17286 will **degrade performances by 12.49%**

We discussed this internally. It's obviously far from ideal, but for now we're going to eat the performance regression. We have some ideas for how to address it, but they may or may not work out, and we want to move onto other tasks for now. I'll writeup a separate issue with our findings from looking at the tracing logs and various profiles.

---

_Merged by @AlexWaygood on 2025-04-09 14:36_

---

_Closed by @AlexWaygood on 2025-04-09 14:36_

---

_Branch deleted on 2025-04-09 14:36_

---
