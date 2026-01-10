```yaml
number: 13391
title: "feat(docker): set default `UV_TOOL_BIN_DIR` on docker images"
type: pull_request
state: merged
author: samypr100
labels:
  - breaking
assignees: []
merged: true
base: release/080
head: docker-default-uv-tool-bin-dir
created_at: 2025-05-12T01:47:13Z
updated_at: 2025-08-04T03:26:55Z
url: https://github.com/astral-sh/uv/pull/13391
synced_at: 2026-01-10T06:44:32Z
```

# feat(docker): set default `UV_TOOL_BIN_DIR` on docker images

---

_Pull request opened by @samypr100 on 2025-05-12 01:47_

## Summary

Closes #13057

Sets `UV_TOOL_BIN_DIR` to `/usr/local/bin` for all derived images to allow `uv tool install` to work out of the box.

Note, when the default image user is overwritten (e.g. `USER <UID>`) by a less privileged one, an alternative writable location would now need to be set by downstream consumers to prevent issues, hence I'm labeling this as a breaking change for 0.8.x release.

Relates to https://github.com/astral-sh/uv-docker-example/pull/55

## Test Plan

Each image was tested to work with uv tool with `UV_TOOL_BIN_DIR` set to `/usr/local/bin` with the default root user and alternative non-root users to confirm breaking nature of the change.


---

_Label `breaking` added by @samypr100 on 2025-05-12 01:47_

---

_Review requested from @zanieb by @konstin on 2025-05-12 09:29_

---

_@zanieb approved on 2025-05-12 20:43_

Can you add this to the `uv-docker-example `too?

---

_Comment by @zanieb on 2025-05-12 20:43_

Ah the breaking change is a little unfortunate, I guess we can wait

---

_Added to milestone `v0.8.0` by @zanieb on 2025-05-12 20:43_

---

_Assigned to @zanieb by @konstin on 2025-05-13 07:52_

---

_@zanieb approved on 2025-06-21 19:42_

---

_Merged by @zanieb on 2025-06-21 19:42_

---

_Closed by @zanieb on 2025-06-21 19:42_

---

_Branch deleted on 2025-08-04 03:26_

---
