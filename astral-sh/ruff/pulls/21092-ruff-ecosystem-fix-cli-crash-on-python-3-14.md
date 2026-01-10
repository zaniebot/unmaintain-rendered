```yaml
number: 21092
title: "[`ruff-ecosystem`] Fix CLI crash on Python 3.14"
type: pull_request
state: merged
author: danparizher
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-20965
created_at: 2025-10-27T01:34:15Z
updated_at: 2025-10-29T21:53:37Z
url: https://github.com/astral-sh/ruff/pull/21092
synced_at: 2026-01-10T16:59:49Z
```

# [`ruff-ecosystem`] Fix CLI crash on Python 3.14

---

_Pull request opened by @danparizher on 2025-10-27 01:34_

## Summary

Fixed the Python 3.14 crash in `ruff-ecosystem` by updating event loop handling.

Fixes #20965

### Changes Made

**1. `python/ruff-ecosystem/ruff_ecosystem/cli.py` (line 90-91)**
- Replaced `asyncio.get_event_loop()` with `asyncio.new_event_loop()` and `asyncio.set_event_loop(loop)`

**2. `scripts/check_ecosystem.py` (line 531-532)**
- Applied the same update

### Why This Works

In Python 3.14, `asyncio.get_event_loop()` raises when no loop exists. Creating the loop explicitly ensures:
- Python 3.10+ support
- Signal handler behavior preserved
- Loop is closed in the finally block


---

_Review requested from @zanieb by @charliermarsh on 2025-10-27 01:42_

---

_Comment by @github-actions[bot] on 2025-10-27 01:44_

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

_Label `internal` added by @AlexWaygood on 2025-10-27 06:53_

---

_Review requested from @amyreese by @MichaReiser on 2025-10-27 07:38_

---

_@AlexWaygood reviewed on 2025-10-27 11:06_

Please can you also make this change to our workflow file, if this fixes the crash:

```diff
diff --git a/.github/workflows/ci.yaml b/.github/workflows/ci.yaml
index c57773078f..131f76acad 100644
--- a/.github/workflows/ci.yaml
+++ b/.github/workflows/ci.yaml
@@ -531,8 +531,7 @@ jobs:
           persist-credentials: false
       - uses: astral-sh/setup-uv@d0cc045d04ccac9d8b7881df0226f9e82c39688e # v6.8.0
         with:
-          # TODO: figure out why `ruff-ecosystem` crashes on Python 3.14
-          python-version: "3.13"
+          python-version: ${{ env.PYTHON_VERSION }}
           activate-environment: true
 
       - uses: actions/download-artifact@634f93cb2916e3fdff6788551b99b062d0335ce0 # v5.0.0
```

---

_@MichaReiser approved on 2025-10-29 21:37_

Thank you. 

---

_Merged by @MichaReiser on 2025-10-29 21:37_

---

_Closed by @MichaReiser on 2025-10-29 21:37_

---

_Branch deleted on 2025-10-29 21:53_

---
