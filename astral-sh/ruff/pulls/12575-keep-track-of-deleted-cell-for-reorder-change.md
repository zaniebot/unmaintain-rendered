```yaml
number: 12575
title: Keep track of deleted cell for reorder change request
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/deleted-cell-content
created_at: 2024-07-30T09:40:11Z
updated_at: 2024-07-30T10:00:53Z
url: https://github.com/astral-sh/ruff/pull/12575
synced_at: 2026-01-10T21:47:02Z
```

# Keep track of deleted cell for reorder change request

---

_Pull request opened by @dhruvmanila on 2024-07-30 09:40_

## Summary

This PR fixes a bug where the server wouldn't retain the cell content in case of a reorder change request.

As mentioned in https://github.com/astral-sh/ruff/issues/12573#issuecomment-2257819298, this change request is modeled as (a) remove these cell URIs and (b) add these cell URIs. The cell content isn't provided. But, the way we've modeled the `NotebookCell` (it contains the underlying `TextDocument`), we need to keep track of the deleted cells to get the content.

This is not an ideal solution and a better long term solution would be to model it as per the spec but that is a big structural change and will affect multiple parts of the server. Modeling as per the spec would also avoid bugs like https://github.com/astral-sh/ruff/pull/11864. For context, that model would add complexity per https://github.com/astral-sh/ruff/pull/11206#discussion_r1600165481.

fixes: #12573

## Test Plan

This video shows the before and after the bug is fixed:

https://github.com/user-attachments/assets/2fcad4b5-f9af-4776-8640-4cd1fa16e325


---

_Label `bug` added by @dhruvmanila on 2024-07-30 09:40_

---

_@MichaReiser approved on 2024-07-30 09:42_

---

_Comment by @MichaReiser on 2024-07-30 09:42_

Uh that's a nasty bug. Nice find!

---

_Merged by @dhruvmanila on 2024-07-30 09:51_

---

_Closed by @dhruvmanila on 2024-07-30 09:51_

---

_Branch deleted on 2024-07-30 09:51_

---

_Comment by @github-actions[bot] on 2024-07-30 10:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
