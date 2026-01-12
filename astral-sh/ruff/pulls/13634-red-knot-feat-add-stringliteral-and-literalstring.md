```yaml
number: 13634
title: "[red-knot] feat: add `StringLiteral` and `LiteralString` comparison"
type: pull_request
state: merged
author: Slyces
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/str-comparison
created_at: 2024-10-04T19:48:34Z
updated_at: 2024-10-07T08:45:25Z
url: https://github.com/astral-sh/ruff/pull/13634
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] feat: add `StringLiteral` and `LiteralString` comparison

---

_@Slyces_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements string lietral comparisons and fallbacks to `str` instance for `LiteralString`.
Completes an item in #13618

## Test Plan

- Adds a dedicated test with non exhaustive cases


---

_Review requested from @carljm by @Slyces on 2024-10-04 19:48_

---

_Review requested from @MichaReiser by @Slyces on 2024-10-04 19:48_

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-04 19:48_

---

_Comment by @github-actions[bot] on 2024-10-04 20:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2614 on 2024-10-05 05:40_

I don't think this is doing what you intend, since `s1` and `s2` are `StringLiteralType` salsa interned structs. So what you're actually comparing here is just the Salsa u32 IDs. I suspect a few more string comparison tests (specifically, one where the string created earlier in the source file is "greater" than one created later in the source file) will reveal the problem. You'll need to use `s1.value(db)` and `s2.value(db)` like you do below in the "in" branch to actually compare the strings.

Let's not even worry about whether Rust and Python have any weird edge-case differences in their lexicographic string ordering 😆 Probably not, and if so I suspect nobody will ever care anyway.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4166 on 2024-10-05 05:42_

Let's add a test showing that `is` returns `Literal[False]` with unequal strings, and also both variants of `is not` (equal and unequal strings).

---

_@carljm reviewed on 2024-10-05 05:43_

Looks great! I think we need a few more tests, in one case to catch a bug that also needs fixing, and in the other just for completeness.

---

_Review requested from @carljm by @Slyces on 2024-10-05 16:54_

---

_@AlexWaygood reviewed on 2024-10-05 17:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2616 on 2024-10-05 17:02_

should this be a TODO rather than a note? I think ideally I'd want to check this and get it right. sounds like I might care about this more than @carljm though 😆

tbh I think somebody will inevitably come up with some unicode corner case that doesn't matter that much and report it as a bug. Maybe we should just avoid the problem entirely by short-circuiting if it's not an ASCII string?

```suggestion
                // For anything non-ASCII, don't even try to infer precise literal types.
                // It would be complicated, it's an unimportant edge case,
                // and Rust and Python probably do subtly different things
                // in their string methods for non-ASCII characters.
                let s1 = salsa_s1.value(self.db);
                if !s1.is_ascii() {
                    return Some(builtins_symbol_ty(self.db, "bool").to_instance(self.db));
                }
                let s2 = salsa_s2.value(self.db);
                if !s2.is_ascii() {
                    return Some(builtins_symbol_ty(self.db, "bool").to_instance(self.db));
                }
```

---

_@Slyces reviewed on 2024-10-05 17:06_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2616 on 2024-10-05 17:06_

I think that's better, good call!

---

_@AlexWaygood reviewed on 2024-10-05 17:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2620 on 2024-10-05 17:10_

heh, now that you've accepted my suggestion, maybe we should add some tests with some emojis or something 😆

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2614 on 2024-10-05 17:39_

Sorry to get back to this only after the change was made, but I feel pretty strongly we should not do this. It's discriminatory against people using non ASCII strings 😆 seriously, it will be surprising if your type inference changes just because you add a non ASCII character in a string literal. We should provide the same level of behavior for all string literals that we support at all. If there's an operation we don't believe we can reasonably support for all string literals, we should just support it for none of them. 

At the very least I think it's important that we do support equality and inequality here. Salsa already interns all our string types based on rust string equality, so if it differs from Python string equality we have an issue that goes beyond this code. 

I feel less strongly about comparison. But my preference would be to just support them all, assuming rust/python equivalence, and leave a note or TODO comment to dig deeper for possible edge cases. I think there is a pretty reasonable chance that this is all sufficiently well specified in Unicode that there actually is no problem. 

---

_@carljm reviewed on 2024-10-05 17:39_

---

_@AlexWaygood reviewed on 2024-10-05 17:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2614 on 2024-10-05 17:47_

Fair enough, I withdraw this suggestion! I, on the other hand, do feel pretty strongly that we should actually check for corner cases here where Rust and Python differ in their handling of strings ;) So I would like us to at least leave a TODO comment (rather than just a note) to check this properly. Otherwise, as I say, I feel like it's inevitable that we'll get bug reports around this. Obviously it doesn't come up that much in real-world code, but people love to hunt for this kind of corner case in static-analysis tools and report them 😆 And, you know, accuracy is nice!

---

_@Slyces reviewed on 2024-10-05 17:54_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2614 on 2024-10-05 17:54_

I'm completely for a `// TODO`, won't look into this myself just yet though
- Reverted both commits
- Fixup'd the `Note:` into `TODO`

---

_@carljm reviewed on 2024-10-05 17:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2614 on 2024-10-05 17:57_

I did a bit of looking, and it seems like both Python and rust have a pretty simple algorithm for the default string comparison: it's just a simple lexicographic sort by Unicode code points. (The complexities happen when you get into locale aware sorting, which requires special libraries in either language.) And we should have the same Unicode code points in our literal strings, since we're literally reading the same source text. (If we don't, that's a bug elsewhere, not here.)

So my conclusion is that we can just go ahead and do this, and there's no need for either a note or a todo comment! Sorry for even raising the question unnecessarily instead of doing the research the first time 😆

---

_Review requested from @carljm by @Slyces on 2024-10-05 17:57_

---

_Review requested from @AlexWaygood by @Slyces on 2024-10-05 17:57_

---

_@Slyces reviewed on 2024-10-05 17:59_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2614 on 2024-10-05 17:59_

We could have done the research as well, so the blame is shared :)

---

_@AlexWaygood reviewed on 2024-10-05 18:00_

You need to switch to using the new APIs you introduced an hour ago in https://github.com/astral-sh/ruff/commit/1c2cafc1010ebfe59573c8dbaf57dc97db192f3c ;)

---

_@AlexWaygood reviewed on 2024-10-05 18:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:2653 on 2024-10-05 18:12_

```suggestion
                self.infer_binary_type_comparison(KnownClass::Str.to_instance(self.db), op, right)
            }
            (_, Type::StringLiteral(_)) => {
                self.infer_binary_type_comparison(left, op, KnownClass::Str.to_instance(self.db))
            }

            (Type::LiteralString, _) => {
                self.infer_binary_type_comparison(KnownClass::Str.to_instance(self.db), op, right)
            }
            (_, Type::LiteralString) => {
                self.infer_binary_type_comparison(left, op, KnownClass::Str.to_instance(self.db))
```

---

_@Slyces reviewed on 2024-10-05 18:13_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types/infer.rs`:2653 on 2024-10-05 18:13_

Saw around the same time, a fixup's already pushed

---

_@AlexWaygood approved on 2024-10-05 18:15_

LGTM

---

_@carljm approved on 2024-10-05 19:22_

---

_Merged by @carljm on 2024-10-05 19:22_

---

_Closed by @carljm on 2024-10-05 19:22_

---

_Branch deleted on 2024-10-05 19:28_

---

_Label `red-knot` added by @MichaReiser on 2024-10-07 08:45_

---
