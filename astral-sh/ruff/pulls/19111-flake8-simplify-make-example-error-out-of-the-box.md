```yaml
number: 19111
title: "[`flake8-simplify`] Make example error out-of-the-box (`SIM116`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-8
created_at: 2025-07-03T01:12:10Z
updated_at: 2025-07-07T21:30:31Z
url: https://github.com/astral-sh/ruff/pull/19111
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-simplify`] Make example error out-of-the-box (`SIM116`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-03 01:12_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [if-else-block-instead-of-dict-lookup (SIM116)](https://docs.astral.sh/ruff/rules/if-else-block-instead-of-dict-lookup/#if-else-block-instead-of-dict-lookup-sim116)'s example error out-of-the-box

[Old example](https://play.ruff.rs/718f17ee-fbe2-4520-97c6-153bc0f4502d)
```py
if x == 1:
    return "Hello"
elif x == 2:
    return "Goodbye"
else:
    return "Goodnight"
```

[New example](https://play.ruff.rs/8a9b47b4-da46-4a50-8576-362cdd707cee)
```py
def find_phrase(x):
    if x == 1:
        return "Hello"
    elif x == 2:
        return "Goodbye"
    elif x == 3:
        return "Good morning"
    else:
        return "Goodnight"
```

The "Use instead" section was also updated to reflect the new case. I also changed it to use an intermediary variable since I find the `return <long dict>.get` very ugly and hard to read.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Renamed from "[`flake8-simplify`] Make example error out-of-the-box (SIM116)" to "[`flake8-simplify`] Make example error out-of-the-box (`SIM116`)" by @MeGaGiGaGon on 2025-07-03 01:13_

---

_Comment by @github-actions[bot] on 2025-07-03 01:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-07-03 14:00_

---

_@ntBre reviewed on 2025-07-03 14:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_lookup.rs`:28 on 2025-07-03 14:03_

Should we wrap these in functions? Otherwise the `return`s should be syntax errors. I was alarmed that we weren't flagging the syntax error, but that semantic error is still associated with the F706 lint rule.

---

_@MeGaGiGaGon reviewed on 2025-07-03 14:50_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_lookup.rs`:28 on 2025-07-03 14:50_

That would probably be a good idea. I ran my tool again looking for that, and I got only this rule and `UP028` as having examples that raised one of F701-F706 (Aside from those rule's examples themselves).

Should the tests also be updated? Since they have the same problem as the example. I also did a ruff run on the tests itself, and found a few other rules that run into these in their tests:
```
crates\ruff_linter\resources\test\fixtures\flake8_bandit\S112.py: F702 `continue` not properly in loop
crates\ruff_linter\resources\test\fixtures\flake8_bugbear\B031.py: F706 `return` statement outside of a function/method
crates\ruff_linter\resources\test\fixtures\flake8_pie\PIE804.py: F704 `yield` statement outside of a function
crates\ruff_linter\resources\test\fixtures\flake8_simplify\SIM115.py: F704 `await` statement outside of a function
crates\ruff_linter\resources\test\fixtures\flake8_simplify\SIM116.py: F706 `return` statement outside of a function/method
crates\ruff_linter\resources\test\fixtures\pycodestyle\E27.py: F704 `yield` statement outside of a function
```

---

_@MeGaGiGaGon reviewed on 2025-07-03 14:55_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_lookup.rs`:28 on 2025-07-03 14:55_

I got a couple more hits adding the "Use instead" sections to the search, `SIM110` and `PLW1501`

---

_@ntBre reviewed on 2025-07-03 14:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/if_else_block_instead_of_dict_lookup.rs`:28 on 2025-07-03 14:55_

Sure, that seems like a good idea! I think the plan is still to make those rules into true `SyntaxError`s eventually, so these snapshots would change then anyway.

---

_Review requested from @ntBre by @MichaReiser on 2025-07-07 13:37_

---

_@ntBre approved on 2025-07-07 21:16_

Thanks!

---

_Merged by @ntBre on 2025-07-07 21:17_

---

_Closed by @ntBre on 2025-07-07 21:17_

---

_Branch deleted on 2025-07-07 21:30_

---
