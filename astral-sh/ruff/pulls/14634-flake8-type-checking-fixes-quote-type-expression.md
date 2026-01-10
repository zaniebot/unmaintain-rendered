```yaml
number: 14634
title: "[`flake8-type-checking`] Fixes `quote_type_expression`"
type: pull_request
state: merged
author: Daverball
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: bugfix/quote-annotation-transformer
created_at: 2024-11-27T13:48:45Z
updated_at: 2024-11-27T21:18:39Z
url: https://github.com/astral-sh/ruff/pull/14634
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-type-checking`] Fixes `quote_type_expression`

---

_Pull request opened by @Daverball on 2024-11-27 13:48_

Supersedes: #14614
Fixes: #14538
Fixes: #14554

## Summary

This achieves a more feature complete version of wrapping annotations in quotes while removing nested forward references and handling nested quotes correctly.

It's not currently making use of the parsed annotations cache. I tried to pass around `Checker` and `ParsedAnnotationsCache`, either approach caused a cascade of borrow checker errors related to lifetimes I wasn't able to resolve.

This is not yet trying to force the outermost quotes of the quoted type expression to match our preference, so there are some scenarios where `Generator` will pick the opposite style.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-11-27 13:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Daverball on 2024-11-27 14:31_

@MichaReiser This is looking pretty good. Do you want to handle the changes to `Generator` to allow forcing the quote preference in a separate PR or should I do it as part of this one?

---

_Comment by @MichaReiser on 2024-11-27 14:35_

> @MichaReiser This is looking pretty good. Do you want to handle the changes to `Generator` to allow forcing the quote preference in a separate PR or should I do it as part of this one?

Glad to hear that! I would prefer a separate PR, unless it requires extra work from your side

---

_Marked ready for review by @Daverball on 2024-11-27 14:36_

---

_Renamed from "[`flake8-type-checking`] Fixes quote_type_expression" to "[`flake8-type-checking`] Fixes `quote_type_expression`" by @Daverball on 2024-11-27 16:19_

---

_Label `bug` added by @dylwil3 on 2024-11-27 16:50_

---

_Label `fixes` added by @dylwil3 on 2024-11-27 16:50_

---

_@MichaReiser approved on 2024-11-27 17:58_

This is great

---

_Merged by @MichaReiser on 2024-11-27 17:58_

---

_Closed by @MichaReiser on 2024-11-27 17:58_

---

_Branch deleted on 2024-11-27 21:18_

---
