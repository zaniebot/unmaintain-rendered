```yaml
number: 16520
title: Remove bookworm and alpine 3.21 published images
type: pull_request
state: open
author: samypr100
labels:
  - breaking
assignees: []
base: main
head: remove-bookworm-alpine321
created_at: 2025-10-30T18:04:43Z
updated_at: 2025-12-24T13:37:24Z
url: https://github.com/astral-sh/uv/pull/16520
synced_at: 2026-01-12T16:12:18Z
```

# Remove bookworm and alpine 3.21 published images

---

_@samypr100_

## Summary

Follow up to https://github.com/astral-sh/uv/pull/15352

Removes published bookworm, alpine 3.21 images. As a side-effect, python 3.8 support from the published images is dropped as it's no longer supported upstream for trixie, and since alpine release 3.20.

## Test Plan

No functional changes made besides removing tag pointers.

---

_Label `breaking` added by @samypr100 on 2025-10-30 18:04_

---

_Review requested from @zanieb by @samypr100 on 2025-10-30 18:05_

---

_Comment by @samypr100 on 2025-10-30 18:06_

I believe this should alleviate our rate limits as well, but not sure when this should occur release wise, e.g. 0.10.x or 0.11.x+

---

_Added to milestone `v0.10.0` by @samypr100 on 2025-10-30 18:06_

---

_Marked ready for review by @samypr100 on 2025-10-30 18:06_

---
