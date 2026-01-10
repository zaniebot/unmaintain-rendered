```yaml
number: 14885
title: "Update docs for `eq-without-hash`"
type: pull_request
state: merged
author: KotlinIsland
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-eq_without_hash
created_at: 2024-12-10T01:14:37Z
updated_at: 2024-12-20T08:37:02Z
url: https://github.com/astral-sh/ruff/pull/14885
synced_at: 2026-01-10T20:42:27Z
```

# Update docs for `eq-without-hash`

---

_Pull request opened by @KotlinIsland on 2024-12-10 01:14_

## Summary

resolves #14883

This PR removes the known limitation section in the documentation of `eq-without-hash`. That is not actually a limitation as a subclass overriding the `__eq__` method would have its `__hash__` set to `None` implicitly. The user should explicitly inherit the `__hash__` method from the parent class.

## Test Plan

<img width="619" alt="Screenshot 2024-12-20 at 2 02 47 PM" src="https://github.com/user-attachments/assets/552defcd-25e1-4153-9ab9-e5b9d5fbe8cc" />



---

_Comment by @github-actions[bot] on 2024-12-10 01:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @dhruvmanila on 2024-12-10 04:15_

---

_@dhruvmanila reviewed on 2024-12-10 04:17_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/eq_without_hash.rs`:17 on 2024-12-10 04:17_

I think we should provide an additional example below for the case where a superclass has defined a `__hash__` method and still the subclass is unhashable because Python will not inherit it and provide a link to https://docs.python.org/3/reference/datamodel.html#object.__hash__.

---

_Renamed from "(📚) fix docs for eq-without-hash" to "fix docs for eq-without-hash" by @MichaReiser on 2024-12-10 07:04_

---

_Comment by @MichaReiser on 2024-12-19 12:16_

@KotlinIsland are you planning to follow up on @dhruvmanila's comment or would you prefer if someone from us updates the documentation?

---

_Comment by @KotlinIsland on 2024-12-19 13:50_

sorry, I'm just watching a shark movie. I'll have time in the next two days

---

_Review requested from @dhruvmanila by @KotlinIsland on 2024-12-20 06:35_

---

_@dhruvmanila approved on 2024-12-20 08:28_

---

_Renamed from "fix docs for eq-without-hash" to "Update docs for `eq-without-hash`" by @dhruvmanila on 2024-12-20 08:29_

---

_Merged by @dhruvmanila on 2024-12-20 08:33_

---

_Closed by @dhruvmanila on 2024-12-20 08:33_

---
