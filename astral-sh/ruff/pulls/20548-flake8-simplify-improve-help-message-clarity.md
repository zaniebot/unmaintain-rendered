```yaml
number: 20548
title: "[`flake8-simplify`] Improve help message clarity (`SIM105`)"
type: pull_request
state: merged
author: mgiovani
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: fix/20462-improve-sim105-message
created_at: 2025-09-24T11:37:32Z
updated_at: 2025-09-25T19:22:11Z
url: https://github.com/astral-sh/ruff/pull/20548
synced_at: 2026-01-12T15:57:04Z
```

# [`flake8-simplify`] Improve help message clarity (`SIM105`)

---

_@mgiovani_

## Summary

Improve the SIM105 rule message to prevent user confusion about how to properly use `contextlib.suppress`.

The previous message "Replace with `contextlib.suppress(ValueError)`" was ambiguous and led users to incorrectly use `contextlib.suppress(ValueError)` as a statement inside except blocks instead of replacing the entire try-except-pass block with `with contextlib.suppress(ValueError):`.

This change makes the message more explicit:
- **Before**: `"Use \`contextlib.suppress({exception})\` instead of \`try\`-\`except\`-\`pass\`"`
- **After**: `"Replace \`try\`-\`except\`-\`pass\` block with \`with contextlib.suppress({exception})\`"`

The fix title is also updated to be more specific:
- **Before**: `"Replace with \`contextlib.suppress({exception})\`"`  
- **After**: `"Replace \`try\`-\`except\`-\`pass\` with \`with contextlib.suppress({exception})\`"`

Fixes #20462

## Test Plan

- ✅ All existing SIM105 tests pass with updated snapshots
- ✅ Cargo clippy passes without warnings  
- ✅ Full test suite passes
- ✅ The new messages clearly indicate that the entire try-except-pass block should be replaced with a `with` statement, preventing the misuse described in the issue

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:56 on 2025-09-24 14:34_

Hmm, I think the `message` might actually be okay as it was. This part wasn't included in the snippet in the issue, so I didn't realize it already mentioned try-except-pass. I think I would revert this change and keep your new `fix_title`, just to keep some variety in the phrasing since users will often see both.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:62 on 2025-09-24 14:37_

I don't really love the "with with," but it also makes sense. I kind of wish we could make it even more clear that you need to put the code from `try` in the context manager, but we need to keep this as a single line. Do you think something like this would be helpful or also confusing?


```suggestion
            "Replace `try`-`except`-`pass` with `with contextlib.suppress({exception}): ...`"
```

---

_@ntBre reviewed on 2025-09-24 14:39_

Thank you! Just a couple of nits/ideas about the phrasing, but this looks great overall.

---

_Label `diagnostics` added by @ntBre on 2025-09-24 14:39_

---

_@mgiovani reviewed on 2025-09-25 10:40_

---

_Review comment by @mgiovani on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:56 on 2025-09-25 10:40_

Thank you for the feedback!
You're right. I've reverted the main message back to the original and kept only the `fix_title` improvement, which addresses the core confusion from the issue.

---

_@mgiovani reviewed on 2025-09-25 10:40_

---

_Review comment by @mgiovani on `crates/ruff_linter/src/rules/flake8_simplify/rules/suppressible_exception.rs`:62 on 2025-09-25 10:40_

I agree with the "with with" awkwardness, but I think it's clear.
I've also implemented your idea with the ellipsis, it's a good one to keep it as a single line

---

_@ntBre approved on 2025-09-25 14:35_

This looks great, thank you!

---

_Renamed from "[flake8-simplify] Improve SIM105 message clarity (SIM105)" to "[`flake8-simplify`] Improve help message clarity (`SIM105`)" by @ntBre on 2025-09-25 14:36_

---

_Comment by @github-actions[bot] on 2025-09-25 14:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @ntBre on 2025-09-25 15:19_

---

_Closed by @ntBre on 2025-09-25 15:19_

---

_Branch deleted on 2025-09-25 19:22_

---
