```yaml
number: 13731
title: "[`pylint`] - restrict `iteration-over-set` to only work on sets of literals (`PLC0208`)"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: improve-PLC0208
created_at: 2024-10-13T18:50:56Z
updated_at: 2024-10-21T11:23:02Z
url: https://github.com/astral-sh/ruff/pull/13731
synced_at: 2026-01-12T15:55:45Z
```

# [`pylint`] - restrict `iteration-over-set` to only work on sets of literals (`PLC0208`)

---

_@diceroll123_

## Summary

Restricts `iteration-over-set` to only work on sets of literals. There are valid use cases for iterating over a set, for de-duplication purposes.

Fixes #11694 

## Test Plan

`cargo test`


---

_Comment by @diceroll123 on 2024-10-13 18:57_

I'm on the fence about still emitting a diagnostic without a fix even for "intentional" use of a set iteration, please advise. 😄

---

_Comment by @github-actions[bot] on 2024-10-13 19:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2024-10-14 16:08_

---

_Label `preview` added by @MichaReiser on 2024-10-14 16:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/iteration_over_set.rs`:54 on 2024-10-14 16:13_

Technically, this also applies to literals. E.g. it's possible that someone wrote `set(1, 2, 2)` where fixing it to `[1, 2, 2]` would result in a semantic change. 

That's why I'm inclined that we should instead use `ComparableExpr` to test if no two set entries are identical. See e.g. 

https://github.com/astral-sh/ruff/blob/aa0db338d935aa40fafaa05feff7343c42491d8e/crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs#L141

---

_@MichaReiser requested changes on 2024-10-14 16:13_

Limiting the rule to only case where we can prove that the values are unique seems reasonable to me. Wdyt @AlexWaygood 

But I suggest that we use `ComparableExpr` to ensure that the literals are unique.

---

_@diceroll123 reviewed on 2024-10-18 04:03_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/iteration_over_set.rs`:54 on 2024-10-18 04:03_

Rule `B033` catches duplicate set literals, so I've made it so this only emits a diagnostic if the set literals are unique.

The alternative is that we can still emit a diagnostic _without a fix_ if the set literals are not unique. Let me know what you think! 😄 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/iteration_over_set.rs`:64 on 2024-10-18 06:24_

Nit: We can avoid building the hash set and then exiting after we added a few values because there's a non-literal by first testing if all entries are literals

```suggestion
		if set.iter().any(|value| !value.is_literal_expr()) {
			return;
		}
		
		let mut seen_values = FxHashSet::with_capacity(set.len());

    for value in set {
        let comparable_value = ComparableExpr::from(value);
        if !seen_values.insert(comparable_value) {
            // if the set contains a duplicate literal value, early exit.
            // rule `B033` can catch that.
            return;
        }
     }
```

---

_@MichaReiser approved on 2024-10-18 06:25_

LGTM, thank you. I'll let @AlexWaygood chime in to see if he agrees with the behavior change

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-18 06:25_

---

_@diceroll123 reviewed on 2024-10-18 06:40_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/iteration_over_set.rs`:64 on 2024-10-18 06:40_

Good call. Implemented!

---

_Comment by @AlexWaygood on 2024-10-21 11:13_

I guess I'm not a massive fan of this rule in general :/
- Iteration over a list or tuple might be slightly more efficient in theory, but in practice it's a micro-optimisation that I would expect to very rarely make a difference
- Using a `set` rather than a tuple or list can make it clearer to readers of your code that the iteration order doesn't matter, and/or that you expect each element in the iterable to be unique
- Using a `set` rather than a tuple or list means you have deduplication of elements guaranteed, and means that Ruff will check your code with useful lints such as B033. With a tuple or list you could easily make an accidental typo such as `(1, 1, 2, 3)` when you meant `(1, 2, 3)`.

However, all of that's pretty irrelevant to this PR, which I think pretty clearly makes the rule _better_!

---

_@AlexWaygood approved on 2024-10-21 11:13_

Thank you!




---

_Merged by @AlexWaygood on 2024-10-21 11:14_

---

_Closed by @AlexWaygood on 2024-10-21 11:14_

---
