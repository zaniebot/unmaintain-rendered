```yaml
number: 13036
title: "Add `crates/uv-publish/src/lib.rs` to version patch files"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/upload-request
created_at: 2025-04-21T23:54:49Z
updated_at: 2025-04-22T08:02:04Z
url: https://github.com/astral-sh/uv/pull/13036
synced_at: 2026-01-12T16:10:31Z
```

# Add `crates/uv-publish/src/lib.rs` to version patch files

---

_@zanieb_

See https://github.com/astral-sh/uv/actions/runs/14583501775/job/40904734786?pr=13034

cc @jtfmumm I'm not sure why this changed in https://github.com/astral-sh/uv/pull/12920 but please be careful of snapshots with the uv version. We might want to filter that out.

cc @konstin regarding if there are less brittle ways snapshot here.

---

_Label `internal` added by @zanieb on 2025-04-21 23:54_

---

_Merged by @zanieb on 2025-04-22 01:45_

---

_Closed by @zanieb on 2025-04-22 01:45_

---

_Branch deleted on 2025-04-22 01:45_

---

_Comment by @zanieb on 2025-04-22 03:41_

Awkward, we actually don't support `.rs` files in rooster.

---

_Comment by @konstin on 2025-04-22 08:02_

Yeah we shouldn't snapshot that part of the headers for the publish test, if that doesn't work we can filter the uv version.

---
