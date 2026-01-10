```yaml
number: 18311
title: "[`flake8-pyi`] Fix `custom-typevar-for-self` with string annotations (`PYI019`)"
type: pull_request
state: merged
author: close2code-palm
labels:
  - rule
assignees: []
merged: true
base: main
head: fix/custom_tv_for_self_string
created_at: 2025-05-26T06:41:06Z
updated_at: 2025-06-16T14:47:17Z
url: https://github.com/astral-sh/ruff/pull/18311
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-pyi`] Fix `custom-typevar-for-self` with string annotations (`PYI019`)

---

_Pull request opened by @close2code-palm on 2025-05-26 06:41_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
<!-- What's the purpose of the change? What does it do, and why? -->

Solves #18257 

## Test Plan

<!-- How was it tested? -->
Snapshots updated with some cases (negative, positive, mixed annotations).

---

_Review requested from @AlexWaygood by @close2code-palm on 2025-05-26 06:41_

---

_Label `rule` added by @ntBre on 2025-05-28 12:44_

---

_Comment by @github-actions[bot] on 2025-05-28 12:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Fix of custom typevar for self with string annotations" to "[flake8-pyi] fix of custom typevar for self with string annotations (PYI019)" by @close2code-palm on 2025-05-29 20:31_

---

_Comment by @close2code-palm on 2025-05-29 20:37_

@MichaReiser Hi! My mate adviced me to request review from you, can I?)

---

_Comment by @ntBre on 2025-05-29 22:31_

I'm planning to review this, just haven't had a chance yet!

---

_Review requested from @ntBre by @ntBre on 2025-05-29 22:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:161 on 2025-06-13 16:10_

I have two suggestions here:

```suggestion
    let self_or_cls_annotation = match self_or_cls_annotation_unchecked {
        ast::Expr::StringLiteral(literal_expr) => {
            let Ok(parsed_expr) = checker.parse_type_annotation(literal_expr) else {
                return;
            };
            parsed_expr.expression()
        }
        _ => self_or_cls_annotation_unchecked,
    };
```

One is just to simplify the `as_string_literal_expr`, which can be combined with the `match` arm itself, and the second is that the original code didn't restrict the `Expr` to a `Subscript` or `Name`, so I don't think we should do that here either. Instead any other `Expr` should be allowed, as before, with the wildcard arm (`_`).

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI019_PYI019_0.py.snap`:709 on 2025-06-13 16:15_

We may also need to adjust the replacement range. I don't think we want to replace `"_S"` with `"Self"`. I think it should just be an unquoted `Self`.

---

_@ntBre reviewed on 2025-06-13 18:16_

Thanks! This is looking good, just a couple of suggestions.

---

_Review comment by @close2code-palm on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI019_PYI019_0.py.snap`:709 on 2025-06-14 13:09_

I tried to look around for the right way how to extend this behaviour but failed even with ugly approach. So, do we need this, as some other rule applies a fix to this moment right after, we just see "Self" in tests only related to this rule? I can't disagree that rule should do this on its own, but it will require more code changes and help from others or more time.

---

_@close2code-palm reviewed on 2025-06-14 13:09_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI019_PYI019_0.py.snap`:709 on 2025-06-14 15:39_

Okay, you may be right. I looked at this a bit and didn't see a good solution either. I was afraid that we might be adding an unused `Self` import if all of the `Self` instances were quoted, but that doesn't appear to be the case: https://play.ruff.rs/014e3efe-829c-42b9-af40-f3527508bc57 (no import warnings with `ALL` rules selected). So I'm happy moving forward with the current version. Thanks!

---

_@ntBre reviewed on 2025-06-14 15:39_

---

_@ntBre approved on 2025-06-14 15:39_

Thank you!

---

_Renamed from "[flake8-pyi] fix of custom typevar for self with string annotations (PYI019)" to "[`flake8-pyi`] Fix `custom-typevar-for-self` with string annotations (`PYI019`)" by @ntBre on 2025-06-14 15:42_

---

_Merged by @ntBre on 2025-06-16 14:47_

---

_Closed by @ntBre on 2025-06-16 14:47_

---
