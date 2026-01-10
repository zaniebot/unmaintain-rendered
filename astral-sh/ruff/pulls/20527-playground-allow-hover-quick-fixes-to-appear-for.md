```yaml
number: 20527
title: "[`playground`] Allow hover quick fixes to appear for overlapping diagnostics"
type: pull_request
state: merged
author: danparizher
labels:
  - playground
assignees: []
merged: true
base: main
head: fix-19655
created_at: 2025-09-23T02:51:14Z
updated_at: 2025-09-23T13:15:44Z
url: https://github.com/astral-sh/ruff/pull/20527
synced_at: 2026-01-10T17:40:28Z
```

# [`playground`] Allow hover quick fixes to appear for overlapping diagnostics

---

_Pull request opened by @danparizher on 2025-09-23 02:51_

## Summary

Fixes #19655

- Switched quick-fix filtering from line-equality to proper range intersection to show all relevant fixes for the hovered range.
- Set `code` on markers with `diagnostic.code ?? undefined` to satisfy Monaco’s `IMarkerData` type and ensure the rule code shows up without type errors.

---

_@MichaReiser reviewed on 2025-09-23 08:36_

---

_Review comment by @MichaReiser on `playground/ruff/src/Editor/SourceEditor.tsx`:167 on 2025-09-23 08:36_

Can we use `range.containsRange(other_range)` here instead (or the other way round or any other range method)

---

_@MichaReiser reviewed on 2025-09-23 08:37_

Thank you. This makes sense to me, but I'm wondering if there's any `Range` method that we could use instead of rolling our own "intersects" implementation

---

_Label `playground` added by @MichaReiser on 2025-09-23 08:37_

---

_Comment by @danparizher on 2025-09-23 13:07_

I did some digging and found `Range.areIntersecting()` which is perfect for this case.

---

_Review requested from @MichaReiser by @danparizher on 2025-09-23 13:08_

---

_@MichaReiser approved on 2025-09-23 13:14_

---

_Merged by @MichaReiser on 2025-09-23 13:15_

---

_Closed by @MichaReiser on 2025-09-23 13:15_

---

_Branch deleted on 2025-09-23 13:15_

---
