```yaml
number: 13135
title: "[red-knot] Reduce some repetitiveness in tests"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: is-macro
created_at: 2024-08-28T09:47:39Z
updated_at: 2024-09-03T10:26:45Z
url: https://github.com/astral-sh/ruff/pull/13135
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Reduce some repetitiveness in tests

---

_@AlexWaygood_

## Summary

~~This PR derives `is_macro::Is` on the `Type` enum, to reduce repetitiveness across our red-knot test suite. The `expect_*` methods that the macro generates for us are particularly useful for testing: we can remove quite a lot of duplication now, and I expect that we'll have a need to add similar tests in the future.~~

~~`is-macro` is an existing dependency of Ruff, and I doubt we'll be getting rid of it from Ruff anytime soon, as its use in Ruff is quite widespread.~~

Following discussion on this PR, it now just adds handwritten methods that are the equivalent of what `is-macro` would generate for us.

## Test Plan

`cargo test`

---

_Label `red-knot` added by @AlexWaygood on 2024-08-28 09:47_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-28 09:47_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-28 09:47_

---

_Comment by @github-actions[bot] on 2024-08-28 10:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:292 on 2024-08-28 12:09_

What's the rationale for this name change?

Looking at https://rust-lang.github.io/api-guidelines/naming.html#ad-hoc-conversions-follow-as_-to_-into_-conventions-c-conv it seems to me that if we are adding a conventional prefix here, the appropriate one would be `to_`, as this is a conversion that stays at the same level of abstraction, and also doesn't consume `self` (`Type` is `Copy`).

---

_@carljm reviewed on 2024-08-28 12:12_

This looks pretty nice to me! The only part I don't understand the reason for is the rename from `instance` to `into_instance`.

I know @MichaReiser has objections to `is-macro`, but I don't understand them, so it'd probably be nice to wait for his review so we can discuss before merging.

---

_@AlexWaygood reviewed on 2024-08-28 12:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:292 on 2024-08-28 12:16_

The name of the method has to change because it now conflicts with a generated `instance()` method that deriving `is_macro::Is` adds to the class: `ty.instance()` would produce `Some(<inner class type>)` if `ty` was a `Type::Instance` variant, or `None` if not.

> it seems to me that if we are adding a conventional prefix here, the appropriate one would be `to_`, as this is a conversion that stays at the same level of abstraction, and also doesn't consume `self` (`Type` is `Copy`).

Ahh, good point, `to_` is the better prefix here. I'll make that change, thanks!

---

_@AlexWaygood reviewed on 2024-08-28 12:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:292 on 2024-08-28 12:18_

See https://docs.rs/is-macro/latest/is_macro/derive.Is.html#example. It's possible to rename the `is-macro`-generated methods, but here it feels easier to rename the custom method we already have

---

_Comment by @dhruvmanila on 2024-08-29 11:33_

This doesn't necessarily need to block this PR but one drawback that I've faced a couple of times in the past is that rust-analyzer won't be able to update the method references when a variant is renamed using the rename capability. Using explicit methods allows us to do that by simply renaming the references of the variants and the method. From that point onwards, I've usually tried to avoid using `is-macro` unless it provides a net benefit.

---

_Comment by @AlexWaygood on 2024-08-29 11:41_

> This doesn't necessarily need to block this PR but one drawback that I've faced a couple of times in the past is that rust-analyzer won't be able to update the method references when a variant is renamed using the rename capability. Using explicit methods allows us to do that by simply renaming the references of the variants and the method. From that point onwards, I've usually tried to avoid using `is-macro` unless it provides a net benefit.

Ah, thanks. That's a great point and definitely worth keeping in mind. For this case specifically, I still feel like the usefulness of the generated methods outweighs that cost, especially since it's a large enum already which is likely to continue growing as we add support for more typing features. But I absolutely agree that we should in general try to check whether we actually _need_ `is-macro` on a given enum before deriving it.

---

_Comment by @MichaReiser on 2024-09-02 06:40_

> This doesn't necessarily need to block this PR but one drawback that I've faced a couple of times in the past is that rust-analyzer won't be able to update the method references when a variant is renamed using the rename capability. Using explicit methods allows us to do that by simply renaming the references of the variants and the method. From that point onwards, I've usually tried to avoid using `is-macro` unless it provides a net benefit.

This, and it also has the downside that I can't search for references. E.g. find all references to `is_any` won't work because there's no such method that I can select in the IDE. I only recently ran into this again when searching for all usages of `PreviewMode.is_enabled` which I couldn't. I had to remove the `is_macro` and add the method manually to find all referneces.

It's also important to note that the `is-macro` adds many more methods:

* `as_*`
* `as_mut_*`
* `expect_*`
* `*` (which should be named `into_*` to follow ruff's/rust's naming convention)

I still prefer not to use the is-macro because it just got in my way in the past when it make refactors or finding references more painful at the mere benefit that adding the methods becomes cheaper. Making adding the methods cheap also comes at the downside that we don't think about whether we want all these methods and how the methods should be named.

I won't block this change but I keep my preference. If you go ahead with this, please rename the take methods to `into_`. I would love to link you to some documentation but it seems the macro is entirely undocumented and it seems that renaming the `take` method isn't possible. Which is kind of a deal breaker to me because it means that all types using the `is-macro` don't follow our naming convention ([source](https://docs.rs/is-macro/latest/src/is_macro/lib.rs.html#284-291))



---

_@MichaReiser reviewed on 2024-09-02 06:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:432 on 2024-09-02 06:58_

I do like the rename from `instance` to `to_instance` or `as_instance` because it is a conversion method and not a simple getter.

---

_Renamed from "[red-knot] Use `is-macro` to reduce repetitiveness in tests" to "[red-knot] Reduce some repetitiveness in tests" by @AlexWaygood on 2024-09-02 15:29_

---

_Comment by @AlexWaygood on 2024-09-02 15:30_

I still think we may want to use `is-macro` at some point, as I expect this enum will continue to grow in the coming months. But for now I've switched the PR so that it just writes out by hand the methods we'd find useful.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-02 15:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:300 on 2024-09-02 16:22_

Nit: My preferred way here is:

```
let function =  ty.into_function().unwrap();
```

I do find `into_*` helpers generally useful and I'm fine if tests simply fail with an `unwrap`.

---

_@MichaReiser approved on 2024-09-02 16:23_

---

_Comment by @codspeed-hq[bot] on 2024-09-03 09:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:is-macro)

### Merging #13135 will **degrade performances by 4.45%**

<sub>Comparing <code>AlexWaygood:is-macro</code> (64df49b) with <code>main</code> (facf6fe)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:is-macro)._

### Benchmarks breakdown

|     | Benchmark | `main` | `AlexWaygood:is-macro` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.45% |


---

_Merged by @AlexWaygood on 2024-09-03 10:26_

---

_Closed by @AlexWaygood on 2024-09-03 10:26_

---

_Branch deleted on 2024-09-03 10:26_

---
