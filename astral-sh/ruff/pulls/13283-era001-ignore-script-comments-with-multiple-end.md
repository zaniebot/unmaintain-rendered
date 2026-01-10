```yaml
number: 13283
title: "`ERA001`: Ignore script-comments with multiple end-tags"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: micha/fix-script-comment-era001
created_at: 2024-09-08T16:18:06Z
updated_at: 2024-09-09T18:47:41Z
url: https://github.com/astral-sh/ruff/pull/13283
synced_at: 2026-01-10T21:38:32Z
```

# `ERA001`: Ignore script-comments with multiple end-tags

---

_Pull request opened by @MichaReiser on 2024-09-08 16:18_

## Summary

A script-comment according to [PEP 723]() starts with `# /// script` and spans all lines up to `# ///`. 
However, the `´# ///` isn't considered the end-tag if the next line is a valid content line according to PEP 723 (`#` or `#<space>`). 

This PR fixes the handling of script-comments with multipe end-tags. 

Fixes https://github.com/astral-sh/ruff/issues/13278


## Discussion

This PR changes the rule's semantics to not skip over invalid script comments. Any code in invaid-script comments will get flagged.

I'm torn if that's the right behavior. Arguably, detecting invalid-script-comments should be its own rule. However, 
the spec is very cleary about unclosed blocks:

> Unclosed blocks MUST be ignored.


## Test Plan

Added and update tests


---

_Label `rule` added by @MichaReiser on 2024-09-08 16:18_

---

_Label `bug` added by @MichaReiser on 2024-09-08 16:18_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-08 16:18_

---

_Comment by @github-actions[bot] on 2024-09-08 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-09-09 14:00_

We also have an implementation for this in uv. You could consider copying that over, but this is also fine. Thanks!

---

_Merged by @MichaReiser on 2024-09-09 18:47_

---

_Closed by @MichaReiser on 2024-09-09 18:47_

---

_Branch deleted on 2024-09-09 18:47_

---
