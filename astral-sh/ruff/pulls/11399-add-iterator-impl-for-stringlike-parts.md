```yaml
number: 11399
title: "Add `Iterator` impl for `StringLike` parts"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/string-like-part-iter
created_at: 2024-05-13T08:23:24Z
updated_at: 2024-05-13T16:02:57Z
url: https://github.com/astral-sh/ruff/pull/11399
synced_at: 2026-01-12T15:55:37Z
```

# Add `Iterator` impl for `StringLike` parts

---

_@dhruvmanila_

## Summary

This PR adds support to iterate over each part of a string-like expression.

This similar to the one in the formatter:

https://github.com/astral-sh/ruff/blob/128414cd9507adc45584189f025363793dd7cd94/crates/ruff_python_formatter/src/string/any.rs#L121-L125

Although I don't think it's a 1-1 replacement in the formatter because the one implemented in the formatter has another information for certain variants (as can be seen for `FString`).

The main motivation for this is to avoid duplication for rules which work only on the parts of the string and doesn't require any information from the parent node. Here, the parent node being the expression node which could be an implicitly concatenated string.

This PR also updates certain rule implementation to make use of this and avoids logic duplication.


---

_Label `internal` added by @dhruvmanila on 2024-05-13 08:23_

---

_Comment by @github-actions[bot] on 2024-05-13 08:42_

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

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-13 08:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs`:460 on 2024-05-13 13:49_

nit:

```suggestion
    let ranges: Vec<_> = string_like
        .parts()
        .map(|part| part.range())
        .collect();
```

---

_@AlexWaygood approved on 2024-05-13 13:50_

---

_Merged by @dhruvmanila on 2024-05-13 15:52_

---

_Closed by @dhruvmanila on 2024-05-13 15:52_

---

_Branch deleted on 2024-05-13 15:52_

---
