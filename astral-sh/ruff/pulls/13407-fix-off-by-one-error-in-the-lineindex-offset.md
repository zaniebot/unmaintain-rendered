```yaml
number: 13407
title: "Fix off-by one error in the `LineIndex::offset` calculation"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: micha/fix-line-index-offset-calculation
created_at: 2024-09-19T11:11:43Z
updated_at: 2024-09-19T12:13:27Z
url: https://github.com/astral-sh/ruff/pull/13407
synced_at: 2026-01-10T21:30:32Z
```

# Fix off-by one error in the `LineIndex::offset` calculation

---

_Pull request opened by @MichaReiser on 2024-09-19 11:11_

## Summary

This fixes an error where Emacs showed obscure syntax errors.
The reason for the obscure syntax errors was that Ruff incorrectly
applied the change updates from emacs because of an off-by-one error 
in the UTF32 to UTF8 offset calculation. When you add a new type wrapper to make it clear that the offsets are one-based and you still get it wrong :shrug: 

We didn't notice this error before because VS code always uses UTF 16 and 
I think neovim uses UTF8. The same method is also used to detect the format range. 
But it was never significant there because range formatting always extends the range to the enclosing statement. 


Fixes https://github.com/emacs-lsp/lsp-mode/issues/4547
Fixes https://github.com/astral-sh/ruff-lsp/issues/495

and possibly https://github.com/astral-sh/ruff/issues/13392


## Test Plan

Added tests. I used emacs with a debug build that contains the fix and I no longer
observed any stale/obscure syntax errors


---

_Label `server` added by @MichaReiser on 2024-09-19 11:12_

---

_Label `bug` added by @MichaReiser on 2024-09-19 11:12_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-19 11:13_

---

_Comment by @zanieb on 2024-09-19 11:16_

Nice!

---

_Merged by @MichaReiser on 2024-09-19 11:58_

---

_Closed by @MichaReiser on 2024-09-19 11:58_

---

_Branch deleted on 2024-09-19 11:58_

---

_Comment by @github-actions[bot] on 2024-09-19 12:13_

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
