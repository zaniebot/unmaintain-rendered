```yaml
number: 12203
title: Consider the content of the new cells during notebook sync
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/notebook-sync
created_at: 2024-07-05T10:46:07Z
updated_at: 2024-07-05T11:40:01Z
url: https://github.com/astral-sh/ruff/pull/12203
synced_at: 2026-01-12T15:55:40Z
```

# Consider the content of the new cells during notebook sync

---

_@dhruvmanila_

## Summary

This PR fixes the bug where the server was not considering the `cells.structure.didOpen` field to sync up the new content of the newly added cells.

The parameters corresponding to this request provides two fields to get the newly added cells:
1. `cells.structure.array.cells`: This is a list of `NotebookCell` which doesn't contain any cell content. The only useful information from this array is the cell kind and the cell document URI which we use to initialize the new cell in the index.
2. `cells.structure.didOpen`: This is a list of `TextDocumentItem` which corresponds to the newly added cells. This actually contains the text content and the version.

This wasn't a problem before because we initialize the cell with an empty string and this isn't a problem when someone just creates an empty cell. But, when someone copy-pastes a cell, the cell needs to be initialized with the content.

fixes: #12201 

## Test Plan

First, let's see the panic in action:

1. Press <kbd>Esc</kbd> to allow using the keyboard to perform cell actions (move around, copy, paste, etc.)
2. Copy the second cell with <kbd>c</kbd> key
3. Delete the second cell with <kbd>dd</kbd> key
4. Paste the copied cell with <kbd>p</kbd> key

You can see that the content isn't synced up because the `unused-import` for `sys` is still being highlighted but it's being used in the second cell. And, the hover isn't working either. Then, as I start editing the second cell, it panics.

https://github.com/astral-sh/ruff/assets/67177269/fc58364c-c8fc-4c11-a917-71b6dd90c1ef

Now, here's the preview of the fixed version:

https://github.com/astral-sh/ruff/assets/67177269/207872dd-dca6-49ee-8b6e-80435c7ef22e

---

_Label `bug` added by @dhruvmanila on 2024-07-05 10:46_

---

_Comment by @github-actions[bot] on 2024-07-05 10:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-07-05 11:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-05 11:24_

---

_@MichaReiser approved on 2024-07-05 11:35_

The inline comments are very helpful. Thanks for adding them.

---

_Renamed from "Consider the cell content during notebook sync" to "Consider the content of the new cells during notebook sync" by @dhruvmanila on 2024-07-05 11:39_

---

_Merged by @dhruvmanila on 2024-07-05 11:40_

---

_Closed by @dhruvmanila on 2024-07-05 11:40_

---

_Branch deleted on 2024-07-05 11:40_

---
