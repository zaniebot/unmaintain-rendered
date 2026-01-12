```yaml
number: 15222
title: "[`flake8-type-checking`] Add a fix for `runtime-string-union` (`TC010`)"
type: pull_request
state: open
author: Daverball
labels:
  - fixes
assignees: []
base: main
head: feat/tc010-fix
created_at: 2025-01-02T15:07:22Z
updated_at: 2025-06-25T09:01:20Z
url: https://github.com/astral-sh/ruff/pull/15222
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-type-checking`] Add a fix for `runtime-string-union` (`TC010`)

---

_@Daverball_

## Summary

This adds a fix for `TC010` which removes quotes whenever they can be removed safely and otherwise extends the quotes to the entire union.

This also disables `TC010` explicitly in stubs, rather than rely on the execution context, which can still be runtime in stub files for a small set of type expressions.

## Test Plan

`cargo nextest run`


---

_Comment by @Daverball on 2025-01-02 15:10_

As a side note: I'm not quite sure why some of these fixes are marked as unsafe, even though the union expression doesn't overlap a comment. Do we perhaps have a off-by-one error here?

---

_Comment by @github-actions[bot] on 2025-01-02 15:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@Daverball reviewed on 2025-01-08 13:59_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:280 on 2025-01-08 13:59_

While there is some code duplication between this and `quotes_are_unremovable`, this version is simplified for lookups from a runtime context and needs to use `lookup_symbol` instead of `resolve_name` since the forward reference has not been traversed yet by the checker.

So I'm torn about whether to merge this with `quotes_are_unremovable` into a common function in `helpers.rs` and leveraging an enum with two variants to determine whether we can use `resolve_name` or need to use `lookup_symbol` instead. Alternatively we could try passing in a closure. But both approaches seem kind of messy to me.

We could also say we're fine with the additional overhead of `lookup_symbol` over `resolve_name` and just always use that.

---

_@Daverball reviewed on 2025-01-14 12:59_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:183 on 2025-01-14 12:59_

I'm not stoked about having to do this and not taking advantage of the cache on the checker, but I don't see a good way to avoid this without moving analysis further down the pipeline when traversing the parsed forward reference and checking for a union in the parent expression.

The only problem with that is that we would potentially generate the same fix for neighboring violations, without them being explicitly grouped together, but maybe we can get around that by setting the fix's parent to the location of the expression we're extending the quotes to.

The other issue with moving the analysis to a different stage is that we're potentially destabilizing a stable rule in doing so and potentially emitting incorrect violations in nested forward references like `A | "B | 'C'"`, unless we add additional semantic state so we can detect if we're inside a nested forward reference.

That being said, it still might be the correct decision, in order to reduce some of the complexity.

---

_@Daverball reviewed on 2025-01-14 13:03_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_string_union.rs`:183 on 2025-01-14 13:03_

If we go with changing where the analysis happens, we at least have a straightforward way to keep the old implementation while the new one is in preview. Each version of the function can just check whether preview is enabled or not.

We may want to only enable the fix in preview anyways, regardless of how we choose to implement this.

---

_Marked ready for review by @Daverball on 2025-01-14 13:03_

---

_Comment by @Daverball on 2025-01-14 13:15_

@MichaReiser @AlexWaygood I'm taking this out of draft. Depending on your responses to my review comments this may either only need small changes or larger changes where experimenting in a separate PR would make more sense, so we can compare the two approaches.

---

_Label `fixes` added by @MichaReiser on 2025-01-14 13:31_

---

_Comment by @MichaReiser on 2025-01-15 13:39_

Thanks @Daverball. This PR is on my to-do list, but it might be some time before I get to it.

---
