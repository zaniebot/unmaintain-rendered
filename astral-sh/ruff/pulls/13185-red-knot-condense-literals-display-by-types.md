```yaml
number: 13185
title: "[red-knot] Condense literals display by types"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/#11785-condense-literals
created_at: 2024-08-31T17:29:02Z
updated_at: 2024-09-03T14:46:37Z
url: https://github.com/astral-sh/ruff/pull/13185
synced_at: 2026-01-12T15:55:43Z
```

# [red-knot] Condense literals display by types

---

_@Slyces_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #11785 by condensing string literals by type.

Currently supports (in this order of display):
- Integer Literals: `Literal[0] | Literal[1]` → `Literal[0, 1]`
- String Literals: `Literal["a"] | Literal["b"]` → `Literal["a", "b"]`
- Bytes Literals: `Literal[b"\x00"] | Literal[b"\x07"]` → `Literal[b"\x00", b"\x07"]`
- Booleans Literals: `Literal[False] | Literal[True]` → `Literal[False, True]`
- Classes: `Literal[A] | Literal[B]` → `Literal[A, B]`
- Functions: `Literal[bar] | Literal[foo]` → `Literal[bar, foo]`

Behaviour for all other types is unchanged.

## Notes

This affects one existing test because this gives priority to condensed unions (e.g. classes) over non condensed types, which happened in 1 test case.

## Test Plan

Adds a unit test in `crates/red_knot_python_semantic/src/types/display.rs`

---

_Review requested from @carljm by @Slyces on 2024-08-31 17:29_

---

_Review requested from @MichaReiser by @Slyces on 2024-08-31 17:29_

---

_Review requested from @AlexWaygood by @Slyces on 2024-08-31 17:29_

---

_Label `red-knot` added by @AlexWaygood on 2024-08-31 17:33_

---

_Renamed from "#11785 condense literals display by types" to "[red-knot] Condense literals display by types" by @AlexWaygood on 2024-08-31 17:34_

---

_Comment by @github-actions[bot] on 2024-08-31 19:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:107 on 2024-09-01 05:52_

We should actually remove the support for boolean types here, because both boolean literal types should never appear in the same union, they should be replaced by the `bool` type. See https://github.com/astral-sh/ruff/pull/13178

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:101 on 2024-09-01 05:54_

This is getting close to the point where I'm not totally comfortable having `UnionType` effectively replicate the `Display` logic of each other kind of literal type -- but I think I am ok with it for now. We can always consolidate in future if it starts causing issues.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:94 on 2024-09-01 05:55_

Slightly tempted to suggest either using a struct instead of `all_literals`, or using constants instead of the magic number indices -- but I think this code is all close enough together that it's fine as-is. Using a struct would be significantly more code with arguable benefit, and using constants just doesn't seem necessary when all the indices are used once, in order, in close proximity.

---

_@carljm reviewed on 2024-09-01 05:56_

Thank you, this is great!!

I'd like to remove the boolean support (see inline comment), but otherwise this looks good to merge to me.

---

_Comment by @Slyces on 2024-09-01 11:13_

I removed the support for bool literals.

I think I'd like to give a go at your 2 improvement comments:
- Use a struct to hold the different groups of literals (vs. `Vec<Vec<String>>`)
- Refactor the logic of each type's string representation (to avoid duplicating logic in indivdual `Display` & `UnionType`'s `Display`)

I can either do this in this PR or in 1 or 2 separate PRs, as you'd like :)

---

_Comment by @carljm on 2024-09-01 15:31_

Sure, that'd be excellent! The first one (struct instead of vec of vecs) I'm not totally convinced will be a net improvement, but if you try it and feel that it is, that's great.

The second one I think we should probably do; maybe simplest to just go ahead and do it in this PR?

---

_Comment by @Slyces on 2024-09-01 16:49_

I've got the 2 improvements:
- 85169b8f36c53996c571ac141a3b9d2d07809ac9 introduces a private function (`representation`) to avoid the duplicated logic. I think this is better for sure, would love if you have a better idea for naming though
- 8c5e6acbecca175592e2139aff7406dd84eca37b uses an enum to store the different groups in a hashmap (instead of relying on a `Vec<Vec<_>>`. I don't know 100% if it's better, I'd like your feedback on that. If you prefer the previous iteration, I can easily revert
  - Note: I also tried with `"function", ...` static strings in the hashmap, but I think the enum is better

---

_Comment by @codspeed-hq[bot] on 2024-09-01 20:22_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Slyces:feat/#11785-condense-literals)

### Merging #13185 will **not alter performance**

<sub>Comparing <code>Slyces:feat/#11785-condense-literals</code> (5f0a27e) with <code>main</code> (599103c)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:29 on 2024-09-02 08:17_

Returning a `String` here results in an unnecessary allocation. What we can do to avoid the string allocation is to change the method to `write_representation(&self, f: &mut Formatter) -> std::fmt::Result` and directly write into the target output string.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:81 on 2024-09-02 08:20_

I think I would simplify this code by:

1. Adding a `is_literal` method to `Type` that returns true if `self` is a `Class`, `Function`, Int`, Boolean`, `String` or `Bytes` Literal. 
2. Change the method here to:

```rust
let is_literal = self.ty.is_literal();

if is_literal {
	f.write_str("Literal[")?;
}

match self.ty {
	...
}

if is_literal {
	f.write_str("]")?;
}

```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:109 on 2024-09-02 08:21_

Should this also include `Boolean`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:133 on 2024-09-02 08:24_

We generally try to preserve the source order of unions. Grouping them here would undo that ordering. I'm inclined that we should only group subsequent elements but I'm not sure. I'm interested to hear more opinions.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:151 on 2024-09-02 08:26_

Pushing the `string` here is expensive because it results in many unnecessary allocations. It would be better to just push the type and directly write the representation to the target buffer.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/display.rs`:133 on 2024-09-02 08:26_

```suggestion
        let mut literals_representation_map = FxHashMap::<CondensedLiteral, Vec<String>>::new();
```

---

_@MichaReiser reviewed on 2024-09-02 08:27_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/display.rs`:81 on 2024-09-02 09:50_

I thought about having this information in `Type` as it's way cleaner, but I was hesitant to code outside of `display.rs`. Thank you for the suggestion 🙏 

---

_@Slyces reviewed on 2024-09-02 09:50_

---

_@Slyces reviewed on 2024-09-02 09:51_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/display.rs`:133 on 2024-09-02 09:51_

Would you mean trying to remember the order in which we encounter the first element of each group, and condense literals in that order?

---

_@AlexWaygood reviewed on 2024-09-02 10:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/display.rs`:133 on 2024-09-02 10:30_

> We generally try to preserve the source order of unions. Grouping them here would undo that ordering. I'm inclined that we should only group subsequent elements but I'm not sure. I'm interested to hear more opinions.

I would prefer that we convert `Literal[1] | str | Literal[2]` to `Literal[1, 2] | str`. The former is much more verbose and isn't at all idiomatic (we even have lint rules against doing this if you're annotating your own code). It would be pretty clunky for us to show that in error messages, in my opinion. However, within the groups, I agree that it would be good to preserve the order as much as possible.

It's interesting that mypy and pyright take quite different approaches here. Mypy appears to always display unions as a flat list of elements, but it exactly preserves the order as the user gave them. Pyright always condenses `Literal` elements inside a union (even `Literal` elements of different types), but does not preserve the order of the union elements at all:

- https://mypy-play.net/?mypy=latest&python=3.12&gist=2a5ec4068b420e5dd94b5f4c1844bd20
- https://pyright-play.net/?pythonVersion=3.13&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2AsAFB0AmJwUwYYAFHVD1AB4AuQsTJUA2gEYAulAA%2BUAM4wQc4aQqUxAJikAabrzhCi68RN1QdqpSH21eUAF7GRGsQCI2YdxYAsWiwAjd0DyEHcpOgBKKABaAD4oADkwFBIBAx4QEgA3EioAfXgEEg4%2BKMyobLzC4tK4CvteavzKIsRSxyigA

Of the two I like pyright's approach more overall, but I would prefer it if we could preserve order as much as possible nonetheless

---

_@Slyces reviewed on 2024-09-02 12:58_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/display.rs`:133 on 2024-09-02 12:58_

Thank you so much for your comment and the comparison with existing implementations.

> However, within the groups, I agree that it would be good to preserve the order as much as possible

Can I understand this as:
- We do group literals by type as stated in the issue (unlike pyright)
- Within groups (e.g. ints) we show them in order of appearance
  - To be clear, this would break the previous implementation of sorting ints

> Of the two I like pyright's approach more overall, but I would prefer it if we could preserve order as much as possible nonetheless

Can I understand that as showing the groups in order of appearance?

Example:
```
Literal[1, 2] | None | Literal["A"] | Literal[0] | Literal["B"] | Unbound
    
-> Literal[1, 2, 0] | None | Literal["A", "B"] | Unbound
```
Here, we would:
- Condense by literal group
- Preserve order inside groups (order of appearance)
- Preserve order outside groups (order of appearance of the first element of the group)

That would be similar to what pyright does, except that they always place `Literal[...]` at the end and don't split them by type

All my apologies if I didn't understand correctly :)

---

_@Slyces reviewed on 2024-09-02 14:13_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/display.rs`:151 on 2024-09-02 14:13_

I applied all your performance improvements comments in the last commit, I think the code is much cleaner. Thank you again.

---

_Review requested from @carljm by @Slyces on 2024-09-02 21:29_

---

_Review requested from @MichaReiser by @Slyces on 2024-09-02 21:29_

---

_@MichaReiser approved on 2024-09-03 07:10_

---

_Merged by @MichaReiser on 2024-09-03 07:23_

---

_Closed by @MichaReiser on 2024-09-03 07:23_

---

_Branch deleted on 2024-09-03 07:25_

---

_Comment by @carljm on 2024-09-03 14:46_

Thank you for this contribution, and for working through all the review comments!

---
