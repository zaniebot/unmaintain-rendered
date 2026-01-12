```yaml
number: 14809
title: "Fix infinite watch loop by ignoring 'uninteresting' watch events"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - ci
assignees: []
merged: true
base: main
head: micha/fix-watch
created_at: 2024-12-06T08:34:46Z
updated_at: 2024-12-06T08:52:15Z
url: https://github.com/astral-sh/ruff/pull/14809
synced_at: 2026-01-12T15:55:49Z
```

# Fix infinite watch loop by ignoring 'uninteresting' watch events

---

_@MichaReiser_

## Summary
Fixes https://github.com/astral-sh/ruff/issues/14807

I suspect that this broke when we updated notify, although I'm not quiet sure how this *ever* worked... 

The problem was that the file watcher didn't skip over `Access` events, but Ruff itself accesses the `pyproject.toml` when checking the project. 
That means, Ruff triggers `Access` events but it also schedules a re-check on every `Access` event... and this goes one forever. 

This PR skips over `Access` and `Other` event. `Access` events are uninteresting because they're only reads, they don't change any file metadata or content. 
The `Other` events should be rare and are mainly to inform about file watcher changes... we don't need those. 

I also added an explicit handling for the `Rescan` event. File watchers emit a `Rescan` event if they failed to capture some file watching changes
and it signals that the program should assume that all files might have changed (the program should do a rescan to *get up to date*).

## Test Plan

I tested that Ruff no longer loops when running `check --watch`. I verified that Ruff rechecks file after making content changes.



---

_Label `bug` added by @MichaReiser on 2024-12-06 08:34_

---

_Label `ci` added by @MichaReiser on 2024-12-06 08:34_

---

_Merged by @MichaReiser on 2024-12-06 08:50_

---

_Closed by @MichaReiser on 2024-12-06 08:50_

---

_Branch deleted on 2024-12-06 08:50_

---

_Comment by @github-actions[bot] on 2024-12-06 08:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
