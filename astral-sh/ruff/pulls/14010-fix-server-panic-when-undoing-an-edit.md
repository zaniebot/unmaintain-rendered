```yaml
number: 14010
title: Fix server panic when undoing an edit
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: micha/fix-server-panic
created_at: 2024-10-31T08:36:29Z
updated_at: 2024-11-01T07:16:55Z
url: https://github.com/astral-sh/ruff/pull/14010
synced_at: 2026-01-10T20:59:37Z
```

# Fix server panic when undoing an edit

---

_Pull request opened by @MichaReiser on 2024-10-31 08:36_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13674 and possibly https://github.com/astral-sh/ruff-vscode/issues/607

The `TextDocument::apply_changes` contained some naive logic to reuse the `LineIndex` if it happened that the content didn't change. 
Hower, that logic was incorrect because it compared the `new_content` with the **original** document content rather than the content
from the last iteration. This resulted in a stale `LineIndex` when an edit reverts the content to the **original** content. 

I decided to remove the optimization alltogether because it's too naive to be useful in practice. We could apply a logic similar
to Biome's, we avoid recomputing the LineIndex when the previous edits only changed lines further down in the document. [source](https://github.com/biomejs/biome/blob/3401663efea3a3c45a7d9240d72b92c981285fb2/crates/biome_lsp/src/utils.rs#L360-L363)
However, I don't think that this is a big perf bottlneck. Computing a `LineIndex` is relatively cheap.

## Test Plan

I added a unit test


---

_Label `bug` added by @MichaReiser on 2024-10-31 08:36_

---

_Label `server` added by @MichaReiser on 2024-10-31 08:36_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-10-31 08:36_

---

_Comment by @github-actions[bot] on 2024-10-31 08:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-11-01 06:53_

Nice! Thanks for looking into it

---

_Merged by @MichaReiser on 2024-11-01 07:16_

---

_Closed by @MichaReiser on 2024-11-01 07:16_

---

_Branch deleted on 2024-11-01 07:16_

---
