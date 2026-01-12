```yaml
number: 14218
title: Bump version to 0.7.14
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: release/0714
created_at: 2025-06-23T15:49:54Z
updated_at: 2025-06-23T18:03:35Z
url: https://github.com/astral-sh/uv/pull/14218
synced_at: 2026-01-12T16:11:05Z
```

# Bump version to 0.7.14

---

_@Gankra_

_No description provided._

---

_Review comment by @konstin on `CHANGELOG.md`:25 on 2025-06-23 16:03_

I'd move this to the top of the enhancements as "Stabilize `--torch-backend`" so it's clear why the rest of the torch-backend items isn't in the preview list.

---

_@konstin approved on 2025-06-23 16:03_

---

_Merged by @Gankra on 2025-06-23 16:48_

---

_Closed by @Gankra on 2025-06-23 16:48_

---

_Branch deleted on 2025-06-23 16:48_

---

_Comment by @notatallshaw on 2025-06-23 18:03_

Very minor, but IMO news item:

> - Make cold resolves about 10% faster ([#14035](https://github.com/astral-sh/uv/pull/14035))

Was a bit misleading, would have been nice to state that it was a regression fix, got excited for a moment there was some new resolution optimization.

---
