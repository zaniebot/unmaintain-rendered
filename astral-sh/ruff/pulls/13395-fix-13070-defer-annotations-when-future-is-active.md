```yaml
number: 13395
title: "Fix/#13070 defer annotations when future is active"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: fix/#13070-defer-annotations-when-future-is-active
created_at: 2024-09-18T16:05:41Z
updated_at: 2024-09-19T08:16:03Z
url: https://github.com/astral-sh/ruff/pull/13395
synced_at: 2026-01-12T15:55:44Z
```

# Fix/#13070 defer annotations when future is active

---

_@Slyces_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR tries to provide support for deferred resolution of type-hints in files with `from __future__ import annotations`. Fixes #13070.

### Implementation

Currently, the method `is_stub` is already used in multiple places to resolve deferred annotations. As all current usage of `is_stubs` are dedicated to deferred type inference, I decided in this implementation to refactor `is_stubs` && `has_future_annotations` together in `are_all_types_deferred`.
We can also keep them separate and always test them together.

The main difficulty is how to know if that flag is active. I tried an implementation that checks this when building the semantic index. I do not know if it's the best choice (as `SemanticIndex` doesn't have any attribute similar to this), but the building of the semantic index already parses every statement in the file, which was ideal to find this flag.

Happy to get any feedback on a better spot for this if you can think of one.

## Test Plan

There is currently one test addressing deferred annotations resolution, focused on builtins. I don't quite understand why builtins require deferred annotations, so I focused on test cases I encounter when coding in python.

In my experience, the two most common occurrences requiring deferred annotations (in regular code) are:
- Referencing a symbol before it's defined
- Referencing a class inside one of its own methods

Some testing led me to find that we don't currently support [type resolution for methods](https://github.com/astral-sh/ruff/blob/4eb849aed3a6df9ce0f45dd7a2fdb525e31e61e3/crates/red_knot_python_semantic/src/types.rs#L439), so I only tested the case of referencing a symbol before its definition.

I added 3 separate test case, all with the same base code:
```python
def get_foo() -> Foo: ...
class Foo: ...
foo = get_foo()  # Resolved if and only if deferred annotations are active
```
- Inside a source file (`*.py`, `foo` must not resolve (`Unknown`)
- Inside a stub file (`*.pyi`), `foo` must resolve to `Foo`
- Inside a source file **with __future__.annotations**, `foo` must resolve to `Foo`


---

_Review requested from @carljm by @Slyces on 2024-09-18 16:05_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-18 16:05_

---

_Review requested from @AlexWaygood by @Slyces on 2024-09-18 16:05_

---

_Label `red-knot` added by @AlexWaygood on 2024-09-18 16:08_

---

_Comment by @codspeed-hq[bot] on 2024-09-18 16:11_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Slyces:fix/#13070-defer-annotations-when-future-is-active)

### Merging #13395 will **degrade performances by 4.26%**

<sub>Comparing <code>Slyces:fix/#13070-defer-annotations-when-future-is-active</code> (301d065) with <code>main</code> (d3530ab)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/Slyces:fix/#13070-defer-annotations-when-future-is-active)._

### Benchmarks breakdown

|     | Benchmark | `main` | `Slyces:fix/#13070-defer-annotations-when-future-is-active` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `red_knot_check_file[incremental]` | 2.9 ms | 3 ms | -4.26% |


---

_Comment by @github-actions[bot] on 2024-09-18 16:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Slyces on 2024-09-18 16:36_

I just realised that the performance issue can probably be fixed by relying on the fact that `from __future__ import annotations` should always be the first import in a file - I'll try to get that to work.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:324 on 2024-09-18 17:06_

nit: checking `has_future_annotations()` is slightly cheaper, maybe let's do that first?

---

_@carljm approved on 2024-09-18 17:23_

This is fantastic, thank you!!

The semantic index builder is the right place to look for this, and the semantic index is the right place to put it. Any analysis that we can do without requiring a dependency on other files, we want to do in semantic indexing.

I suspect the performance regression on the incremental check is because tomllib (which we are using for our benchmark) does use `from __future__ import annotations`, and deferred types require an extra Salsa query and thus increase Salsa overhead on incremental check.

So I suspect that requiring `from __future__ import annotations` to be the first statement in a file won't help with the performance.

And I'm not sure if we even want to require it. A mis-placed `from __future__ import annotations` won't work at runtime (the module won't even import), but for our purposes I think we should assume the user intended for it to take effect, and we should still check the code assuming its use. Otherwise accidentally adding a line before your `from __future__ import annotations` could suddenly result in a ton of bogus diagnostics on your forward-reference annotations, which isn't really useful.

I think we should add a comment to this effect, where we check for `from __future__ import annotations` in the semantic index builder.

And I think we should emit a diagnostic ~in type inference~ if we run across a misplaced `__future__` import. But this is pretty much a separate thing and could be its own PR. (EDIT: I actually think probably it shouldn't be done in type inference; misplaced `__future__` imports are a syntax-level error that isn't really part of type-checking. So we'll want this diagnostic, but probably not in type inference, and definitely not in this PR.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:553 on 2024-09-18 17:25_

Something like this:

```suggestion
                    // Look for imports `from __future__ import annotations`, ignore `as ...`.
                    // We intentionally don't enforce the rules about location of `__future__` imports here,
                    // we assume the user's intent was to apply the `__future__` import, so we still check
                    // using it (and TODO will also emit a diagnostic about a mis-placed `__future__` import.)
                    self.has_future_annotations |= alias.name.id.as_str() == "annotations"
```

---

_@carljm reviewed on 2024-09-18 17:25_

---

_Comment by @Slyces on 2024-09-18 17:27_

> So I suspect that requiring from __future__ import annotations to be the first statement in a file won't help with the performance.

That's somewhat of a relief (if it's true) because the implementation I have so far is not ideal. I will still try to go through with it to make sure that it is not the source of the regression

---

_Comment by @carljm on 2024-09-18 18:02_

> Some testing led me to find that we don't currently support [type resolution for methods](https://github.com/astral-sh/ruff/blob/4eb849aed3a6df9ce0f45dd7a2fdb525e31e61e3/crates/red_knot_python_semantic/src/types.rs#L439)

This is true, though the spot in the code you linked is for supporting attribute access of attributes on function objects. To support method calls, I think what we are missing is understanding attribute access on instances of classes (as opposed to class objects): https://github.com/astral-sh/ruff/blob/c173ec5bc7cff4d453ddc6d45293aed5b79f0bbe/crates/red_knot_python_semantic/src/types.rs#L460

---

_Comment by @carljm on 2024-09-18 18:04_

> I will still try to go through with it to make sure that it is not the source of the regression

If this is a lot of work, an easier way to check the performance impact would be to see how performance on the benchmark fares if you just consider `from __future__ import annotations` to always be true, without even looking for it in semantic index.

---

_Comment by @Slyces on 2024-09-18 18:09_

While this is not related to the PR, I did waste some time on the difference between "expected Unknown" (e.g. resolution failed) vs. "Not implemented Unknown" (e.g. this case should yield a value for the current AST but we didn't code it yet).

Would it be worth considering a temporary value (until red-knot is feature complete or almost so) that would make this difference obvious? To be fair I'll also be more careful next time now that I know 🙂

---

_Comment by @carljm on 2024-09-18 18:24_

> Would it be worth considering a temporary value (until red-knot is feature complete or almost so) that would make this difference obvious?

That's an interesting idea! I think we could have `Type::Unknown` contain another enum indicating the source of the unknown (mypy does something similar, @AlexWaygood has mentioned this before). Then most code handling Unknowns wouldn't have to care (we wouldn't have to add a new branch in every place that handles types), but we could display them differently.

This seems like a net positive to me! Not sure I would get to it soon, but if you're interested in doing it, I'd certainly consider a PR.

---

_Comment by @AlexWaygood on 2024-09-18 18:27_

> That's an interesting idea! I think we could have `Type::Unknown` contain another enum indicating the source of the unknown (mypy does something similar, @AlexWaygood has mentioned this before). Then most code handling Unknowns wouldn't have to care (we wouldn't have to add a new branch in every place that handles types), but we could display them differently.

Yes, see #12986 for previous discussion! I think the main concern with that PR was how to handle the inner enum in unions and intersections.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:561 on 2024-09-19 07:30_

```suggestion
                    self.has_future_annotations |= alias.name.id == "annotations"
                        && node
                            .module
                            .as_ref()
                            .is_some_and(|module| module.id() == "__future__");
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:561 on 2024-09-19 07:31_

Or even

```rust
                    self.has_future_annotations |= alias.name.id == "annotations"
                        && node.module.as_deref() == Some("__future__");
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:49 on 2024-09-19 07:32_

We probably want to make this a bit flag long term but a boolean is just fine for now :)

---

_@MichaReiser approved on 2024-09-19 07:32_

This is great. Thanks you

---

_Comment by @MichaReiser on 2024-09-19 08:11_

It's a bit a problem that I can't acknowledge the codspeed regression because the URl doesn't work haha

Okay, there's a way. Navigate to the runs page and open the results from there https://codspeed.io/astral-sh/ruff/runs/66ebdbcf6ba711013f9a14a5

---

_Merged by @MichaReiser on 2024-09-19 08:13_

---

_Closed by @MichaReiser on 2024-09-19 08:13_

---

_Branch deleted on 2024-09-19 08:13_

---
