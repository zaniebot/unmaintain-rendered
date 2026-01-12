```yaml
number: 8444
title: "Account for selector specificity when merging `extend_unsafe_fixes` and `override extend_safe_fixes`"
type: pull_request
state: merged
author: lukaspiatkowski
labels:
  - configuration
assignees: []
merged: true
base: main
head: lukas/safe-unsafe-opts-specificity
created_at: 2023-11-02T10:30:42Z
updated_at: 2023-11-07T16:36:58Z
url: https://github.com/astral-sh/ruff/pull/8444
synced_at: 2026-01-12T15:55:26Z
```

# Account for selector specificity when merging `extend_unsafe_fixes` and `override extend_safe_fixes`

---

_@lukaspiatkowski_

## Summary

Prior to this change `extend_unsafe_fixes` took precedence over `extend_safe_fixes` selectors, so any conflicts were resolved in favour of `extend_unsafe_fixes`. Thanks to that ruff were conservatively assuming that if configs conlict the fix corresponding to selected rule will be treated as unsafe.

After this change we take into account Specificity of the selectors. For conflicts between selectors of the same Specificity we will treat the corresponding fixes as unsafe. But if the conflicting selectors are of different specificity the more specific one will win.

## Test Plan

Tests were added for the `FixSafetyTable` struct. The `check_extend_unsafe_fixes_conflict_with_extend_safe_fixes_by_specificity` integration test was added to test conflicting rules of different specificity.

Fixes #8404


---

_Review comment by @zanieb on `crates/ruff_linter/src/settings/mod.rs`:48 on 2023-11-02 16:02_

I think I have a preference for something like `fix_safety` i.e. we don't include `table` for the `RuleTable` field

---

_Review comment by @zanieb on `crates/ruff_linter/src/linter.rs`:295 on 2023-11-02 16:06_

This feels kind of awkward, could the `FixSafetyTable` provide a `resolve_applicability(Rule, Applicability) -> Applicability` method instead? Then we could do:

```
diagnostic.set_fix(fix.with_applicability(fix_safety.resolve_applicability(rule, applicability))
```

and push this logic into the safety table

---

_Review comment by @zanieb on `crates/ruff_linter/src/settings/fix_safety_table.rs`:26 on 2023-11-02 16:07_

I'd rather not add another enum, I think this may be removable with my suggestion to provide a `resolve_applicability` interface?

---

_Review comment by @zanieb on `crates/ruff_linter/src/settings/fix_safety_table.rs`:52 on 2023-11-02 16:08_

I worry that these names could be confused with our applicability ones — is there a way to do this without defining this temporary enum?

---

_@zanieb reviewed on 2023-11-02 16:09_

Thank you for contributing! I have some questions about simplifying the implementation but otherwise this looks great.

---

_Renamed from "Change extend_unsafe_fixes to override extend_safe_fixes per Specificity" to "Account for selector specificity when merging `extend_unsafe_fixes` and `override extend_safe_fixes`" by @zanieb on 2023-11-02 16:11_

---

_Label `configuration` added by @zanieb on 2023-11-02 16:11_

---

_Review comment by @lukaspiatkowski on `crates/ruff_linter/src/settings/fix_safety_table.rs`:52 on 2023-11-02 18:11_

We could use boolean. Not a great fan of that as I think it will make this less readable. Your call.

---

_@lukaspiatkowski reviewed on 2023-11-02 18:11_

---

_Comment by @github-actions[bot] on 2023-11-02 20:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @lukaspiatkowski on 2023-11-04 09:07_

I was going to ask if there is a chance this will make it to the next release, but I see the release happened yesterday :) is this good to go as is @zanieb?

---

_@zanieb reviewed on 2023-11-07 16:18_

---

_Review comment by @zanieb on `crates/ruff_linter/src/settings/fix_safety_table.rs`:52 on 2023-11-07 16:18_

I switched back to the enum, I agree it's clearer. I removed the `use` though.

---

_Comment by @zanieb on 2023-11-07 16:18_

Sorry for the delay this one got lost!

---

_@zanieb approved on 2023-11-07 16:19_

---

_Comment by @lukaspiatkowski on 2023-11-07 16:33_

LGTM

---

_Merged by @zanieb on 2023-11-07 16:33_

---

_Closed by @zanieb on 2023-11-07 16:33_

---
