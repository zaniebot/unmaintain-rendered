```yaml
number: 21481
title: Fix analyze graph tests on windows
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/fix-analyze-test-windows
created_at: 2025-11-16T17:56:25Z
updated_at: 2025-11-16T18:27:09Z
url: https://github.com/astral-sh/ruff/pull/21481
synced_at: 2026-01-12T15:57:25Z
```

# Fix analyze graph tests on windows

---

_@MichaReiser_

It would be nice if adding a filter to every `CliTest` wouldn't require the `AnalyzeTest` wrapper but this works... 

I don't want to apply this filter globally as it replaces `\` in places where they aren't paths.

---

_Label `testing` added by @MichaReiser on 2025-11-16 17:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-16 18:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @MichaReiser on 2025-11-16 18:27_

---

_Merged by @MichaReiser on 2025-11-16 18:27_

---

_Closed by @MichaReiser on 2025-11-16 18:27_

---

_Branch deleted on 2025-11-16 18:27_

---
