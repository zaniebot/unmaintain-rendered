```yaml
number: 16905
title: "[syntax-errors] Irrefutable case pattern before final case"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-irrefutable-match-cases
created_at: 2025-03-21T21:33:56Z
updated_at: 2025-03-26T16:27:20Z
url: https://github.com/astral-sh/ruff/pull/16905
synced_at: 2026-01-12T15:55:59Z
```

# [syntax-errors] Irrefutable case pattern before final case

---

_@ntBre_

Summary
--

Detects irrefutable `match` cases before the final case using a modified version
of the existing `Pattern::is_irrefutable` method from the AST crate. The
modified method helps to retrieve a more precise diagnostic range to match what
Python 3.13 shows in the REPL.

Test Plan
--

New inline tests, as well as some updates to existing tests that had irrefutable
patterns before the last block.


---

_Label `rule` added by @ntBre on 2025-03-21 21:33_

---

_Label `preview` added by @ntBre on 2025-03-21 21:33_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-21 21:33_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-21 21:33_

---

_Comment by @github-actions[bot] on 2025-03-21 21:43_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:116 on 2025-03-24 15:05_

Should we move the match expression in `check_stmt` above now that we have multiple checks? That would also require updating the methods like `irrefutable_match_case` to accept the relevant AST node which needs to be checked like in this case it would accept `StmtMatch` which would also lead to a safer typed API.

---

_@dhruvmanila approved on 2025-03-24 15:13_

Nice, this looks good!

We could improve the error messages as I think there are only two cases where this can occur - name and wildcard pattern. CPython provides the following error messages for the above cases:
* `SyntaxError: name capture 'foo' makes remaining patterns unreachable`
* `SyntaxError: wildcard makes remaining patterns unreachable`

---

_@MichaReiser approved on 2025-03-24 15:27_

---

_Merged by @ntBre on 2025-03-26 16:27_

---

_Closed by @ntBre on 2025-03-26 16:27_

---

_Branch deleted on 2025-03-26 16:27_

---
