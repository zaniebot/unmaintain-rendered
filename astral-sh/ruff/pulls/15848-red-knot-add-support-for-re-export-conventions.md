```yaml
number: 15848
title: "[red-knot] Add support for re-export conventions for imports"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
draft: true
base: main
head: dhruv/re-export
created_at: 2025-01-31T12:14:33Z
updated_at: 2025-05-07T15:22:53Z
url: https://github.com/astral-sh/ruff/pull/15848
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Add support for re-export conventions for imports

---

_Pull request opened by @dhruvmanila on 2025-01-31 12:14_

## Summary

This PR adds support for re-export conventions for imports.

The implementation differs for stub files and runtime files as follows:
* For stub files, this is enforced by using the `unresolved-import` and the inferred type is `Unknown` by way of making the symbol is unbound
* For runtime files, this is enforced by using a new rule `implicit-reexport` and the type is inferred as usual

The current implementation updates the `Symbol::Type` variant to include a new `ReExport` enum which specifies whether the symbol is defined in an import statement and if so if it's being implicitly or explicitly re-exported. This is then used downstream to perform the check.

This implementation does not yet support `__all__` and `*` imports as both are features that needs to be implemented independently.

### Other potential solution

While implementing this, another potential solution would be to use the visibility qualifier for a symbol to specify whether this is an implicitly exported symbol which translates to:
```rs
enum Visibility {
    Public,
    Private,
    ImplicitExport,
}
```

This would then mean that this enum could be created for the `Definition` while building the semantic index and it could also consider the `__all__` values. One issue that I saw with this is that the values in `__all__` could come from another module which isn't possible to do in the semantic index builder because that does not cross the file boundary.

closes: astral-sh/ruff#14099
closes: astral-sh/ruff#15476 

## Test Plan

Add test cases, update existing ones if required.


---

_Label `red-knot` added by @dhruvmanila on 2025-01-31 12:14_

---

_Comment by @codspeed-hq[bot] on 2025-01-31 12:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fre-export)

### Merging astral-sh/ruff#15848 will **not alter performance**

<sub>Comparing <code>dhruv/re-export</code> (3f92a06) with <code>main</code> (1f7a29d)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Marked ready for review by @dhruvmanila on 2025-02-04 12:31_

---

_Review requested from @carljm by @dhruvmanila on 2025-02-04 12:31_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-04 12:31_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-04 12:31_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-02-04 12:31_

---

_Comment by @MichaReiser on 2025-02-04 13:07_

Hmm, it's not obvious to me why we see a 4% regression in the incremental benchmark. It either suggests that we now create more ingredients (which I didn't see) or that some queries are invalidated more often (which they shouldn't, because the benchmark only inserts a comment). Did you look into where we're spending more time? 

---

_Comment by @dhruvmanila on 2025-02-04 13:11_

> Hmm, it's not obvious to me why we see a 4% regression in the incremental benchmark. It either suggests that we now create more ingredients (which I didn't see) or that some queries are invalidated more often (which they shouldn't, because the benchmark only inserts a comment). Did you look into where we're spending more time?

Oh, I missed that completely as I didn't see the comment being updated and I didn't expect this change to regress. Let me look into it. Thanks for catching that.

Edit: I think I've a hunch on where it might be but need to confirm. The `Symbol` type now includes one additional information for the `Type` variant. `Symbol` is the return type of various salsa queries so that might be it but as said need to confirm.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:19 on 2025-02-04 16:37_

Nit: it would be better to quote the typing spec rather than PEP 484. PEP 484 is now a historical document, the typing spec is authoritative.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:178 on 2025-02-04 16:39_

I didn't actually realize this was the rule! I assume you verified that this is the way pyright/mypy implement it?

It looks like this is another reason to refer to the typing spec instead of PEP 484, because the text quoted from PEP 484 above would not suggest this aspect of the rule.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:219 on 2025-02-04 16:40_

Would it be clearer if our test for `__all__` didn't also exercise the subtle same-name wrinkle? I.e. this could just be `from b import Foo` instead (and rename `AnyFoo` to `Foo` below)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:230 on 2025-02-04 16:45_

I thought we already support rule selection? See #15645

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:259 on 2025-02-04 17:13_

```suggestion
Red knot does not special case `__init__.py` files, so if a symbol is imported in `__init__.py`
without an explicit re-export, it should raise an error.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:25 on 2025-02-04 17:40_

Why do we need both `Explicit` and `None`? I don't think there is any difference in behavior between an import statement that _is_ an explicit re-export, and some other definition that isn't an import at all. They are both equally available for import from another module. The only case that needs to be distinguished, as far as I can see, is what you call `ReExport::Implicit` -- an import that is not an explicit re-export. (And per discussion above, I don't think `ReExport::Implicit` is the best name for that.)

There might be a different reason for this to be tri-valued though; I think we might need a `Possibly_` variant, similar to `Boundness::PossiblyUnbound`, for cases where a symbol might be defined normally (by which I mean, either explicit re-export or some other kind of definition), or might be not re-exported in another path. E.g. a case like this:

```pyi
import foo, get_bool

if get_bool():
    foo = "bar"
```

If we have a stub file defined like the above, then I think importing `foo` from that stub file should act exactly like importing a possibly-unbound name (that is, emit `possibly-unbound-import`). If it's a non-stub file, I think we should still raise the `implicit-reexport` diagnostic.

In the stub file case, the possibly harder part is that the type from `import foo` should ideally be ignored, and the inferred type would just be `Literal['bar']`. I think this might require changes in `symbol_from_bindings`?

I think we should add some tests for these conditionally-defined cases.

And it seems like maybe the variants for this enum should be `Yes`, `No`, `Maybe`? (Where `No` would be equivalent to what you currently name `Implicit`, `Yes` would be equivalent to either `Explicit` or `None`, and `Maybe` would be for the conditional cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:14 on 2025-02-04 17:43_

```suggestion
re-exported.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:34 on 2025-02-04 17:43_

```suggestion
Similarly, trying to import the symbols from the builtins module which aren't re-exported
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:54 on 2025-02-04 17:44_

```suggestion
When a symbol is re-exported, imporing it should not raise an error.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:80 on 2025-02-04 17:46_

I think we should be careful about the phrasing "implicitly re-exported". That phrasing suggests that the symbol _is_ re-exported, but "implicitly". This is not really accurate -- if the symbol were re-exported, it would be available for import. The point is that it's not re-exported.

I realize that mypy uses `--no-implicit-reexport` for enabling this rule, and I think it's reasonable to name our rule similar. In the context of a rule name, it's effectively saying "you are treating this symbol as if it were implicitly re-exported, but it's actually not re-exported" -- I think it works in that context. 

```suggestion
## Non-exported symbols in stub files
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:107 on 2025-02-04 17:46_

```suggestion
## Nested non-exports
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:109 on 2025-02-04 17:46_

```suggestion
Here, a chain of modules all don't re-export an import.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:144 on 2025-02-04 17:47_

```suggestion
## Nested mixed re-export and not
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:147 on 2025-02-04 17:47_

```suggestion
raise an error at that step in the chain.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:688 on 2025-02-04 17:48_

```suggestion
    /// Checks for import statements that import names from a module which imports but does not explicitly
    /// re-export them.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:691 on 2025-02-04 17:51_

```suggestion
    /// Imports may only be intended for local use, not intended for re-export as part of the public API of a module.
    /// Allowing other modules to import names that weren't intended for re-export can create inconsistent import
    /// locations for the same symbol across the codebase, cause breakages if an import is removed, and make
    /// future refactors more difficult.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:720 on 2025-02-04 17:55_

```suggestion
        summary: "detects imports of imported names that are not explicitly re-exported",
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:337 on 2025-02-04 17:58_

```suggestion
Similarly, for an `__init__.pyi` (stub) file, importing a non-exported name should raise an error but the
```

---

_@carljm reviewed on 2025-02-04 18:22_

Thank you for adjusting here as we figured out on the fly what the right semantics should be! The difference in behavior between the stub and non-stub case definitely made this more complex than I'd initially realized. In hindsight we could have postponed the non-stub opt-in rule entirely, but I think we would have wanted it eventually, so this way it forces us to consider it in the design.

This first review pass is mostly just about the tests and the semantics; will do a more detailed review of the code changes once we have that stuff nailed down.

---

_@AlexWaygood reviewed on 2025-02-04 18:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:178 on 2025-02-04 18:23_

This is definitely correct, yes -- mypy and pyright do implement it this way. It needs to be a truly "redundant" alias for it to be considered an explicit re-export.

---

_@AlexWaygood reviewed on 2025-02-04 18:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:178 on 2025-02-04 18:25_

> It looks like this is another reason to refer to the typing spec instead of PEP 484, because the text quoted from PEP 484 above would not suggest this aspect of the rule.

PEP 484 is actually very clear on this specific point, FWIW. The sentence after the one Dhruv quotes above is:

> (_UPDATE_: To clarify, the intention here is that only names imported using the form X as X will be exported, i.e. the name before and after as must be the same.)

https://peps.python.org/pep-0484/#stub-files

---

_@dhruvmanila reviewed on 2025-02-05 04:37_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:19 on 2025-02-05 04:37_

I think I'll remove this part then as I've provided a reference link to the typing spec section related to this.

---

_@dhruvmanila reviewed on 2025-02-05 04:38_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:219 on 2025-02-05 04:38_

Yes, that makes sense.

---

_@dhruvmanila reviewed on 2025-02-05 04:40_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:230 on 2025-02-05 04:40_

Oh, I think I misunderstood that PR (misread the first sentence). I'll update this PR accordingly then:
* Remove `implicit-reexport` from the default rule set
* Enable it on specific test cases where it's required

---

_@dhruvmanila reviewed on 2025-02-05 04:42_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:230 on 2025-02-05 04:42_

I think I'll need to add support for it in the test config. I'll do that in a separate PR.

---

_@dhruvmanila reviewed on 2025-02-05 05:15_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:80 on 2025-02-05 05:15_

Thanks, I think that makes sense.

---

_@dhruvmanila reviewed on 2025-02-05 05:18_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:691 on 2025-02-05 05:18_

Thanks, I was planning to do this today :)

---

_Comment by @dhruvmanila on 2025-02-05 05:29_

**Todo:**
- [x] Address Carl's review (https://github.com/astral-sh/ruff/pull/15848#discussion_r1941614832)
- [ ] Check if the incremental regression still persists and figure out why that's happening - Is the regression valid? Could it be addressed?

---

_@dhruvmanila reviewed on 2025-02-05 09:23_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/symbol.rs`:25 on 2025-02-05 09:23_

> In the stub file case, the possibly harder part is that the type from `import foo` should ideally be ignored, and the inferred type would just be `Literal['bar']`. I think this might require changes in `symbol_from_bindings`?

One challenge with this is that in `symbol_by_id` salsa query, if we were able to find the symbol using only declarations, we'd return that symbol. That would be the case in this scenario as `import foo` would provide a bound symbol, thus we would never consider the bindings:

https://github.com/astral-sh/ruff/blob/d757fc071b9d8fe914282bf73704f759f26bfbb4/crates/red_knot_python_semantic/src/types.rs#L128-L130

It might be related to #14297 reading the TODO mentioned in that issue.

---

_@AlexWaygood reviewed on 2025-02-05 14:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-02-05 14:05_

did you consider encoding the information on whether or not the type is a re-export in the `qualifiers` field? It feels like "metadata carried along with the type" in a similar way to the way that whether a symbol is a class variable or not is additional metadata that we carry around with the type of the symbol. We could possibly even rename `TypeQualifiers` to `TypeMetadata`, and add documentation saying that the metadata we record includes (but is not limited to) type qualifiers such as `ClassVar`?

I suppose the `ReExport::Maybe` variant means that we'd have to have two separate flags on the `TypeQualifiers` struct: `MAYBE_REEXPORT` and `DEFINITE_REEXPORT`.

Curious what @carljm thinks of this idea

---

_@dhruvmanila reviewed on 2025-02-06 05:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-02-06 05:48_

I like that idea and it seems more prominent now because for the binding, the `Type` would also need to contain the re-export metadata. My only concern with this would be that the metadata structure would be shared for both declared and binding type so there's a chance that we could mistakenly set metadata for binding type while it's only relevant for a declared type. For example, setting the `Final` qualifier. Any thoughts on this? If it's not a big concern then I might prefer to go this route.

---

_@carljm reviewed on 2025-02-06 18:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:25 on 2025-02-06 18:01_

Hmm, yeah. That sample code I wrote is actually not valid, as we emit a diagnostic on `foo = "bar"` assignment, since it violates the import "declaration". So I think it is actually OK that we ignore the invalid binding in this case. But if it were `foo: str = "bar"`, then I think my original point still holds; we'd want this to be a possibly-unbound symbol of type `str`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-02-06 18:15_

The feeling I've had in looking at this PR is that this should be metadata on a `Definition` (possibly actually just on the `Import` and `ImportFrom` variants of `DefinitionKind`, which might save some space if those are not currently the largest variants). Then it would be handled in `symbol_from_bindings` and `symbol_from_declarations` (where we'd ignore non-re-exported definitions if querying a public global symbol from a stub file). In general, I think we should be looking for ways for this metadata to spread _less_ widely, not more widely, because the cases where it is relevant are limited. I don't think it needs to travel with Types in general, and I think ideally code in `TypeInferenceBuilder` would be totally unaffected by this change.

If we were just implementing the stub-file semantics this approach would be pretty simple, I think. The trickier part is the `implicit-reexport` rule, where we do want the type from the definition, plus some metadata that allows us to emit a diagnostic. To do this, we need `symbol_from_bindings`, `symbol_from_declarations`, and `symbol` to be able to pass back the metadata via `Symbol` that "this type came from, or maybe came from, a not-re-exported definition", and then we do at least need the import handling code in `TypeInferenceBuilder` to inspect that metadata and emit a diagnostic. 

I'm very tempted to just consider the opt-in `implicit-reexport` rule out of scope for now, because it's adding a lot of complexity here, and I think the much more important thing right now is to fix the `builtins.pyi` problem. The ability to also opt-in to this rule in diagnostic form for non-stub files is nice to have, but not high priority right now.


---

_@carljm reviewed on 2025-02-06 18:15_

---

_@AlexWaygood reviewed on 2025-02-06 18:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-02-06 18:17_

Happy to go with Carl's suggestion here; he's looked at this more closely than I have and it sounds very reasonable!

---

_@dhruvmanila reviewed on 2025-02-07 12:34_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-02-07 12:34_

> The feeling I've had in looking at this PR is that this should be metadata on a `Definition` (possibly actually just on the `Import` and `ImportFrom` variants of `DefinitionKind`, which might save some space if those are not currently the largest variants). Then it would be handled in `symbol_from_bindings` and `symbol_from_declarations` (where we'd ignore non-re-exported definitions if querying a public global symbol from a stub file). In general, I think we should be looking for ways for this metadata to spread _less_ widely, not more widely, because the cases where it is relevant are limited. I don't think it needs to travel with Types in general, and I think ideally code in `TypeInferenceBuilder` would be totally unaffected by this change.

I looked into implementing based on this comment, I originally thought of this as well but I didn’t pursue this because of the requirement of inferring the types.

To give context, the plan is to define a `is_reexported` field on both `ImportDefinitionKind` and `ImportFromDefinitionKind` and use that as `Definition::is_reeexported` which would default to `true` for non-import definitions.

This will be then used in `symbol_from_declarations` and `symbol_from_bindings` to filter out the definitions. This creates a problem as both functions does not know whether the symbol is coming from the current module or from another module. This means that for the following code (file `other.pyi`):

```python
from typing import Any

# error: Name `Any` used when not defined
reveal_type(Any)
```

The reason being that while looking up the `Any` symbol in the `reaveal_type` call, we’ll first encounter the import definition which doesn’t re-export the symbol but the `symbol_from_*` does not know that this symbol is coming from the current module itself. We’ll need to pass additional info for the `symbol_from_*` functions to only consider the re-exports from another module.

Implementation diff: https://github.com/astral-sh/ruff/compare/dhruv/re-export-2?expand=1

---

Given the above context, I took a stab at addressing some of the short comings for conditional re-exports via the existing implementation and I think I've addressed Carl's feedback from https://github.com/astral-sh/ruff/pull/15848#discussion_r1941614832.

I've added four more examples to test conditional re-exports.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conditional.md`:98 on 2025-02-07 17:38_

can we add a TODO here? once we are able to properly make `implicit-reexport` rule disabled by default, I would rather this test not use this explicit re-export syntax, it distracts from the point of the test

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conditional.md`:128 on 2025-02-07 17:38_

same here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2601 on 2025-02-07 17:49_

> We’ll need to pass additional info for the symbol_from_* functions to only consider the re-exports from another module.

Yes, I meant to imply that with "if querying a public global symbol from a stub file", but I should have been more explicit. Adding this extra context to `symbol` and friends doesn't bother me because I think we'll need it anyway in order to resolve #15777 -- our current conflation of "type as seen by import" and "type as seen by use from nested scope" is not going to last. We probably need a function specifically for getting an imported symbol from a different file.

I still think it would be better to switch to this approach and add this additional context. I don't like how the re-export metadata in the current version of the diff infects everywhere we use `Symbol`, even though it is relevant in so few places, and I suspect this is also related to the regression.

I think it would actually be better to separate the stub-file unconditional behavior from the `implicit-reexport` rule as separate PRs. One small reason is just that it would be nice to add the `implicit-reexport` rule once we already have full support for making it opt-in, to avoid TODO churn in other tests. It is worth considering how we can implement the opt-in rule on top of the second approach, but I think the plan would be fairly clear: the function that implements getting a symbol's type for import would return a wrapper around `Symbol` that also includes the re-exported metadata, so the importing code can issue the diagnostic if necessary.

---

_@carljm reviewed on 2025-02-07 18:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:378 on 2025-02-07 18:09_

This should be just `str`; the `Foo` class type should not be visible here at all. I think this behavior change will naturally fall out of the alternate implementation strategy.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/conventions.md`:368 on 2025-02-07 18:10_

These new tests look great! Modulo one comment below

---

_@carljm reviewed on 2025-02-07 18:10_

---

_Converted to draft by @dhruvmanila on 2025-02-11 04:46_

---

_Comment by @dhruvmanila on 2025-02-11 07:51_

Closing in favor of astral-sh/ruff#16073 

---

_Closed by @dhruvmanila on 2025-02-11 07:51_

---

_Branch deleted on 2025-04-30 15:47_

---
