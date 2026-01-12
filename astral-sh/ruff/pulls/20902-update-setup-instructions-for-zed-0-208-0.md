```yaml
number: 20902
title: Update setup instructions for Zed 0.208.0+
type: pull_request
state: merged
author: injust
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-10-15T18:12:38Z
updated_at: 2025-10-16T07:39:48Z
url: https://github.com/astral-sh/ruff/pull/20902
synced_at: 2026-01-12T15:57:12Z
```

# Update setup instructions for Zed 0.208.0+

---

_@injust_

## Summary

Add the new config format for Zed 0.208.0+ (see https://redirect.github.com/zed-industries/zed/pull/39375).

I also reordered `source.fixAll.ruff` before `source.organizeImports.ruff` for consistency (since fixes should come before formatting).

## Test Plan

I manually tested each changed config in Zed.

---

_Label `documentation` added by @AlexWaygood on 2025-10-15 18:30_

---

_@MichaReiser reviewed on 2025-10-15 19:19_

---

_Review comment by @MichaReiser on `docs/editors/setup.md`:517 on 2025-10-15 19:19_

Thank you. I think I'd remove the 0.146+ section. Zed auto updates, so that all users should be on very recent zed versions

---

_@MichaReiser approved on 2025-10-16 07:36_

Thank you

---

_Merged by @MichaReiser on 2025-10-16 07:39_

---

_Closed by @MichaReiser on 2025-10-16 07:39_

---
