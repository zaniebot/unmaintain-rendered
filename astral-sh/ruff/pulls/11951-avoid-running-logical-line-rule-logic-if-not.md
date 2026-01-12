```yaml
number: 11951
title: Avoid running logical line rule logic if not enabled
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/logical-line-rules
created_at: 2024-06-20T12:22:17Z
updated_at: 2024-06-20T16:38:42Z
url: https://github.com/astral-sh/ruff/pull/11951
synced_at: 2026-01-12T15:55:39Z
```

# Avoid running logical line rule logic if not enabled

---

_@dhruvmanila_

## Summary

This PR updates the logical line rules entry-point function to only run the logic if any of the rules within that group is enabled.

Although this shouldn't really give any performance improvements, it's better not to do additional work if we can. This is also consistent with how other rules are run.

## Test Plan

`cargo insta test`


---

_Label `internal` added by @dhruvmanila on 2024-06-20 12:22_

---

_Comment by @github-actions[bot] on 2024-06-20 12:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/logical_lines.rs`:62 on 2024-06-20 14:08_

Let's hope the compiler is smart enough to move the check out of the `for` loop :)

---

_@MichaReiser approved on 2024-06-20 14:08_

Thanks for doing this. This might improve performance, it just won't show in our benchmarks because we either run all or no logical line rules.

---

_@dhruvmanila reviewed on 2024-06-20 16:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/logical_lines.rs`:62 on 2024-06-20 16:13_

Yeah, I hope so as well. Do you know how can one get that kind of information?

---

_@dhruvmanila reviewed on 2024-06-20 16:24_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/logical_lines.rs`:62 on 2024-06-20 16:24_

I've decided to collect them out in variables outside the loop similar to `physical_lines.rs`. Thanks for pointing it out.

---

_Merged by @dhruvmanila on 2024-06-20 16:28_

---

_Closed by @dhruvmanila on 2024-06-20 16:28_

---

_Branch deleted on 2024-06-20 16:28_

---
