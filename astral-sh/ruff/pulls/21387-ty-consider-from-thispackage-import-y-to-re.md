```yaml
number: 21387
title: "[ty] Consider `from thispackage import y` to re-export `y` in `__init__.pyi`"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/abs-re
created_at: 2025-11-11T18:11:20Z
updated_at: 2025-11-11T19:45:31Z
url: https://github.com/astral-sh/ruff/pull/21387
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Consider `from thispackage import y` to re-export `y` in `__init__.pyi`

---

_Pull request opened by @Gankra on 2025-11-11 18:11_

Fixes https://github.com/astral-sh/ty/issues/1487

This one is a true extension of non-standard semantics, and is therefore a certified Hot Take we might conclude is simply a Bad Take (let's see what ecosystem tests say...).

---

_Review requested from @carljm by @Gankra on 2025-11-11 18:11_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-11 18:11_

---

_Review requested from @sharkdp by @Gankra on 2025-11-11 18:11_

---

_Review requested from @dcreager by @Gankra on 2025-11-11 18:11_

---

_Label `ty` added by @Gankra on 2025-11-11 18:11_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-11 18:11_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 18:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 18:14_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/timeout.py:23:20: error[unresolved-attribute] Module `multiprocessing` has no member `context`
- Found 205 diagnostics
+ Found 204 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 18:18_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 1 | 0 |
| **Total** | **0** | **1** | **0** |

**[Full report with detailed diff](https://gankra-abs-re.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-abs-re.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-11 18:19_

Hillarious.

---

_@carljm approved on 2025-11-11 18:24_

Nice! One less false positive in the ecosystem 😆 

---

_Comment by @Gankra on 2025-11-11 18:24_

The one (1) change is this code:

https://gitlab.com/cki-project/cki-lib/blob/fca022d15d1e881f561f3a53da3b19dc6c4cae18/cki_lib/timeout.py#L23

Accesses `multiprocessing.context`, and in typeshed we now think it exists because of:

https://github.com/python/typeshed/blob/5c8b7fcbbeb4af2d7e9f33e745a7863e401c2578/stdlib/multiprocessing/__init__.pyi#L1

```py
from multiprocessing import context, reduction as reducer
```

(Additional context: this stub includes an `__all__` that actually doesn't actually list `context`)

---

_Comment by @Gankra on 2025-11-11 18:26_

I am fine with landing this, but this seems more like an argument that maybe typeshed should be changed here X3

---

_Comment by @carljm on 2025-11-11 18:27_

This is a false positive we are removing: at runtime `multiprocessing.context` is always available, due to https://github.com/python/cpython/blob/main/Lib/multiprocessing/__init__.py#L16

I don't know if sample size of one is a sufficient indication that this is the right heuristic for stub files.

@AlexWaygood what do you think? Should we land this, or should we change `multiprocessing/__init__.pyi` in typeshed to make the re-export more explicit?

---

_Comment by @Gankra on 2025-11-11 18:29_

My two cents is that I actually like how explicit/clear `from . import x` is compared to `from thispackage import x`, and I *think* pyright only supports the former so we're in some sense introducing less fragmentation by not shipping this.

---

_Comment by @AlexWaygood on 2025-11-11 18:33_

> This one is a true extension of non-standard semantics

But so is considering `from . import y` as a stub re-export, right? There doesn't seem to me to be a principled reason to privilege `from . import y` as a stub re-export but not `from thispackage import y`. According to the spec, neither should be considered a stub re-export, and they have the same semantics at runtime, so if we're special-casing one of them I think we should special-case the other too.

On the other hand -- yes, that typeshed import is definitely _not_ intended to be an explicit re-export of the `context` submodule from `multiprocessing/__init__.pyi`. It's true that you can access `.context` from the `multiprocessing` module at runtime, but the `multiprocessing.context` module is entirely undocumented; people are supposed to use the public API of `multiprocessing` from the top-level module rather than use its undocumented submodules. It seems to me like it's an implementation detail of the runtime that the submodule is currently exposed from the `__init__.py` at runtime; I'm not sure users should be depending on that.

> (Additional context: this stub includes an `__all__` that actually doesn't actually list `context`)

If `__all__` exists in the stub, maybe that should just have the final say over which import definitions in stubs are considered re-exported from the stub?

---

_Comment by @Gankra on 2025-11-11 18:43_

Would you also want it to be able to mask out `from x import y as y`? That's a pretty big change...

---

_Comment by @AlexWaygood on 2025-11-11 19:01_

> Would you also want it to be able to mask out `from x import y as y`? That's a pretty big change...

Hmm... the [spec](https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols) says:

> Type checkers can use the following rules to determine which symbols are visible outside of the package.
>
> - Symbols whose names begin with an underscore (but are not dunder names) are considered private.
>
> - Imported symbols are considered private by default. A fixed set of [import forms](https://typing.python.org/en/latest/spec/distributing.html#import-conventions) re-export imported symbols.
>
> - A module can expose an `__all__` symbol at the module level that provides a list of names that are considered part of the interface. This overrides all other rules above, allowing imported symbols or symbols whose names begin with an underscore to be included in the interface.

The `from x import y as y` convention is the second bullet point here -- but the third bullet point (regarding `__all__` states that "this overrides all other rules above". So that would imply that yeah, if `y` doesn't appear in `__all__`, we should regard it as unexported even if it was imported with `from x import y as y`.

But on the other hand, there's literally no reason to use a redundant alias like that unless you want it re-exported. You can't get any more explicit than that. So I think leaving it as it is there would also be fine, even if it would arguably be most consistent to give `__all__` top priority in all cases.

---

_Comment by @carljm on 2025-11-11 19:14_

IMO the typing spec is not super clear about the relationship between the `import x as x` convention and respecting `__all__`, but personally I think the wording implies that they are additive -- if a symbol uses the `x as x` convention OR appears in `__all__`, it is exported. It does say that `__all__` "overrides all rules above" but it never suggests that overriding in a negative direction is even considered as a possibility. (The additive version is also what mypy and pyright actually implement. And I think it's the better choice for users, since `x as x` is so clear and explicit.)

The trouble here is that we are adding some less-explicit re-exports, and we have to decide if we should treat them the same as the explicit `x as x`, or let them be overridden in a negative direction by `__all__`. Personally I don't think this is worth the complexity either in implementation or in mental model for users. We should just decide if we want to consider these forms re-exports, and if we do, then we should treat them the same as the fully-explicit re-exports.

---

_Comment by @AlexWaygood on 2025-11-11 19:22_

> The trouble here is that we are adding some less-explicit re-exports, and we have to decide if we should treat them the same as the explicit `x as x`, or let them be overridden in a negative direction by `__all__`. Personally I don't think this is worth the complexity either in implementation or in mental model for users. We should just decide if we want to consider these forms re-exports, and if we do, then we should treat them the same as the fully-explicit re-exports.

Yeah, that's fair. In that case, I think I favour landing this PR as-is, despite the fact that the `multiprocessing.context` thing is arguably an ecosystem regression. I think it makes our heuristic easier to understand if we consider `from thispackage import y` the same way as we consider `from . import y`, since they do the same thing at runtime.

---

_@AlexWaygood approved on 2025-11-11 19:22_

---

_Comment by @carljm on 2025-11-11 19:26_

The one thing that occurs to me: if we land this and don't let `__all__` override it negatively, would there be any way for the stub author to say "no, actually, this submodule is private."?

If the answer is no, that maybe suggests we shouldn't land this? Because the other direction is always an easy fix for the stub author -- just use `x as x` in the import. (Though I agree that if we don't land this, we should also revert making `from . import sub` a re-export -- and that one seemed clearly positive in the ecosystem. So I guess I don't think we should revert that, which means I do think we should land this regardless, to maintain consistency.)

The other possible thing we could do is ensure the "initial underscore means private" rule does override this heuristic. That would mean that `from thispackage import sub` and `from . import sub`, if they were really meant to not be re-exports, could be rewritten as `from thispackage import sub as _sub` or `from . import sub as _sub` (or actually rename the submodule to begin with underscore.) But I think this is low priority, we can wait and see if we get any user requests for it.

---

_Comment by @Gankra on 2025-11-11 19:32_

I pushed up a docs change to add some more context:

* Unlike `from . import x`, this was done purely for consistency and not for compatibility
* The only way to opt out is with `from . import x as y` (this already worked and is tested)

---

_Comment by @AlexWaygood on 2025-11-11 19:33_

> The one thing that occurs to me: if we land this and don't let `__all__` override it negatively, would there be any way for the stub author to say "no, actually, this submodule is private."?

I don't think so, no. If you need to import it in `__init__.pyi` for a type annotation, there'd be no way of excluding it from the public interface. But distinguishing between `from . import y` and `from thispackage import y` just seems very arbitrary; I think the only reason why one had more ecosystem impact is just that `from thispackage import y` is more characters to type out, so it's a less popular idiom in general in `__init__.pyi` files. It doesn't mean that people are using that syntactic form specifically so that type checkers will recognise the submodule as re-exported. I do think treating them both the same way is the most important thing here: if we want `__all__` to be able to override these re-exports negatively, we should apply that to both `from . import y` and `from thispackage import y`.

> But I think this is low priority, we can wait and see if we get any user requests for it.

Yes, I agree; we can come back to all this later. Nobody's going to complain if the ty beta lets you access `.context` as an attribute on the stdlib `multiprocessing` module, in contravention of the intentions of the typeshed maintainers 😄

---

_Comment by @AlexWaygood on 2025-11-11 19:34_

> The only way to opt out is with `from . import x as y` (this already worked and is tested)

Oh, we _still_ have a nonstandardised syntactic distinction even after this PR? I'm not sure I like that either 😆

---

_Comment by @Gankra on 2025-11-11 19:35_

Oh to be clear `from . import x as y` and `from thispackage import x as y` *both* opt-out of the re-export. I basically prioritize the `x as x` check over the self-import check if there is an alias.

---

_Comment by @carljm on 2025-11-11 19:38_

I think the exception for `from {.,thispackage} import x as y` is good. Conceptually with this re-export rule we are modeling automatic submodule attribute attachment, and saying "if you explicitly import a submodule, we'll consider it exported". In the case of `x as y`, the submodule attribute would be `x`, not `y`, but you chose to rename it, so we're certainly not going to consider `x` as exported, nor `y` because it doesn't match any other rule. It's only when the `x` and `y` line up with the same name that we're going to consider that "strong enough" to be a re-export.

---

_Comment by @AlexWaygood on 2025-11-11 19:39_

> I think the exception for `from {.,thispackage} import x as y` is good. Conceptually with this re-export rule we are modeling automatic submodule attribute attachment, and saying "if you explicitly import a submodule, we'll consider it exported". In the case of `x as y`, the submodule attribute would be `x`, not `y`, but you chose to rename it, so we're certainly not going to consider `x` as exported, nor `y` because it doesn't match any other rule. It's only when the `x` and `y` line up with the same name that we're going to consider that "strong enough" to be a re-export.

I guess that makes as much sense as anything else here 😆

---

_@AlexWaygood approved on 2025-11-11 19:40_

---

_Comment by @Gankra on 2025-11-11 19:41_

Alright merging, have fun explaining your sins to all your typing counsel friends!

---

_Merged by @Gankra on 2025-11-11 19:41_

---

_Closed by @Gankra on 2025-11-11 19:41_

---

_Branch deleted on 2025-11-11 19:41_

---

_Comment by @AlexWaygood on 2025-11-11 19:45_

> Alright merging, have fun explaining your sins to all your typing counsel friends!

😭

---
