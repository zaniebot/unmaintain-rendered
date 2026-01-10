```yaml
number: 19851
title: "[`flake8-simplify`] Implement fix for `maxsplit` without separator (`SIM905`)"
type: pull_request
state: merged
author: RazerM
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feature/split-static-string-whitespace-maxsplit
created_at: 2025-08-10T22:23:06Z
updated_at: 2025-08-15T19:18:07Z
url: https://github.com/astral-sh/ruff/pull/19851
synced_at: 2026-01-10T17:52:17Z
```

# [`flake8-simplify`] Implement fix for `maxsplit` without separator (`SIM905`)

---

_Pull request opened by @RazerM on 2025-08-10 22:23_

**Stacked on top of #19849; diff will include that PR until it is merged.**

---

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

As part of #19849, I noticed this fix could be implemented.

## Test Plan

Tests added based on CPython behaviour.


---

_Comment by @github-actions[bot] on 2025-08-10 22:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-08-11 08:14_

Could you rebase your PR now that https://github.com/astral-sh/ruff/pull/19849 is merged. It will make it easier for us to review your changes. Thank you

---

_@ntBre approved on 2025-08-12 16:56_

Thanks, this makes sense to me! 

If we wanted to be conservative we could probably gate this behind preview since it's conceptually close to adding a new safe fix to a stable rule. However, I think it's okay because the fact that the fix was missing before was kind of surprising. And there are no changes in the ecosystem either.

---

_Label `fixes` added by @ntBre on 2025-08-12 16:58_

---

_Renamed from "SIM905: Implement fix for maxsplit without separator" to "[`flake8-simplify`] Implement fix for `maxsplit` without separator (`SIM905`)" by @ntBre on 2025-08-12 16:59_

---

_Comment by @ntBre on 2025-08-12 17:34_

The more I think about this, the more I'm leaning towards using preview to be safe. @MichaReiser do you feel strongly one way or the other? I only found 99 results in a [code search](https://github.com/search?q=%2F%5B%27%22%5D%5C.r%3Fsplit%5C%28maxsplit%3D%5B1-9%5D%2B%5C%29%2F+language%3APython&type=code), so maybe it's quite a niche pattern anyway.

The preview-gate itself would be easy enough, but we also need a new function in `preview.rs` and to add the test case to the flake8-simplify `preview_rules` function, if we go that route.

---

_Comment by @MichaReiser on 2025-08-13 07:04_

I think we have to, according to our versioning policy. See https://github.com/astral-sh/ruff/issues/8074 why we might want to change that.

---

_Comment by @ntBre on 2025-08-13 13:08_

Sounds good, @RazerM do you mind gating this behind preview?

---

_Label `preview` added by @ntBre on 2025-08-13 13:08_

---

_Comment by @RazerM on 2025-08-14 18:56_

@ntBre I think I've done it correctly, but let me know.

---

_@ntBre approved on 2025-08-15 19:17_

Excellent, thank you!

---

_Merged by @ntBre on 2025-08-15 19:18_

---

_Closed by @ntBre on 2025-08-15 19:18_

---
