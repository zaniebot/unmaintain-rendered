```yaml
number: 13562
title: Add some basic subscript type inference
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: main
head: charlie/sub
created_at: 2024-09-30T02:51:48Z
updated_at: 2024-09-30T20:51:01Z
url: https://github.com/astral-sh/ruff/pull/13562
synced_at: 2026-01-10T20:59:36Z
```

# Add some basic subscript type inference

---

_Pull request opened by @charliermarsh on 2024-09-30 02:51_

## Summary

Just for tuples and strings -- the easiest cases. I think most of the rest require generic support?


---

_Review requested from @carljm by @charliermarsh on 2024-09-30 02:51_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-09-30 02:51_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-09-30 02:51_

---

_Label `red-knot` added by @charliermarsh on 2024-09-30 02:51_

---

_@charliermarsh reviewed on 2024-09-30 02:52_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2409 on 2024-09-30 02:52_

I assume it is beneficial to compute the `StringLiteral` when we can?

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 02:52_

What's the general philosophy around when we should error in a case like this?

---

_@charliermarsh reviewed on 2024-09-30 02:52_

---

_@AlexWaygood reviewed on 2024-09-30 10:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2409 on 2024-09-30 10:46_

yup

---

_Comment by @github-actions[bot] on 2024-09-30 10:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 11:17_

There's a few things to note here.

### Issues with `LiteralString`

According to a strict reading of [PEP 675](https://peps.python.org/pep-0675/#type-inference), I don't think this is a legal place where we can infer `LiteralString`, since the PEP states:

> As per the [Rationale](https://peps.python.org/pep-0675/#rationale), we also infer `LiteralString` in the following cases:
> 
> [...]
>
> - Literal-preserving methods: In [Appendix C](https://peps.python.org/pep-0675/#pep-675-appendix-c), we have provided an exhaustive list of `str` methods that preserve the `LiteralString` type.

`str.__getitem__` is not listed in Appendix C of the PEP, so I suppose that means that we're not allowed to infer `LiteralString` for `x`, `y` or `z` in a snippet like this:

```py
def foo(a: Literal["foo"], b: LiteralString, c: int):
    x = a[c]
    y = b[42]
    z = b[c]
```

I'm not sure what the status of Appendix C is, though... it doesn't seem to have been copied into the [relevant section](https://typing.readthedocs.io/en/latest/spec/literal.html#valid-locations-for-literalstring) of the typing spec, so perhaps it isn't canonical anymore? Or maybe that was just an omission when writing the spec? There's also the question of typeshed's stubs: typeshed doesn't include a `LiteralString` overload in its stub for `str.__getitem__`, although it includes `LiteralString` overloads for many of its other `str` methods. I'm not sure if there's a reason for that or not -- even if not, it may be desirable to be in sync with typeshed here.

For now I think I'd just delete this branch -- continuing to fallback to `Unknown` is ~fine for now.

### Emitting diagnostics

In general, if we can't infer that an operation creates a `Literal` type (whether that's `LiteralString` or `StringLiteral`), we should fall back to typeshed's stubs. Here that means falling back to `builtins.str.__getitem__` -- you'd do something like `value_ty.to_meta_type(db).member(db, "__getitem__").call(&[value_ty, slice_ty])` to figure out what the type of the subscript expression is. Except there are some complications ;) Take a look at `Type::iterate` for a more complete example of how we lookup dunder methods elsewhere, try to call them, and emit a diagnostic if the dunder method eithere doesn't exist or isn't callable. https://github.com/astral-sh/ruff/blob/5118166d21784e7c78e38ea42919ba50bb2a5142/crates/red_knot_python_semantic/src/types.rs#L629-L684

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6454 on 2024-09-30 11:17_

```suggestion
    fn subscript_literal_string() -> anyhow::Result<()> {
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6470 on 2024-09-30 11:18_

`c` leads to a `TypeError` at runtime; this isn't correct. `TypeError`s should always cause us to infer `Unknown`. We should emit a diagnostic and infer `Unknown` rather than `LiteralString`

---

_@AlexWaygood requested changes on 2024-09-30 11:19_

---

_@charliermarsh reviewed on 2024-09-30 13:02_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:6470 on 2024-09-30 13:02_

What is the plan for emitting those diagnostics? Don't we need to propagate some information about _why_ it failed?

---

_@AlexWaygood reviewed on 2024-09-30 13:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6470 on 2024-09-30 13:10_

Of course! Like I said in https://github.com/astral-sh/ruff/pull/13559#issuecomment-2382679447, take a look at the `add_diagnostic` and `not_iterable_diagnostic` methods for how we do this elsewhere

---

_Comment by @charliermarsh on 2024-09-30 14:18_

Okay thanks, will re-request review once I've addressed feedback.

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-09-30 14:20_

---

_Comment by @charliermarsh on 2024-09-30 14:20_

Actually, it should be fine to merge in its current state. I'll look at doing the dunder fallback and diagnostics in a separate PR to keep this small.

---

_@AlexWaygood reviewed on 2024-09-30 14:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-30 14:24_

You need to add calls to `self.add_diagnostic()` on all these `else {Type::Unknown}` branches (or add a helper method, similar to our `not_iterable_diagnostic()` helper, and call that from each branch)

---

_@charliermarsh reviewed on 2024-09-30 14:28_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-30 14:28_

It seems like that shouldn't be "necessary" for the PR to merge given that we have fallbacks to `Type::Unknown` everywhere without diagnostics (`infer_import_definition` would be a good example), but I'll add it here.

---

_@AlexWaygood reviewed on 2024-09-30 14:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-30 14:32_

> `infer_import_definition` would be a good example

We emit a diagnostic here in that method when we can't resolve the import, no? https://github.com/astral-sh/ruff/blob/5f4b2823277df38fc222e9d8096b805f9cc34c74/crates/red_knot_python_semantic/src/types/infer.rs#L1314

---

_@AlexWaygood reviewed on 2024-09-30 14:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-30 14:35_

But yeah, we can defer adding the calls to `add_diagnostic` if you want! But I'd definitely like us to at least add `TODO` comments for these branches if we are going to defer it. Inferring `Unknown` is correct here, but we shouldn't do it silently without emitting a diagnostic to the user informing them about the error, and I think there's a strong chance we'll forget about this otherwise

---

_@charliermarsh reviewed on 2024-09-30 14:42_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-30 14:42_

(I was referring to:

```rust
tracing::debug!("Failed to resolve import due to invalid syntax");
Type::Unknown
```
)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2472 on 2024-09-30 14:45_

Right, but that's because we'll have already emitted a diagnostic in an earlier pass if the node contains invalid syntax; there's no need to duplicate the parser's diagnostic in the type inference builder. That's not true for the branches you're adding here, however

---

_@AlexWaygood reviewed on 2024-09-30 14:45_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-09-30 16:21_

---

_Comment by @charliermarsh on 2024-09-30 16:26_

Added diagnostics, though unsure if there are established patterns around the rule names, granularity, etc.

---

_Comment by @AlexWaygood on 2024-09-30 16:28_

> Added diagnostics, though unsure if there are established patterns around the rule names, granularity, etc.

It's a bit of a wild west right now, I wouldn't worry about that too much! I'm sure we'll clean them up at some point (and make the diagnostics less stringly typed etc.). Micha's proposals he shared during the on-site go some way to addressing this.

---

_Comment by @charliermarsh on 2024-09-30 16:29_

Sounds good, I figured as much :)

---

_@AlexWaygood reviewed on 2024-09-30 16:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1263 on 2024-09-30 16:40_

The other two diagnostics look great to me, but this I don't think is correct. These diagnostics will be messages shown to users indicating errors in their code, but this diagnostic doesn't represent that -- it represents an implementation detail of how we're representing Python types in Rust.

I think if we get an `IntLiteral` so big that it can't fit into a `usize`, we don't actually need to know its precise value here, because we _know_ it will represent an index-out-of-bounds for both a string-literal and a tuple:
- The maximum size of a string-literal type is set here: https://github.com/astral-sh/ruff/blob/d86b73eb3dbcf06552a2e4d316ef1765608bf4a4/crates/red_knot_python_semantic/src/types/infer.rs#L282
- The maximum size of a heterogenous tuple type will be `usize::Max`, due to the `TupleType` definition here: https://github.com/astral-sh/ruff/blob/d86b73eb3dbcf06552a2e4d316ef1765608bf4a4/crates/red_knot_python_semantic/src/types.rs#L1214-L1218

An `i64` bigger than `usize::MAX` will be bigger than either of those numbers

---

_@carljm reviewed on 2024-09-30 16:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 16:42_

I don't agree with the pedantic reading of PEP 675 (or typeshed) as a rationale for removing this branch. Many useful things are simply overlooked. If a more precise type inference is clearly correct in terms of the runtime semantics, and there's nothing in the typing spec or conformance suite explicitly prohibiting it, then I think we should feel totally free to go ahead and do it.

It is clearly correct wrt the runtime semantics to infer `LiteralString` as the result of indexing into a `LiteralString` or `StringLiteral`.

---

_@carljm reviewed on 2024-09-30 16:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 16:51_

On second thought, given the security rationale for `LiteralString`, there _may_ be an argument that untrusted-user-supplied indices could allow turning a "safe" `LiteralString` into an "unsafe" one via careful cropping. I have mixed feelings about this, because there's also type-system value to `LiteralString` (we know it is really a built-in `str`, not a subclass), and in type-system terms, the inference of `LiteralString` is correct. And the security issue seems fairly contrived. But that _might_ be why inference of `LiteralString` on `__getitem__` is not in the PEP?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 16:52_

> It is clearly correct wrt the runtime semantics to infer `LiteralString` as the result of indexing into a `LiteralString` or `StringLiteral`.

I agree, but the fact that PEP-675 is so clear that the list of string methods in Appendix C is valid is meant to be "exhaustive" made me worried that there was something I was misunderstanding :-) and until recently, PEP-675 _was_ the spec -- it's not clear to me whether Appendix C was deliberately kept out of the typing spec or not.

This branch was anyway incorrect -- `StringLiteral` cannot be indexed into with objects of arbitraty types, which is what this branch allowed before Charlie removed it.

---

_@AlexWaygood reviewed on 2024-09-30 16:52_

---

_@carljm reviewed on 2024-09-30 16:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1263 on 2024-09-30 16:53_

Yes, we shouldn't emit this as a diagnostic. Wherever we would do so, we should just silently fall back to whatever type we can safely infer without knowing the precise index.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1254 on 2024-09-30 16:57_

Not sure if typo or intentional, but "out of bounds" would be the usual terminology here; "out of band" usually means something else (side-channel communication.)

```suggestion
    pub(super) fn tuple_index_out_of_bounds_diagnostic(
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1272 on 2024-09-30 16:58_

```suggestion
    pub(super) fn string_index_out_of_bounds_diagnostic(
```

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 17:05_

So to clarify... We need to add a branch for `__getitem__` (in general -- I plan to do this in a separate PR), and slices into `StringLiteral` (with non-int literal indexes) should be handled by that branch? Or do we want to resolve to `LiteralString`?

---

_@charliermarsh reviewed on 2024-09-30 17:05_

---

_@charliermarsh reviewed on 2024-09-30 17:11_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1263 on 2024-09-30 17:11_

Removed

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2426 on 2024-09-30 17:16_

Python allows negative indexing, which indexes from the end of the container:

```
>>> t = (1, 2, 3)
>>> t[-1]
3
```

So we should handle that here. I would say we don't have to do it in this PR, and we can just add a TODO, but I don't like that in the meantime this would mean we'd wrongly emit an out-of-bounds error.

---

_@carljm reviewed on 2024-09-30 17:18_

Looks great, thank you! One minor naming nit, and the issue of negative indexing.

---

_@charliermarsh reviewed on 2024-09-30 17:27_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:1254 on 2024-09-30 17:27_

Hah sorry, that's just a typo.

---

_@charliermarsh reviewed on 2024-09-30 17:28_

---

_Review comment by @charliermarsh on `crates/red_knot_python_semantic/src/types/infer.rs`:2426 on 2024-09-30 17:28_

I'll handle it here, thanks.

---

_@carljm reviewed on 2024-09-30 17:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2415 on 2024-09-30 17:42_

After further thought, I don't really buy the possible security argument I mentioned above for not inferring `LiteralString`: you can easily construct many other cases where the contents of a `LiteralString`, though always originating from a literal string in the source code, could be _influenced_ in some way by untrusted user input (e.g. determining whether some concatenation does or doesn't occur.)

But that said, I don't think this is critical, and it is probably best to just rely on typeshed for this. So we could at some point consider proposing a change to typeshed with a `LiteralString` overload for `str.__getitem__`, but I don't think it's worth an extra special case in red-knot to diverge from typeshed here.

So yeah, I would say let's just add the branch that uses the return type from `__getitem__`, as Alex suggested. (And yeah, fine for that to be a separate follow-up if you prefer.

(I suspect, but don't know, that the reason Appendix C isn't in the type spec is that it was considered to already be reflected in the typeshed change.)

---

_Review requested from @carljm by @charliermarsh on 2024-09-30 18:34_

---

_@carljm approved on 2024-09-30 20:17_

Nice! LGTM.

I think there would be a possible implementation where we extract a helper function for the actual indexing-or-None for each type, and then the negative case could just add `.len()` to the index and try that function. But there's really not that much duplication in this version, so I'm not sure that would be worth it, happy with this as-is.

---

_Merged by @charliermarsh on 2024-09-30 20:50_

---

_Closed by @charliermarsh on 2024-09-30 20:50_

---

_Branch deleted on 2024-09-30 20:50_

---

_Comment by @charliermarsh on 2024-09-30 20:51_

Thanks for the thorough reviews, much appreciated! I'll look at the `__getitem__` part next.

---
