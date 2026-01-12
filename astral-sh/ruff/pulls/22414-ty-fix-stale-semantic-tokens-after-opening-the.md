```yaml
number: 22414
title: "[ty] Fix stale semantic tokens after opening the same document with new content"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/fix-stale-semantic-tokens
created_at: 2026-01-06T09:32:04Z
updated_at: 2026-01-06T10:52:53Z
url: https://github.com/astral-sh/ruff/pull/22414
synced_at: 2026-01-12T15:57:49Z
```

# [ty] Fix stale semantic tokens after opening the same document with new content

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ty/issues/2346
Fixes https://github.com/astral-sh/ty/issues/2353

We never synced the `File::revision` after closing a file, but we should to ensure `File::revision` reverts back to using the file's last modified timestamp. 

## Test Plan

Added E2E test


---

_Review requested from @carljm by @MichaReiser on 2026-01-06 09:32_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-06 09:32_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-06 09:32_

---

_Label `bug` added by @MichaReiser on 2026-01-06 09:32_

---

_Label `server` added by @MichaReiser on 2026-01-06 09:32_

---

_Label `ty` added by @MichaReiser on 2026-01-06 09:32_

---

_Review requested from @dhruvmanila by @MichaReiser on 2026-01-06 09:32_

---

_Comment by @astral-sh-bot[bot] on 2026-01-06 09:42_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@dhruvmanila approved on 2026-01-06 10:51_

Thanks!

I tested out that this also fixes https://github.com/astral-sh/ty/issues/2353.

---

_Merged by @MichaReiser on 2026-01-06 10:52_

---

_Closed by @MichaReiser on 2026-01-06 10:52_

---

_Branch deleted on 2026-01-06 10:52_

---
