```yaml
number: 19371
title: "[ty] Initial implementation of declaration and definition providers."
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: goto_definition
created_at: 2025-07-15T18:32:31Z
updated_at: 2025-07-18T18:26:19Z
url: https://github.com/astral-sh/ruff/pull/19371
synced_at: 2026-01-12T15:56:37Z
```

# [ty] Initial implementation of declaration and definition providers.

---

_@UnboundVariable_

This PR implements "go to definition" and "go to declaration" functionality for name nodes only. Future PRs will add support for attributes, module names in import statements, keyword argument names, etc.

This PR:
* Registers a declaration and definition request handler for the language server.
* Splits out the `goto_type_definition` into its own module. The `goto` module contains functionality that is common to `goto_type_definition`, `goto_declaration` and `goto_definition`.
* Roughs in a new module `stub_mapping` that is not yet implemented. It will be responsible for mapping a definition in a stub file to its corresponding definition(s) in an implementation (source) file.
* Adds a new IDE support function `definitions_for_name` that collects all of the definitions associated with a name and resolves any imports (recursively) to find the original definitions associated with that name.
* Adds a new `VisibleAncestorsIter` stuct that iterates up the scope hierarchy but skips scopes that are not visible to starting scope.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-15 18:32_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-15 18:32_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-15 18:32_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-15 18:32_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-15 18:32_

---

_Comment by @github-actions[bot] on 2025-07-15 18:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-15 19:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/UnboundVariable%3Agoto_definition?runnerMode=Instrumentation)

### Merging #19371 will **not alter performance**

<sub>Comparing <code>UnboundVariable:goto_definition</code> (c258942) with <code>main</code> (cbe94b0)</sub>



### Summary

`✅ 41` untouched benchmarks  





---

_Comment by @AlexWaygood on 2025-07-15 19:43_

> Merging #19371 will **degrade performances by 5.26%**

I think that's just a race condition due to https://github.com/astral-sh/ruff/pull/19328 being merged at exactly the wrong time -- if you rebase this PR, it should go away

---

_Label `server` added by @AlexWaygood on 2025-07-15 19:43_

---

_Label `ty` added by @AlexWaygood on 2025-07-15 19:43_

---

_Comment by @UnboundVariable on 2025-07-15 20:52_

To potential reviewers... Most of the changes in this PR are straightforward and probably not worth your time reviewing in any great detail. Here are the specific areas where I would most appreciate review and feedback:

1. In [semantic_index.rs](https://github.com/astral-sh/ruff/pull/19371/files#diff-2c8485d2796b0e1e643d3e48cd572c9810dcc7f45bbecae951ca00e5276d7d9d), I added a `VisibleAncestorsIter` struct to walk up the scope hierarchy but skip the scopes that are not visible to the starting scope. I would have expected to see functionality like this already, but I couldn't find it. Let me know if I should be using something else here.
2. I added a function [definitions_for_name](https://github.com/astral-sh/ruff/pull/19371/files#diff-f9eb3d520f255c7ff2246e1250a1ee679eea2e642bef437c28cc36f49e797cee) in `ide_support.rs`. Does this look like the right abstraction and separation of concern? I tried a couple of different options, but some of my other attempts required exposing way more public structs and methods from `ty_python_semantic`  to the `ty_ide` crate. 
3. I added a new module [resolve_definition](https://github.com/astral-sh/ruff/pull/19371/files#diff-940d4c36a4b3d1411e05f62bcbf93457712b47d29e5afc2bcbb751c51286ad1d) that is used by `definitions_for_name`. It contains code that "resolves" import declarations -- following them recursively to locate the original source declaration. Does this look like the right approach?



---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index.rs`:394 on 2025-07-15 23:08_

Class scopes are also visible to an annotations scope immediately enclosed by the class scope, e.g. in a case like:

```py
class C:
    X = int
    def f[T](self, a: T, b: X) -> T: ...
```

We don't already have this iterator because Python's lexical scoping rules (which we currently implement in `TypeInferenceBuilder::infer_name_load`) are more complex than what's implemented here. Even if we don't consider things like narrowing constraints (which aren't relevant for LSP goto), there are other factors, such as `nonlocal` and `global` statements. Once all of that is implemented, I'm not sure if this iterator is a net simplification, or if it just ends up splitting the lexical scoping rules into two different places.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/resolve_definition.rs`:60 on 2025-07-15 23:59_

I think it would be not much more duplicative, and simpler, if we duplicate the `let file = ...; let module = ...` in the import arms below, and eliminate the redundant early return (which we just end up duplicating in an unreachable arm below.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/resolve_definition.rs`:109 on 2025-07-16 00:00_

If we keep the early return above, I think we should use `unreachable!("would have early returned above")` here rather than duplicating.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/resolve_definition.rs`:64 on 2025-07-16 02:27_

This line seems removable?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:457 on 2025-07-16 02:43_

This feels like a concern that ought to be encapsulated inside `resolve_definition`.

Could we eliminate the need for this clause (and get slightly more consistent behavior as well when there are multiple original definitions and some "resolve" and others don't?) by ensuring that `resolve_definition` always just returns a `ResolvedDefinition::Definition` of the original definition (instead of an empty vec) in error cases?

Or alternatively, perhaps there should be a `fn resolve_definitions` in the definition-resolver module that takes an iterator of Definition and returns the resolved definitions, and this logic would then belong there.

---

_Review comment by @carljm on `crates/ty_ide/src/goto.rs`:209 on 2025-07-16 04:10_

Why accept the ExprRef here if it's unused?

---

_Review comment by @carljm on `crates/ty_ide/src/stub_mapping.rs`:12 on 2025-07-16 04:16_

Without any implementation, it's difficult to evaluate whether a `StubMapper` struct makes sense. Will it carry any state (other than a db, which we pass everywhere anyway)? Or will it end up really just being a stateless `map_definition` function?

---

_Review comment by @carljm on `crates/ty_ide/src/goto.rs`:158 on 2025-07-16 04:20_

It's a bit confusing that several comments here reference "declarations", when this method is used for both goto-definition and goto-declaration (depending on whether a stub mapper is provided).

---

_Review comment by @carljm on `crates/ty_ide/src/goto.rs`:147 on 2025-07-16 04:23_

At the moment in this PR, it looks like the only difference between goto-definition and goto-declaration is whether stubs are mapped to source files. Otherwise, both goto-definition and goto-declaration return all reachable bindings and declarations.

Is this the right behavior? Or should there be a distinction between bindings and declarations?

I guess in the most common cases (functions and classes) it likely won't make a difference. But for e.g. a case like

```py
SOME: int
if whatever:
    SOME = 1
else:
    SOME = 2
```

I think I would expect goto-declaration to take me only to `SOME: int`, and goto-definition to take me only to the two assignments with RHS?

Maybe this kind of case isn't common enough for it to matter.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:410 on 2025-07-16 04:31_

This walk doesn't currently handle `nonlocal` and `global` statements, both of which should modify this logic. If a name is declared `nonlocal` then we shouldn't stop the walk when we discover definitions in that scope, because definitions in the nearest containing (function-like) scope also apply. If we see a `global` statement, we should consider definitions in that scope, and then jump straight to global scope.

It would be good to add tests for these cases.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:464 on 2025-07-16 04:54_

Our type inference has a more precise understanding in many cases of exactly which bindings can actually reach a given use. This is true both in the same scope:

```py
def f():
    x = 1
    x<CURSOR>
    x = 2 # should goto-definition from the above line offer this definition as a nav target?
```

And in enclosing scopes, in particular where the nested scope is known to be eagerly executed:

```py
def f():
    x = 1
    [x<CURSOR> for _ in [1]]
    x = 2  # same question as above
```

Is this an intentional choice based on your experience of what behavior users will likely prefer (or what other LSPs do), or is it an expedient choice because that's what was simplest to implement?

If we will eventually want a more precise behavior that eliminates definitions that can't reach the selected use, that implies that we'll end up duplicating much more of the logic found in `TypeInferenceBuilder::infer_name_load`, and could imply a different implementation direction altogether (where we try to reuse more of the type-inference logic.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/resolve_definition.rs`:1 on 2025-07-16 04:58_

I wonder if this should be a submodule of `ide_support.rs`, since I think that's the only use case for this?

---

_@carljm reviewed on 2025-07-16 04:59_

This is great, thank you!

At a high level, my main question mark is the extent to which this adds a new and totally separate reimplementation of logic to model Python semantics which already exists in a different form in our type inference. In this PR, that's mostly `definitions_for_name`, which sort-of-duplicates `TypeInferenceBuilder::infer_name_load`. But as we flesh out e.g. attribute access, I suspect there will be even more of this duplication.

On the one hand, this duplication will likely (and already does, in this PR) lead to some subtleties being modeled correctly in one case and not in another case, leading to inconsistencies between the goto feature and type inference.

On the other hand, that may not be a big deal, and this may still be the best tradeoff. The LSP goto features can ignore some complexities (narrowing constraints) entirely, and (at least currently) make some other major simplifications (always considering all reachable bindings and declarations in every considered scope, even if some could not reach the selected name expression). And the clearest implementation alternative (packaging up the full set of discovered Definitions with every Place load in type inference, so that we can reuse the type-inference code) could have significant performance implications for type inference, even when goto features aren't being used.

Regarding your 3 reviewer questions, I think I answered (1) in an inline comment, (2) is discussed both inline (regarding specific limitations of the new function) and above (regarding the higher-level question of whether we re-implement or re-use the type-inference logic), and re (3) I think the new `ResolvedDefinition` machinery looks reasonable. 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_declaration.rs`:1 on 2025-07-16 08:41_

Splitting the modules as you suggest makes a lot of sense to me. But I suggest doing it in a separate PR. It makes this PR smaller and also makes it easier to see if there were any changes in those modules (and what's pre-existing code)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/stub_mapping.rs`:12 on 2025-07-16 08:43_

I agree. I don't think having the struct in place provides much value in its current form. It also makes it hard to judge if it should be a method on `Definition` instead. 

I suggest removing it for now and instead add a TODO comment where the mapping must happen.

---

_@MichaReiser reviewed on 2025-07-16 09:24_

Thank. I don't have much to add other than I agree with @carljm's summary: It be curious to hear your thoughts on duplicating some of the semantic behavior for the LSP vs trying to enrhich `ty_python_semantic` itself to avoid the code duplication.

---

_Comment by @carljm on 2025-07-16 17:03_

@UnboundVariable and I discussed this today in person and felt that it makes sense to continue with the current approach, even though it involves some of what looks like duplication, because in fact the duplication is "shallower" than it appears; there are a lot of behavior differences between type inference and collecting definitions for goto-definition. (One upcoming example with attributes is that goto-definition actually shouldn't run the descriptor protocol, which is a big part of our type-inference attribute-access code.) Having to integrate all of these differences into the type-inference code as optional flags will increase complexity and slow down implementing the LSP features, as well as making it harder to make future changes to the behavior. In other words, we feel this is a case where it makes sense to accept some duplication in exchange for greater flexibility.

---

_@UnboundVariable reviewed on 2025-07-16 20:46_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/goto.rs`:209 on 2025-07-16 20:46_

Good catch. That was left over from an earlier iteration of the code.

---

_@UnboundVariable reviewed on 2025-07-16 20:49_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/stub_mapping.rs`:12 on 2025-07-16 20:49_

I'd prefer to leave it in place for now. I told Aria that I'd leave hooks in place for the follow-on work that she's going to do. It's easy to remove if she decides to implement it in some other manner, but I think it's likely that this is the preferred pattern.

---

_Merged by @UnboundVariable on 2025-07-16 22:07_

---

_Closed by @UnboundVariable on 2025-07-16 22:07_

---

_Branch deleted on 2025-07-16 22:07_

---

_@carljm reviewed on 2025-07-16 22:47_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:454 on 2025-07-16 22:47_

Do we really want to _skip_ current scope here? I realize that there's a broader TODO above about respecting nonlocal/global writes from arbitrary other scopes (which I think is doable with some non-trivial changes to semantic indexing), but it seems like we could easily (and should) include definitions from the same scope in which it is declared `nonlocal`? (Same question applies re `global` handling above)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:217 on 2025-07-17 07:23_

The `use` here seems unnecessary given that it is used exactly once. We also prefer using global imports in general unless there's a reason not to (e.g. a star import or conflicting trait imports `std:fmt::Write` and `std::io::Write`)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:404 on 2025-07-17 07:35_

This can only fail if `name` doesn't belong to `file` or if `ast::ExprName` is from a previous revision. Id' be inclined to use `expression_scope_id` directly.

Having to clone `ExprName` here seems unnecessary. I suggest either implementing `HasTrackedScope` for `ExprName` or passing `&ExprRef::from(name)` here

---

_@MichaReiser reviewed on 2025-07-17 07:40_

This is great. 

My only concern with duplicating the logic is that it will be very easy to only make the change in one implementation and not in the other. Is there a way that we could run the new logic as part of our mdtests so that at least a test is failing if I only update one implementation but not the other (it would also give us a lot of test coverage to start from and track the remaining TODOs)

---

_@UnboundVariable reviewed on 2025-07-17 21:43_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:454 on 2025-07-17 21:43_

With language server functionality like "go to declaration", it's not clear what behavior is "correct". It's somewhat subjective, and we will undoubtedly need to iterate on all of the language server features in response to user feedback. As you and I discussed in person, my recommended approach is to use Microsoft's language server (pylance) as a starting point to inform the initial behavior. Pylance has already gone through several years of iteration and user feedback, so it's probably a good starting point for ty.

Another general principle with "go to declaration" and "go to definition" is that it's probably not a good idea to return different results for different usages of a symbol. In other words, if you "go to definition" from any use of a symbol, it should display the same list of definitions.

Based on these principles, the "correct" solution here is to address the TODO that I left in the code. Once we do that, we will want to skip the current scope here. In other words, I wrote the code assuming that we will eventually fix the TODO. If you have strong opinions about how this should work in the interim — or prefer that I prioritize fixing the TODO now, let me know.

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/goto.rs`:217 on 2025-07-17 21:46_

Thanks, this was left over from a previous iteration of the code. I'll clean it up in my next PR.

---

_@UnboundVariable reviewed on 2025-07-17 21:46_

---

_@UnboundVariable reviewed on 2025-07-17 21:50_

---

_Review comment by @UnboundVariable on `crates/ty_python_semantic/src/types/ide_support.rs`:404 on 2025-07-17 21:50_

I'll change this in my next PR. 

---

_@carljm reviewed on 2025-07-18 18:26_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/ide_support.rs`:454 on 2025-07-18 18:26_

Understood. Not sure how to weight "prefer always returning the same set of definitions" vs "don't fail to return a local definition that is highly likely to be relevant" in the short term, but I agree the right answer is to fix the TODO and consider all definitions. I think the main thing blocking this is to adjust semantic indexing so it does a "breadth-first" rather than "depth-first" walk (defers walking function bodies), so that we can be confident of the target scope for a `nonlocal` declaration (because we know we've seen all possible bindings in enclosing scopes). But we have plenty to do, so I'm not going to suggest this definitely should be the next priority. It may also make sense for someone more familiar with the existing semantic indexing to tackle that.

---
