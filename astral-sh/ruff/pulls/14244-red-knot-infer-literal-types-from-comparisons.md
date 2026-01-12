```yaml
number: 14244
title: "[red-knot] Infer `Literal` types from comparisons with `sys.version_info`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/version-info-instance
created_at: 2024-11-10T15:37:03Z
updated_at: 2024-11-11T18:10:18Z
url: https://github.com/astral-sh/ruff/pull/14244
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Infer `Literal` types from comparisons with `sys.version_info`

---

_@AlexWaygood_

## Summary

`sys.version_info` is [annotated in typeshed as being an instance of `sys._version_info`](https://github.com/python/typeshed/blob/ea368c72696afba1eb4c12653123edd764c800bf/stdlib/sys/__init__.pyi#L225-L240). This is correct as far as it goes (or at least as correct as it _can_ be, given typeshed's constraints), but means that we have to work in some special casing for the `sys._version_info` class definition, so that in some situations we treat it the same as we would a heterogenous tuple. We also need to add special casing for `sys.version_info` anyway, so that we infer literal integers for the first two elements; this then enables us to infer that some `sys.version_info` branches are always truthy, and others are always falsey.

This PR adds the necessary special casing.

Helps with #14171 (but doesn't touch any of the control-flow parts of that task, which I think @sharkdp is planning to work on).

## Test Plan

mdtests added.


---

_Label `red-knot` added by @AlexWaygood on 2024-11-10 15:37_

---

_Renamed from "Infer `Literal` types from comparisons with `sys.version_info`" to "[red-kmot] Infer `Literal` types from comparisons with `sys.version_info`" by @AlexWaygood on 2024-11-10 16:55_

---

_Renamed from "[red-kmot] Infer `Literal` types from comparisons with `sys.version_info`" to "[red-knot] Infer `Literal` types from comparisons with `sys.version_info`" by @AlexWaygood on 2024-11-10 16:55_

---

_Marked ready for review by @AlexWaygood on 2024-11-11 12:41_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-11 12:41_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-11 12:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-11 12:41_

---

_Comment by @github-actions[bot] on 2024-11-11 12:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:1435 on 2024-11-11 12:56_

```suggestion
    /// but it's a useful fallback for us in order to infer `Literal` types from `sys.version_info` comparisons.
```

---

_@MichaReiser reviewed on 2024-11-11 12:57_

---

_@sharkdp reviewed on 2024-11-11 13:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-11-11 13:19_

… but would you eventually expect a different output in the `reveal_type` below? As in: is this a TODO comment?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-11-11 13:25_

> … but would you eventually expect a different output in the `reveal_type` below?

No. But it _is_ the reason why there are several other `Type::Todo`s and TODO comments in the file!

It's also the reason why I haven't implemented _more_ special-casing, if that makes sense. What I mean is that we _could_ add special-casing for this class in several other places (`Type::iterate()`, the attribute access for attributes other than `major` and `minor`, etc.). But doing that would be a bit silly IMO, because we'll naturally add support for these without any special-casing for the `sys._version_info` class when we add support for more typing features in the future.

Anyway, I'll clarify this sentence, thanks!

---

_@AlexWaygood reviewed on 2024-11-11 13:25_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1446 on 2024-11-11 13:28_

This can be
```suggestion
            let elements = ["alpha", "beta", "candidate", "final"]
                .iter()
                .map(|level| Type::string_literal(db, level));
            UnionType::from_elements(db, elements)
```

---

_@sharkdp reviewed on 2024-11-11 13:28_

---

_@AlexWaygood reviewed on 2024-11-11 13:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1446 on 2024-11-11 13:32_

I think that would mean we'd do more work than we need to here. `UnionType::from_elements` goes via `UnionBuilder` to ensure that elements are properly deduplicated before the `UnionType` is constructed. But here we know just by looking at it that there are no duplicates in this union, so we can skip a layer of indirection.

https://github.com/astral-sh/ruff/blob/b3b5c191051a4ea2b81b11ed3088a09ec6d051fc/crates/red_knot_python_semantic/src/types.rs#L2714-L2726

https://github.com/astral-sh/ruff/blob/b3b5c191051a4ea2b81b11ed3088a09ec6d051fc/crates/red_knot_python_semantic/src/types/builder.rs#L107-L113

I'll add a comment to explain why I'm not using `UnionType::from_elements` here.

---

_@sharkdp approved on 2024-11-11 13:36_

Thanks!

---

_Comment by @AlexWaygood on 2024-11-11 13:50_

A small bug I discovered while working on this PR in our `tuple`-comparison logic: #14279

---

_Merged by @AlexWaygood on 2024-11-11 13:58_

---

_Closed by @AlexWaygood on 2024-11-11 13:58_

---

_Branch deleted on 2024-11-11 13:58_

---

_@carljm reviewed on 2024-11-11 17:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:56 on 2024-11-11 17:20_

I don't think it's invalid. It's not a runtime error to compare tuples of different lengths, so I don't think it should be a type diagnostic, either. Such a comparison can be either true or false.

---

_Comment by @carljm on 2024-11-11 17:28_

Very nice, thank you!

---

_@AlexWaygood reviewed on 2024-11-11 17:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:56 on 2024-11-11 17:39_

"Invalid" is probably too strong a word, but it seems pretty clear that the user has made a mistake here, right? It probably shouldn't be a type-check diagnostic, but a "type aware lint diagnostic" might be helpful for the user.

Does that make sense? Shall I reword the TODO comment along those lines?

---

_@carljm reviewed on 2024-11-11 18:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:56 on 2024-11-11 18:04_

Yeah, I can see a case for a special-cased warning diagnostic here, but it seems a bit subtle. The existence of additional elements can actually change the result of the comparison! So while it seems like probably a mistake, it's neither a no-op, nor does it render the comparison always-true or always-false. So it's a bit tricky to figure out how to word a "this is wrong" message.

I don't think it's critical to update the comment, although I do think "invalid" is too strong. I don't think a diagnostic for this is a priority, but I don't mind a comment suggesting that we could consider it at some point.

---

_@AlexWaygood reviewed on 2024-11-11 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:56 on 2024-11-11 18:10_

> The existence of additional elements can actually change the result of the comparison!

I'm not even suggesting that we implement a generalised rule for tuples, to be clear! (Though we could, of course.) `sys.version_info` is an instance of a singleton tuple subclass, so we can implement a rule specifically for "comparing a tuple of >5 elements against instances of `sys._version_info`" if we want to. Comparisons against `sys.version_info` are common enough that this could be valuable. (Ruff already has one or two rules that aim to catch common errors in `sys.version_info` comparisons.)

---
