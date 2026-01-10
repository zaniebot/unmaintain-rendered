```yaml
number: 15550
title: "[red-knot] Add support for `typing.ClassVar`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/classvar
created_at: 2025-01-17T12:47:14Z
updated_at: 2025-01-18T12:51:37Z
url: https://github.com/astral-sh/ruff/pull/15550
synced_at: 2026-01-10T20:05:43Z
```

# [red-knot] Add support for `typing.ClassVar`

---

_Pull request opened by @sharkdp on 2025-01-17 12:47_

## Summary

Add support for `typing.ClassVar`, i.e. emit a diagnostic in this scenario:
```py
from typing import ClassVar

class C:
    x: ClassVar[int] = 1

c = C()
c.x = 3  # error: "Cannot assign to pure class variable `x` from an instance of type `C`"
```

## Test Plan

- New tests for the `typing.ClassVar` qualifier
- Fixed one TODO in `attributes.md`

---

_Label `red-knot` added by @sharkdp on 2025-01-17 12:47_

---

_Comment by @github-actions[bot] on 2025-01-17 13:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @sharkdp on 2025-01-17 14:36_

---

_Review requested from @carljm by @sharkdp on 2025-01-17 14:36_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-17 14:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-17 14:36_

---

_@sharkdp reviewed on 2025-01-17 14:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:174 on 2025-01-17 14:40_

We can't do this yet, but should be able to, as soon as we know what the type of `self` is.

---

_@sharkdp reviewed on 2025-01-17 14:45_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2528 on 2025-01-17 14:45_

This could also simply be called `.inner()`, `.inner_type()`, or similar?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1688 on 2025-01-17 16:43_

Nit: adding a method would help with readability here (I think it's the type but hard to tell)

---

_@MichaReiser approved on 2025-01-17 16:44_

Looks good to me from a salsa/rust perspective.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:71 on 2025-01-17 18:24_

Or maybe

```suggestion
    # error: [invalid-type-form] "Type qualifier `typing.ClassVar` expects exactly one type parameter"
```

?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2499 on 2025-01-17 18:29_

strictly this method seems to be querying whether `ClassVar` is _one_ of the qualifiers for the variable rather than whether it _is the sole_ qualifier for the variable, so maybe this might be a better name?

```suggestion
    pub(crate) fn includes_class_var(self) -> bool {
```

I think it's actually invalid to combine `ClassVar` with any other type qualifiers currently (`ClassVar[Final[int]]` is not permitted). But this might change in the future, and we probably want to be able to recover gracefully and infer a good type even if the user does do something illegal with their qualifiers. So using a bitflag rather than an enum seems like a good option here.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1688 on 2025-01-17 18:30_

agreed -- or it could just be a struct with named fields rather than a tuple struct?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4862 on 2025-01-17 18:39_

Hmm, I think the original idea of having both the `infer_annotation_expression` methods and the `infer_type_expression` methods was that the `infer_annotation_expression()` methods would return `(Type, Qualifiers)` but the `infer_type_expression` methods would continue to just return `Type` (since qualifiers are valid in annotation expressions, but not type expressions, per [the grammar in the spec](https://typing.readthedocs.io/en/latest/spec/annotations.html#type-and-annotation-expressions)). Because if we find a `ClassVar[T]` in a context where only a type expression is permitted, we should emit a diagnostic, and probably not record the entire variable as being a `ClassVar`. E.g. this is invalid, and the variable should probably not be recorded as a `ClassVar`. We can debate whether we should infer `Unknown`, `int | Unknown` or `int | str` for `Foo.x`:

```py
class Foo:
    x: int | ClassVar[str]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5419 on 2025-01-17 18:41_

Should we be passing in some context (maybe as part of `InferContext`?) that tells us whether we're in a class body here? That would allow us to emit a diagnostic here if a definition is annotated with `ClassVar` outside a class body, which is obviously illegal

---

_@AlexWaygood reviewed on 2025-01-17 18:41_

Nice!

---

_@sharkdp reviewed on 2025-01-17 18:46_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:5419 on 2025-01-17 18:46_

> that tells us whether we're in a class body here

I felt like I would eventually have to add this anyway for instance attributes that are implicitly defined in methods only. This is why I left this as a TODO here (see test in `classvar.md`).

---

_@sharkdp reviewed on 2025-01-17 18:58_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2499 on 2025-01-17 18:58_

Hm.. for the sake of the argument, imagine that `ClassVar` and `Final` would be allowed, then I think it would still be okay, terminology wise, to say that a particular variable "*is* a classvar and also *is* final"?

I agree that "includes" makes more sense when thinking from the perspective of the set of type qualifiers (which is what this struct represents).

I have now moved this function to `SymbolAndQualifiers` which changes the code that uses it to:
```rs
let instance_member = instance.class.instance_member(self.db(), attr);
if instance_member.is_class_var() {
    // emit diagnostic
}
```

What do you think?

---

_@AlexWaygood reviewed on 2025-01-17 19:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2499 on 2025-01-17 19:00_

Ah, that reads much better if it's a method on `SymbolAndQualifiers`. This is resolved now in that case -- thanks!

---

_@sharkdp reviewed on 2025-01-17 22:10_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:4862 on 2025-01-17 22:10_

Thanks Alex. I implemented this now. It's more code, but definitely the cleaner approach. I think I didn't realize at first that `ClassVar` and `Final` can only appear at the "outer layers" of an annotation expression (unlike `Annotated`, but that is not a type qualifier). I thought I had to modify the existing matches on these KnownInstance types, but we actually need to handle them separately in `infer_annotation_expression_impl`. This introduces a bit of duplicated logic, but I'm afraid it's necessary. The match cases in infer_type_expression are all error cases that only emit diagnostics.

I also had a lot of fun with corpus panics, but it looks like I managed to store exactly one type per expression in the end.

I think I need to do another round of review myself (not today), and probably add some more tests.

---

_@sharkdp reviewed on 2025-01-17 22:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:25 on 2025-01-17 22:13_

@carljm I haven't tackled this TODO yet. I think it requires changes that are rather orthogonal to what I did here, but let me know if you think it should be solved in this PR.

---

_@AlexWaygood reviewed on 2025-01-17 22:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:2538 on 2025-01-17 22:32_

Would it? I don't see `Final` in this example ;)

---

_@sharkdp reviewed on 2025-01-17 22:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:2538 on 2025-01-17 22:34_

Thanks. It was there before, but I removed it and forgot to update the qualifier expression.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5298 on 2025-01-17 22:38_

This feels like it might fit in better as a free function in diagnostic.rs, similar to https://github.com/astral-sh/ruff/blob/98ef5641707cb55c9c4d91983406c6b81d83851c/crates/red_knot_python_semantic/src/types/diagnostic.rs#L1044

---

_@AlexWaygood reviewed on 2025-01-17 22:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:189 on 2025-01-17 23:38_

minor nit, but it would it be clearer to just say "ClassVar" rather than "pure class variable"? I think an explicit `ClassVar` annotation is the only way to create a "pure class variable", so this seems to make the association more direct and explicit, and avoids introducing another vocabulary term ("pure class variable") that we have to define.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:25 on 2025-01-17 23:38_

No, not at all, totally fine to do it separately, as long as it's clearly marked as a TODO (as you've done)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:44 on 2025-01-17 23:46_

Maybe this should be a diagnostic? But it isn't in [pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDCANgIYDOZAaiSAFC0AmApsFCEzAK4gpkD6AIzBgiACgCUUALQA%2BKEJEAuKADo19AMakKhRbSgHMrdlx78FY8XsM2oAD2XFyVGgG1UMALr7DTImSZrWwMHTBQYTSgAXkIJTRU7aKgAJiA) (which has the same behavior you implemented here).

It is in [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=16cd32b442c26dd919e75d151313aff0), but for different reasons than it would be for us, since mypy just generally prohibits re-declaration.

Not sure, don't feel clear enough about it to even suggest a TODO comment. We can always add this diagnostic if we decide we need it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:250 on 2025-01-17 23:53_

nit: is it valuable to maintain an invariant that a variable named `*_ty` contains a `Type`, and a function named `*_ty` returns a `Type`? If so, should we establish a suffix like `*_tyq` for a "type and qualifiers"? Or is this all too pedantic and veering unnecessarily into Hungarian notation? (I don't feel strongly.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2551 on 2025-01-18 00:33_

naming nit: we usually use `_ty` not `_type`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3418 on 2025-01-18 00:36_

We're already inside a match arm for `ctx` being `Store`, why do we need to match on `ctx` again?

---

_@carljm approved on 2025-01-18 00:42_

This looks great!

---

_@sharkdp reviewed on 2025-01-18 12:29_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:250 on 2025-01-18 12:29_

I'm afraid I broke this invariant before, and never cleaned it up. For example, `bindings_ty` returns a `Symbol`. Most call sites that call functions returning `Symbol`/`TypeAndQualifiers` are only interested in the contained type, but it's probably still better to clean up those names. I opened #15569 as a follow-up task for me.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3418 on 2025-01-18 12:34_

Thanks for catching that. I previously had this code block in an entirely different place (where the match on `ctx` was necessary) and forgot to remove it when I moved it here.

With type narrowing, clippy could have been able to catch this :smile: 

---

_@sharkdp reviewed on 2025-01-18 12:34_

---

_@sharkdp reviewed on 2025-01-18 12:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_qualifiers/classvar.md`:44 on 2025-01-18 12:42_

The "currently" was my way to try and describe that this is probably an open design decision. It feels to me like a not very common edge case, so in spirit of "every lint rule generates maintenance work", I'll leave this as-is for now, but it should be straightforward to add a lint for this later if we want to.

---

_Merged by @sharkdp on 2025-01-18 12:51_

---

_Closed by @sharkdp on 2025-01-18 12:51_

---

_Branch deleted on 2025-01-18 12:51_

---
