```yaml
number: 13858
title: "formatter: Introduce `QuoteMetadata`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: micha/refactor-normalize
created_at: 2024-10-21T12:11:41Z
updated_at: 2024-10-21T19:23:51Z
url: https://github.com/astral-sh/ruff/pull/13858
synced_at: 2026-01-12T15:55:45Z
```

# formatter: Introduce `QuoteMetadata`

---

_@MichaReiser_

## Summary

This is a refactor extracted from https://github.com/astral-sh/ruff/pull/13663

Joining implicitly concatenated strings requires merging the quote information from different strings 
to choose the quotation that's best for all string parts and not just one. 

This is why this PR introduces a new `QuoteMetadata` type that stores the quote information in a structured way. 

https://github.com/astral-sh/ruff/pull/13663 will add a `merge` method to `QuoteMetadata` that merges the information 
if two string parts can be merged.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-10-21 12:11_

---

_Label `formatter` added by @MichaReiser on 2024-10-21 12:11_

---

_Comment by @github-actions[bot] on 2024-10-21 12:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-21 12:25_

---

_@dhruvmanila reviewed on 2024-10-21 13:58_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/normalize.rs`:70 on 2024-10-21 13:58_

I'm a bit confused with this diff so just want to make sure it's correct. In the earlier version, the quoting could be anything but if the f-string state is inside an expression element, then it returns the quoting is the condition matches other `Preserve`. In the new version, I see the f-string state is combined with only `CanChange`, does it also cover the `Preserve`?

---

_@dhruvmanila approved on 2024-10-21 14:03_

Thanks for making the change. The diff was a bit harder to read but I understand that because of the indentation changes. I understood how the new code flows and it's same as the earlier one. I only have one confusion but otherwise this looks good.

---

_@MichaReiser reviewed on 2024-10-21 15:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/normalize.rs`:70 on 2024-10-21 15:01_

This should be the same because if the option was `Preserve` in the old code then:

* It returned `Preserve` in the else branch (because `self.quoting` is preserve)
* It returned `Preserve` in the f-string branch because `self.quoting` is preserve)
* It returned `Preserve` in the `if...else` branch because it is hardcoded to `Quoating::Preserve`

---

_Merged by @MichaReiser on 2024-10-21 19:23_

---

_Closed by @MichaReiser on 2024-10-21 19:23_

---

_Branch deleted on 2024-10-21 19:23_

---
