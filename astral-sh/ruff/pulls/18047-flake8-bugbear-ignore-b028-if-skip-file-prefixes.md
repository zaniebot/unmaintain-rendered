```yaml
number: 18047
title: "[`flake8-bugbear`] Ignore `B028` if `skip_file_prefixes` is present"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-B028
created_at: 2025-05-12T14:45:25Z
updated_at: 2025-05-12T22:07:36Z
url: https://github.com/astral-sh/ruff/pull/18047
synced_at: 2026-01-12T15:56:11Z
```

# [`flake8-bugbear`] Ignore `B028` if `skip_file_prefixes` is present

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #18011

## Test Plan

Snapshot tests.
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-12 14:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:74 on 2025-05-12 17:53_

It looks like `skip_file_prefixes` is a keyword-only argument, so you probably want to use `.find_keyword` here.

---

_@dylwil3 requested changes on 2025-05-12 17:54_

Thanks! Small tweak and it should be good to land

---

_Label `bug` added by @dylwil3 on 2025-05-12 17:54_

---

_Label `rule` added by @dylwil3 on 2025-05-12 17:54_

---

_Review requested from @dylwil3 by @LaBatata101 on 2025-05-12 19:23_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:72 on 2025-05-12 20:00_

I think you have to check that the value is a nonempty tuple of strings. Could you also add a test for if a user directly specifies `skip_file_prefixes = ()` - we _should_ emit the lint in that case, when the stacklevel is not set.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:69 on 2025-05-12 20:01_

Could you add a note, maybe even just quote the documentation from that link, that:
> When prefixes are supplied, stacklevel is implicitly overridden to be `max(2, stacklevel)`
so that we can remember why we did this in the future?

---

_@dylwil3 requested changes on 2025-05-12 20:01_

Almost there!

---

_@dylwil3 reviewed on 2025-05-12 20:03_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:72 on 2025-05-12 20:03_

(If the expression is not a literal tuple of strings, then I think it's okay to skip the lint, even though that might be a false negative).

---

_@LaBatata101 reviewed on 2025-05-12 20:49_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:72 on 2025-05-12 20:49_

I think we need to check if the tuple is nonempty, because if we check for a tuple of strings, this case `warnings.warn("test", skip_file_prefixes=(os.path.dirname(__file__),))` will be flagged.

---

_Review requested from @dylwil3 by @LaBatata101 on 2025-05-12 20:50_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:72 on 2025-05-12 21:24_

Sorry maybe I worded that in a confusing way:

1. If the keyword argument is used, and the value is _not_ a literal tuple of strings, then skip the lint.
2. If the keyword argument is used, and the value _is_ a literal tuple of strings, skip if and only if the tuple is nonempty.

The logic you have in the most recent commit will give a false positive in the case where the user does something like this:

```python
_my_prefixes = ("this","that")
warnings.warn("test", skip_file_prefixes = _my_prefixes)
```

Could you add this as another test case and take another stab at the implementation logic?

---

_@dylwil3 reviewed on 2025-05-12 21:24_

---

_@LaBatata101 reviewed on 2025-05-12 22:04_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/no_explicit_stacklevel.rs`:72 on 2025-05-12 22:04_

I've updated the implementation logic

---

_@dylwil3 approved on 2025-05-12 22:06_

Thank you!!

---

_Merged by @dylwil3 on 2025-05-12 22:06_

---

_Closed by @dylwil3 on 2025-05-12 22:06_

---

_Branch deleted on 2025-05-12 22:07_

---
