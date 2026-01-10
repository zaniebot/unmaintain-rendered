```yaml
number: 13938
title: "Extract `LineIndex` independent methods from `Locator`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/line-ranges
created_at: 2024-10-26T15:55:16Z
updated_at: 2024-10-28T08:08:40Z
url: https://github.com/astral-sh/ruff/pull/13938
synced_at: 2026-01-10T20:59:37Z
```

# Extract `LineIndex` independent methods from `Locator`

---

_Pull request opened by @MichaReiser on 2024-10-26 15:55_

## Summary

The `Locator` provides useful methods to extract a line's range or text given an offset
or the full range or text of all lines falling into a text range. 

These methods are foundational and used across many crates. 

However, `Locator` also exposes methods that require computing and memoizing a `LineIndex.` 
The formatter doesn't use any of those methods and "cheats" by always creating an ad-hoc `Locator`
instead of storing an instance on `PyFormatContext`. This leaks the `Locator` abstraction
because it depends on the knowledge of which `Locator` methods require the extra `LineIndex` state. 

This PR splits the "stateless" methods from `Locator` into a new trait and implements that trait 
for `str` (string slices). It also replaces all `Locator` usages with `&str` in all crates other than `ruff_linter`. 
It also moves `Locator` to `ruff_linter` to clarify that this is linter-specific infrastructure. 

I decided to keep `Locator` for `ruff_linter` because:

* `Locator` communicates that this is the source of the entire file
* Some linter code uses the `LineIndex` dependent `Locator` methods where the lazy index computation is useful. 

We could consider changing more `Linter` methods to take `&str` instead of `Locator` but we can do this in separate PRs.

## Renamed methods

I had to rename `lines` to `lines_str` `str.lines` conflicts with the std `lines` builtin (returns an iterator over the lines). I decided to add the `_str` postfix to all other methods that return a `str` for consistency.


## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-10-26 15:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-26 15:55_

---

_Label `internal` added by @MichaReiser on 2024-10-26 15:55_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-10-26 16:02_

---

_Comment by @github-actions[bot] on 2024-10-26 16:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/assertion.rs`:71 on 2024-10-26 16:20_

`range.range()` reads a little oddly -- maybe this?

```suggestion
    fn range_text(&self, ranged: impl Ranged) -> &str {
        &self.source[ranged.range()]
```

---

_Review comment by @AlexWaygood on `crates/ruff_formatter/src/builders.rs`:375 on 2024-10-26 16:21_

hmm, this looks like it could still be `const`?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:11 on 2024-10-26 16:44_

We don't actually need these imports at all here. I didn't really understand properly how Rust imports worked when I wrote this file 😶

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff_text_size/src/traits.rs`:99 on 2024-10-26 16:54_

hmm, most of the uses I see of this method in this PR seem to be cases where we could just directly slice the string using `[]` syntax. Not sure I see the use of this trait.

---

_@AlexWaygood approved on 2024-10-26 16:54_

Nice, this is great!

---

_Review comment by @MichaReiser on `crates/ruff_text_size/src/traits.rs`:99 on 2024-10-27 08:30_

It's correct, there aren't too many usages of `slice` in this PR but `slice` is a very common operator on `Locator` and this trait could help us migrate these calls more easily. But I'm also fine with removing it for now. 

Ideally we would implement `std::ops::Index` for `T` where `T: Ranged` but we can't do that for a foreign trait :( 

An other alternative is to implement `Ranged::slice(str)` although I think that looks off on the call site

```
expression.slice(source)
```

---

_@MichaReiser reviewed on 2024-10-27 08:30_

---

_@AlexWaygood reviewed on 2024-10-27 08:38_

---

_Review comment by @AlexWaygood on `crates/ruff_text_size/src/traits.rs`:99 on 2024-10-27 08:38_

I also don't mind the trait too much! Just wondered if it was necessary. If it makes the transition easier, that's good enough reason for me

---

_@dhruvmanila approved on 2024-10-28 06:19_

---

_@MichaReiser reviewed on 2024-10-28 07:39_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/assertion.rs`:71 on 2024-10-28 07:39_

I went ahead and deleted the method...  :laughing: It's used in a single place where it's always a `TextRange`

---

_@MichaReiser reviewed on 2024-10-28 07:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:11 on 2024-10-28 07:40_

How did you even find this in such a large diff :laughing: 


---

_Merged by @MichaReiser on 2024-10-28 07:53_

---

_Closed by @MichaReiser on 2024-10-28 07:53_

---

_Branch deleted on 2024-10-28 07:53_

---
