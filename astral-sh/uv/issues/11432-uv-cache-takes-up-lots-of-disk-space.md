---
number: 11432
title: uv cache takes up lots of disk space
type: issue
state: closed
author: DetachHead
labels:
  - enhancement
assignees: []
created_at: 2025-02-12T03:01:47Z
updated_at: 2025-02-12T04:47:11Z
url: https://github.com/astral-sh/uv/issues/11432
synced_at: 2026-01-10T01:25:05Z
---

# uv cache takes up lots of disk space

---

_Issue opened by @DetachHead on 2025-02-12 03:01_

### Summary

i don't have much space left on my hard drive, and i've noticed that uv's cache seems to be quite large (20-40gb) so i have to manually clear it every so often.

possible solutions:

- when running any uv command that interacts with the cache, detect if the disk is almost full and/or if the cache directory is very large, then either automatically clean it up, or print a warning that suggests the user cleans it themselves by running `uv cache clean` or `uv cache prune`
- limit the amount of cache that's stored relative to the amount of available disk space

### Example

_No response_

---

_Label `enhancement` added by @DetachHead on 2025-02-12 03:01_

---

_Comment by @zanieb on 2025-02-12 04:47_

I think we can track this in https://github.com/astral-sh/uv/issues/5731

---

_Closed by @zanieb on 2025-02-12 04:47_

---
