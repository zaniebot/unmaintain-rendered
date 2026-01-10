```yaml
number: 20628
title: "[`flake8-bandit`] Fix false negative with implicit string concatenation (`S608`)"
type: pull_request
state: open
author: UgoM
labels:
  - rule
assignees: []
base: main
head: fix-s608-with-implicit-concatenation
created_at: 2025-09-29T12:29:02Z
updated_at: 2025-11-25T13:24:33Z
url: https://github.com/astral-sh/ruff/pull/20628
synced_at: 2026-01-10T16:48:01Z
```

# [`flake8-bandit`] Fix false negative with implicit string concatenation (`S608`)

---

_Pull request opened by @UgoM on 2025-09-29 12:29_

# Disclaimer

This is my first PR to Ruff, and I’m still learning Rust. I’ve done my best to follow the contributing guide and the repository’s patterns, though I may have missed some details. I’d be grateful for your patience and feedback to help me improve.

# Summary

The S608 rule (flake8-bandit's hardcoded_sql_expression) was not detecting SQL injection vulnerabilities in expressions that combine implicit string concatenation with binary operations.

# Bug Description

The rule failed to catch SQL injection patterns like:
```
query("select * "
"from \"" + schema + "\"")
```

# Testing

- Added test case query66 that reproduces the vulnerability
-  Verified all existing tests pass

---

_Comment by @dylwil3 on 2025-09-29 13:10_

Thank you! Do you know if there is a linked issue, or did you notice the bug yourself and file the PR? No worries if so, just wanted to double check - I couldn't immediately find a related issue searching for `S608`.

---

_Renamed from "[flake8-bandit] Fix S608 false negative with implicit string concatenation (S608)" to "[`flake8-bandit`] Fix false negative with implicit string concatenation (`S608`)" by @dylwil3 on 2025-09-29 13:10_

---

_Label `rule` added by @dylwil3 on 2025-09-29 13:10_

---

_Review requested from @dylwil3 by @dylwil3 on 2025-09-29 13:11_

---

_Comment by @github-actions[bot] on 2025-09-29 13:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @UgoM on 2025-09-29 13:24_

> Thank you! Do you know if there is a linked issue, or did you notice the bug yourself and file the PR? No worries if so, just wanted to double check - I couldn't immediately find a related issue searching for `S608`.

I noticed the bug myself, found no related issue. The bug looked like a good first issue so I tackled it :) 

---

_Comment by @MichaReiser on 2025-11-21 08:01_

@dylwil3 would you mind taking another look at this PR?

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S608.py`:188 on 2025-11-25 13:14_

Can you add a more complicated example? In particular I'd like to see some examples using `r`-prefixes and f-strings.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:147 on 2025-11-25 13:15_

Nit: can we rename this? I don't think a "binary string" is really a thing. Maybe `evaluate_concatenated_string`? And in the doc comment you can point out that it evaluates both implicit and explicit string concatenation.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs`:162 on 2025-11-25 13:16_

Is this doing the correct thing if one of the operands is an f-string, for example? Also, should we be returning a `Result` and `Err` (or an `Option` as in `is_explicit_concatenation`) sometimes? Should we be re-using or refactoring to use some of the logic in `is_explicit_concatenation`?

---

_@dylwil3 requested changes on 2025-11-25 13:24_

Thanks for finding this and submitting a PR! I agree that there is a bug, but I'm not sure that this solution is correct. Could you increase the test coverage here and see if we're properly handling things like f-string operands and ternary expression operands etc. here?

---
