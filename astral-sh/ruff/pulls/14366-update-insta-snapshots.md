```yaml
number: 14366
title: Update insta snapshots
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/update-snapshots
created_at: 2024-11-15T18:02:07Z
updated_at: 2024-11-15T18:31:17Z
url: https://github.com/astral-sh/ruff/pull/14366
synced_at: 2026-01-10T20:50:57Z
```

# Update insta snapshots

---

_Pull request opened by @MichaReiser on 2024-11-15 18:02_

## Summary

Cargo insta made updates to its snapshot formats. The new format isn't applied
unless there are other changes to the snapshot. I've found this annoying
when a snapshot test failed, I accepted the changes, and then fixed the regression 
because I then ended up with an "empty" snapshot change that updated the snapshot to the new layout. 

This PR updates all snapshots to use the new insta snapshot layout

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-15 18:02_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-15 18:02_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-15 18:02_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-11-15 18:02_

---

_Label `testing` added by @MichaReiser on 2024-11-15 18:02_

---

_Comment by @github-actions[bot] on 2024-11-15 18:21_

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

_Comment by @dylwil3 on 2024-11-15 18:25_

Biggest diff of the year award! 🏆 

---

_@dylwil3 approved on 2024-11-15 18:26_

---

_Comment by @MichaReiser on 2024-11-15 18:31_

haha :)

---

_Merged by @MichaReiser on 2024-11-15 18:31_

---

_Closed by @MichaReiser on 2024-11-15 18:31_

---

_Branch deleted on 2024-11-15 18:31_

---
