```yaml
number: 15016
title: "chore(ci): pin some lingering actions by hash"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/ci-fixes
created_at: 2025-08-01T18:47:53Z
updated_at: 2025-08-01T19:09:31Z
url: https://github.com/astral-sh/uv/pull/15016
synced_at: 2026-01-10T06:53:02Z
```

# chore(ci): pin some lingering actions by hash

---

_Pull request opened by @woodruffw on 2025-08-01 18:47_

This is the first of several burndown PRs for CI.

I did this automatically with `pinact run -v`
and then cross-checked with `zizmor`'s
impostor commit audit.

## Summary

This takes our remaining ref-pinned GitHub Actions usage and hash-pins them. In other words, `@v1` becomes `@feedfacefeedface...`.

Doing this makes our action usage de facto immutable, at least at the repository state level -- the actions themselves might still be non-idempotent/hermetic/reproducible, but the repository state itself can't change like it could before with a tag overwrite.

## Test Plan

This should be a non-functional change. However, the only way to really confirm that is to run the CI and see what happens 🙂 

---

_Review requested from @zanieb by @woodruffw on 2025-08-01 18:47_

---

_Review requested from @konstin by @woodruffw on 2025-08-01 18:47_

---

_Assigned to @woodruffw by @woodruffw on 2025-08-01 18:47_

---

_@zanieb approved on 2025-08-01 18:49_

Thank you!

---

_Label `internal` added by @zanieb on 2025-08-01 18:49_

---

_Merged by @woodruffw on 2025-08-01 19:09_

---

_Closed by @woodruffw on 2025-08-01 19:09_

---

_Branch deleted on 2025-08-01 19:09_

---
