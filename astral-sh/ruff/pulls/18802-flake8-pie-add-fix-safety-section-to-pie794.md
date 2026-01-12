```yaml
number: 18802
title: "[`flake8-pie`] Add fix safety section to `PIE794`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-duplicate_class_field_definition
created_at: 2025-06-19T19:59:25Z
updated_at: 2025-06-19T22:16:14Z
url: https://github.com/astral-sh/ruff/pull/18802
synced_at: 2026-01-12T15:56:25Z
```

# [`flake8-pie`] Add fix safety section to `PIE794`

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #15584

This PR adds a fix safety section to `PIE794`

I could not track down when this rule was initially implemented/made unsafe due how old it could be + multiple large refactors to `ruff`.

There is no comment/reasoning in the code given for the unsafety.

Here is a code example demonstrating why it should be unsafe, since removing any of the assignments would change program behavior [playground](https://play.ruff.rs/01004644-4259-4449-a581-5007cd59846a)
```py
class A:
    x = 1
    x = 2
    print(x)

class B:
    x = print(3)
    x = print(4)

class C:
    x = [1,2,3]
    y = x
    x = y[1]
```

## Test Plan

<!-- How was it tested? -->

N/A, no tests affected.

---

_Assigned to @dylwil3 by @MichaReiser on 2025-06-19 20:55_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_pie/rules/duplicate_class_field_definition.rs`:37 on 2025-06-19 21:39_

I think I agree with your reasons, but this simpler one seems more fundamental:

```suggestion
/// This fix is always marked as unsafe since we cannot know
//  for certain which assignment was intended.
```

---

_@dylwil3 requested changes on 2025-06-19 21:39_

Thanks! I think there's a more basic reason for unsafety here.

---

_@dylwil3 approved on 2025-06-19 21:48_

---

_Label `documentation` added by @dylwil3 on 2025-06-19 21:48_

---

_Merged by @dylwil3 on 2025-06-19 21:51_

---

_Closed by @dylwil3 on 2025-06-19 21:51_

---

_Comment by @github-actions[bot] on 2025-06-19 21:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-06-19 22:16_

---
