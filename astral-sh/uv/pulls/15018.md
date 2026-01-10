```yaml
number: 15018
title: "chore(ci): address findings in publish-docs workflow"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/publish-docs-fixes
created_at: 2025-08-01T19:22:53Z
updated_at: 2025-08-01T20:11:00Z
url: https://github.com/astral-sh/uv/pull/15018
synced_at: 2026-01-10T06:53:02Z
```

# chore(ci): address findings in publish-docs workflow

---

_Pull request opened by @woodruffw on 2025-08-01 19:22_

This addresses all findings in `publish-docs.yml`.

## Summary

Key changes:

* The workflow now runs with no latent permissions. This should be OK, since it looks like any permissioned actions the workflow does require an explicit secret anyways.
* I've intermediated all template expansions with appropriate environment variables. `VERSION` in particular is now defined at the job level, since it doesn't vary.
* I've disabled credential persistence in `actions/checkout`.

## Test Plan

Like #15016, we need to run and see what happens. It might also make sense to manually trigger a docs deploy afterwards.

---

_Review requested from @zanieb by @woodruffw on 2025-08-01 19:22_

---

_Assigned to @woodruffw by @woodruffw on 2025-08-01 19:22_

---

_Label `internal` added by @woodruffw on 2025-08-01 19:22_

---

_@zanieb approved on 2025-08-01 19:31_

---

_Comment by @zanieb on 2025-08-01 19:32_

You should be able to run the docs deploy job before merging.

---

_Comment by @woodruffw on 2025-08-01 19:43_

Triggered manually; it deploys correctly but with a suboptimal `VERSION=` when manually dispatched. I think that behavior was pre-existing though.

(That has only a cosmetic effect on the commit message, doesn't appear to affect any actual built doc state.)

---

_Merged by @woodruffw on 2025-08-01 20:10_

---

_Closed by @woodruffw on 2025-08-01 20:10_

---

_Branch deleted on 2025-08-01 20:11_

---
